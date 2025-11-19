# Create Epic - Break Down Large Features into Tasks

Break down a large feature or initiative into multiple smaller, focused tasks with proper dependencies.

**User intent:** "Create an epic for..." or "Break down this feature..."

**Expected input:** A high-level feature description (e.g., "Build user authentication system with OAuth and JWT")

## Before You Start

**IMPORTANT:** Before executing ANY AnyTask operations, use the `_ensure_ready` skill to verify:
1. `anyt` CLI is installed
2. `ANYT_API_KEY` environment variable is set
3. `.anyt/anyt.json` configuration file exists

If pre-checks fail, stop and show the error message. If not initialized, guide user to run `/anytask:init` first.

## Your Process

### Step 1: Investigate and Analyze the Requirement

Before creating any tasks, understand the full scope:

1. **Read the user's request carefully**
   - What is the high-level goal?
   - What are the key features/capabilities needed?
   - Are there any constraints or requirements?

2. **Search the codebase** to understand context:
   - Existing architecture and patterns
   - Similar features already implemented
   - Technology stack and frameworks
   - Testing patterns
   - Database schema

3. **Identify key components and workflows**:
   - Database/schema changes needed
   - Backend services or APIs
   - Frontend components
   - Integration points
   - Testing requirements
   - Documentation needs

4. **Consider implementation order**:
   - What needs to be built first (foundation)?
   - What depends on what?
   - What can be done in parallel?
   - What's the critical path?

### Step 2: Break Down into Tasks

Decompose the epic into smaller, focused tasks that each:
- Can be completed in a single pull request (4-8 hours of work)
- Have a clear, specific objective
- Build logically on previous tasks (express via dependencies)
- Are testable independently
- Follow the existing codebase patterns

**Typical task breakdown pattern:**
1. Database/schema changes (foundation)
2. Core services/business logic
3. API endpoints
4. Frontend components
5. Integration/middleware
6. Testing and documentation

**Each task should have:**
- Clear, specific title
- Structured description (Description, Objectives, Acceptance Criteria, etc.)
- Realistic time estimate (3-8 hours)
- Dependencies on other tasks (if any)

### Step 3: Create Each Task with Dependencies

For each task in the breakdown, follow this process:

#### 3a. Create Task with JSON Output

Use JSON mode to reliably capture the task identifier:

```bash
anyt task add "<clear, specific title>" --json
```

**Parse the JSON response:**
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-42",
    "id": "...",
    "title": "...",
    ...
  }
}
```

Extract and store the `identifier` (e.g., DEV-42) - you'll need it for dependencies.

#### 3b. Update with Structured Description

Add the structured description using HEREDOC format:

```bash
anyt task edit DEV-42 --description "$(cat <<'EOF'
## Description
[What needs to be built - 1-2 sentences]

## Objectives
- [Specific goal 1]
- [Specific goal 2]
- [Specific goal 3]

## Acceptance Criteria
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
[List task identifiers this depends on]
(Or "None" if no dependencies)

## Estimated Effort
[X] hours

## Technical Notes
[Implementation guidance, architectural decisions, gotchas]
EOF
)" --json
```

#### 3c. Set Priority (Optional)

If this task is particularly urgent or low priority:

```bash
anyt task edit DEV-42 --priority 1 --json
```

Priority levels:
- `-2`: Lowest
- `-1`: Low
- `0`: Normal (default)
- `1`: High
- `2`: Highest/Urgent

#### 3d. Add Task Dependencies

**IMPORTANT:** If this task depends on other tasks, add dependencies using:

```bash
anyt task dep add DEV-44 --on DEV-42,DEV-43 --json
```

This means: "Task DEV-44 depends on DEV-42 AND DEV-43 being completed first"

**Dependency Rules:**
- Use comma-separated list for multiple dependencies
- Don't create circular dependencies (A depends on B, B depends on A)
- Foundation tasks (database, core services) typically have no dependencies
- UI/integration tasks typically depend on API/service tasks
- Use `anyt task dep list <identifier> --json` to verify dependencies

**Example dependency flow:**
```bash
# 1. Create foundation task (no dependencies)
anyt task add "Create user schema" --json
# → DEV-42 (no dependencies)

# 2. Create service task (depends on schema)
anyt task add "Implement user service" --json
# → DEV-43
anyt task dep add DEV-43 --on DEV-42 --json

# 3. Create API task (depends on service)
anyt task add "Add user API endpoints" --json
# → DEV-44
anyt task dep add DEV-44 --on DEV-43 --json

# 4. Create UI task (depends on API)
anyt task add "Build user profile UI" --json
# → DEV-45
anyt task dep add DEV-45 --on DEV-44 --json
```

#### 3e. Track Task Identifiers

Keep a list of created task identifiers for the summary. Example:

```
Created tasks:
- DEV-42: Create user schema (4h) [No dependencies]
- DEV-43: Implement user service (5h) [Depends on: DEV-42]
- DEV-44: Add user API endpoints (4h) [Depends on: DEV-43]
- DEV-45: Build user profile UI (6h) [Depends on: DEV-44]
```

### Step 4: Create Epic Summary

After creating all tasks, display a comprehensive summary:

```
✅ Epic created successfully!

Created 7 tasks with total estimated effort: 32 hours

Task breakdown (in dependency order):

1. DEV-42: Create user and session database schema (4h)
   Dependencies: None

2. DEV-43: Implement JWT token service (6h)
   Dependencies: DEV-42

3. DEV-44: Integrate Google OAuth provider (5h)
   Dependencies: DEV-42, DEV-43

4. DEV-45: Create login and logout API endpoints (4h)
   Dependencies: DEV-43

5. DEV-46: Build React login component (6h)
   Dependencies: DEV-44, DEV-45

6. DEV-47: Add authentication middleware (4h)
   Dependencies: DEV-43, DEV-45

7. DEV-48: Create example protected routes (3h)
   Dependencies: DEV-47
```

### Step 5: Show Dependency Graph (Optional)

If helpful, visualize the dependency graph:

```bash
anyt graph --json
```

Or show ASCII graph:

```bash
anyt graph
```

### Step 6: Offer Next Actions

Ask the user:

```
The epic has been broken down into <N> tasks with proper dependencies.

Would you like to:
  1. Start working on the first task (DEV-42)?
  2. Review the task board to see the full breakdown?
  3. Adjust priorities or dependencies?

Use /anytask:next to start working on the first available task.
```

## Important Guidelines

- **Use _ensure_ready first**: Always verify environment before starting
- **Investigation is critical**: Take time to understand the codebase before creating tasks
- **Use JSON mode**: All CLI commands should use `--json` for reliable parsing
- **Parse and store identifiers**: You need task identifiers for dependency creation
- **Create dependencies correctly**: Use `anyt task dep add DEV-X --on DEV-Y,DEV-Z --json`
- **Each task independently testable**: Should be deployable on its own
- **Logical ordering**: Use dependencies to enforce sequence, not just descriptions
- **Small tasks**: 4-8 hours max for a single PR
- **Specific titles**: Avoid vague terms like "Setup" or "Implement feature"
- **Technical context**: Include implementation guidance in notes
- **Consistent naming**: Use clear patterns across related tasks
- **Realistic estimates**: Base on complexity and codebase familiarity
- **HEREDOC format**: Use for all multi-line descriptions
- **No circular deps**: Ensure task graph is acyclic (A→B→C, not A→B→A)

## Dependency Guidelines

### Sequential Dependencies
When Task B requires Task A to be complete:

```bash
anyt task dep add DEV-B --on DEV-A --json
```

Response:
```json
{
  "success": true,
  "message": "Dependency added successfully"
}
```

### Multiple Dependencies
When Task C requires both A and B to be complete:

```bash
anyt task dep add DEV-C --on DEV-A,DEV-B --json
```

This creates an AND relationship: C can only start when BOTH A AND B are done.

### Verify Dependencies
Check dependencies were created correctly:

```bash
anyt task dep list DEV-C --json
```

Response shows both dependencies and what tasks are blocked by this one.

### Common Patterns

**Foundation → Services → API → UI:**
```bash
# Foundation (no deps)
anyt task add "Create database schema" --json → DEV-42

# Service layer (depends on schema)
anyt task add "Implement business logic" --json → DEV-43
anyt task dep add DEV-43 --on DEV-42 --json

# API layer (depends on services)
anyt task add "Create REST endpoints" --json → DEV-44
anyt task dep add DEV-44 --on DEV-43 --json

# UI layer (depends on API)
anyt task add "Build frontend components" --json → DEV-45
anyt task dep add DEV-45 --on DEV-44 --json
```

### Rules

- **No circular dependencies**: A → B → C is OK, but A → B → A is NOT
- **Foundation first**: Database/schema → Services → API → UI
- **Parallel tasks OK**: If tasks don't depend on each other, don't add dependencies
- **Test dependencies**: Testing tasks can depend on the feature they test

## Example Workflow

**User:** `/anytask:create-epic "Build user authentication system with OAuth and JWT"`

### Step 1: Pre-check Environment

```
[Uses _ensure_ready skill]
✓ anyt CLI installed
✓ ANYT_API_KEY set
✓ .anyt/anyt.json exists
```

### Step 2: Investigate Codebase

```
Analyzing codebase...
- Current auth: None detected
- Database: PostgreSQL with SQLAlchemy
- API Framework: FastAPI
- Frontend: React
- No existing OAuth providers configured

Identified 7 tasks needed:
1. Database schema for users and sessions (foundation)
2. JWT token generation and validation service
3. OAuth provider integration (Google)
4. Login/logout API endpoints
5. Frontend login component
6. Session management middleware
7. Protected route examples
```

### Step 3: Create Tasks with Dependencies

#### Task 1: Foundation (no dependencies)

```bash
anyt task add "Create user and session database schema" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-42",
    "title": "Create user and session database schema",
    "status": "backlog"
  }
}
```

Store identifier: `DEV-42`

Update description:
```bash
anyt task edit DEV-42 --description "$(cat <<'EOF'
## Description
Create database tables for users, sessions, and OAuth providers

## Objectives
- Design user table with email, hashed_password, oauth_provider fields
- Create session table with token, user_id, expiry
- Add necessary indexes for performance

## Acceptance Criteria
- [ ] Users table created with proper columns and constraints
- [ ] Sessions table created with foreign key to users
- [ ] Alembic migration written and tested
- [ ] Indexes added for email and session token lookups
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
None

## Estimated Effort
4 hours

## Technical Notes
- Use UUID for user IDs
- Add created_at/updated_at timestamps
- Ensure email is unique and indexed
- Session tokens should be hashed
EOF
)" --json
```

#### Task 2: JWT Service (depends on Task 1)

```bash
anyt task add "Implement JWT token service" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-43",
    ...
  }
}
```

Store identifier: `DEV-43`

Update description:
```bash
anyt task edit DEV-43 --description "$(cat <<'EOF'
## Description
Create service for generating and validating JWT tokens

## Objectives
- Implement token generation with user claims
- Add token validation and expiry checking
- Create refresh token mechanism

## Acceptance Criteria
- [ ] Token generation function with configurable expiry
- [ ] Token validation with signature verification
- [ ] Refresh token support implemented
- [ ] Unit tests for all token operations
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
DEV-42: Create user and session database schema

## Estimated Effort
6 hours

## Technical Notes
- Use PyJWT library
- Store secret key in environment variable
- Set token expiry to 1 hour, refresh to 7 days
- Include user_id and role in claims
EOF
)" --json
```

Add dependency on DEV-42:
```bash
anyt task dep add DEV-43 --on DEV-42 --json
```

Response:
```json
{
  "success": true,
  "message": "Dependency added successfully"
}
```

#### Task 3: OAuth Integration (depends on Tasks 1 and 2)

```bash
anyt task add "Integrate Google OAuth provider" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-44",
    ...
  }
}
```

Store identifier: `DEV-44`

Update description:
```bash
anyt task edit DEV-44 --description "$(cat <<'EOF'
## Description
Add Google OAuth authentication flow

## Objectives
- Configure Google OAuth credentials
- Implement OAuth callback handler
- Link OAuth accounts to user records

## Acceptance Criteria
- [ ] Google OAuth app configured in Google Console
- [ ] OAuth callback endpoint implemented
- [ ] User creation/linking from OAuth profile
- [ ] Error handling for failed OAuth attempts
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
DEV-42: Create user and session database schema
DEV-43: Implement JWT token service

## Estimated Effort
5 hours

## Technical Notes
- Use authlib for OAuth client
- Store credentials in environment variables
- Handle existing user matching by email
- Map OAuth profile to user table fields
EOF
)" --json
```

Add dependencies on DEV-42 AND DEV-43:
```bash
anyt task dep add DEV-44 --on DEV-42,DEV-43 --json
```

Response:
```json
{
  "success": true,
  "message": "Dependencies added successfully"
}
```

#### Task 4: Login/Logout Endpoints (depends on Task 2)

```bash
anyt task add "Create login and logout API endpoints" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-45",
    ...
  }
}
```

Store identifier: `DEV-45`

Update description:
```bash
anyt task edit DEV-45 --description "$(cat <<'EOF'
## Description
Implement REST API endpoints for login and logout

## Objectives
- POST /auth/login for username/password auth
- POST /auth/logout for session termination
- Return JWT tokens on successful login

## Acceptance Criteria
- [ ] POST /auth/login endpoint with validation
- [ ] POST /auth/logout endpoint with token invalidation
- [ ] Proper error responses (401, 400, 500)
- [ ] Session creation and cleanup
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
DEV-43: Implement JWT token service

## Estimated Effort
4 hours

## Technical Notes
- Use FastAPI dependencies for auth
- Hash passwords with bcrypt
- Rate limit login attempts
- Clear session from database on logout
EOF
)" --json
```

Add dependency on DEV-43:
```bash
anyt task dep add DEV-45 --on DEV-43 --json
```

#### Task 5: Frontend Login Component (depends on Tasks 3 and 4)

```bash
anyt task add "Build React login component" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-46",
    ...
  }
}
```

Store identifier: `DEV-46`

Update description:
```bash
anyt task edit DEV-46 --description "$(cat <<'EOF'
## Description
Create React component for user login form

## Objectives
- Build login form with email/password
- Add OAuth login button for Google
- Handle authentication errors gracefully

## Acceptance Criteria
- [ ] Login form with validation
- [ ] "Sign in with Google" button
- [ ] Error message display
- [ ] Token storage in localStorage/cookies
- [ ] Redirect to dashboard on success
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
DEV-44: Integrate Google OAuth provider
DEV-45: Create login and logout API endpoints

## Estimated Effort
6 hours

## Technical Notes
- Use React Hook Form for validation
- Store JWT in httpOnly cookies (not localStorage)
- Use React Context for auth state
- Follow existing component patterns
EOF
)" --json
```

Add dependencies on DEV-44 AND DEV-45:
```bash
anyt task dep add DEV-46 --on DEV-44,DEV-45 --json
```

#### Task 6: Authentication Middleware (depends on Tasks 2 and 4)

```bash
anyt task add "Add authentication middleware" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-47",
    ...
  }
}
```

Store identifier: `DEV-47`

Update description:
```bash
anyt task edit DEV-47 --description "$(cat <<'EOF'
## Description
Create middleware to protect authenticated routes

## Objectives
- Validate JWT tokens on protected endpoints
- Extract user context from tokens
- Handle token expiry and refresh

## Acceptance Criteria
- [ ] Middleware validates JWT on each request
- [ ] Current user injected into request context
- [ ] Token refresh logic implemented
- [ ] 401 responses for invalid/expired tokens
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
DEV-43: Implement JWT token service
DEV-45: Create login and logout API endpoints

## Estimated Effort
4 hours

## Technical Notes
- Use FastAPI dependency injection
- Make middleware reusable across routes
- Support optional auth for public routes
- Log authentication failures
EOF
)" --json
```

Add dependencies on DEV-43 AND DEV-45:
```bash
anyt task dep add DEV-47 --on DEV-43,DEV-45 --json
```

#### Task 7: Example Protected Routes (depends on Task 6)

```bash
anyt task add "Create example protected routes" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-48",
    ...
  }
}
```

Store identifier: `DEV-48`

Update description:
```bash
anyt task edit DEV-48 --description "$(cat <<'EOF'
## Description
Add example endpoints demonstrating protected routes

## Objectives
- Create GET /me endpoint for current user
- Create GET /protected/dashboard endpoint
- Demonstrate role-based access

## Acceptance Criteria
- [ ] GET /me returns current user profile
- [ ] GET /protected/dashboard requires auth
- [ ] 401 returned for unauthenticated requests
- [ ] Documentation updated with examples
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
DEV-47: Add authentication middleware

## Estimated Effort
3 hours

## Technical Notes
- Use auth middleware as dependency
- Include user ID in response
- Add OpenAPI/Swagger docs
- Show both required and optional auth patterns
EOF
)" --json
```

Add dependency on DEV-47:
```bash
anyt task dep add DEV-48 --on DEV-47 --json
```

### Step 4: Display Epic Summary

```
✅ Epic created successfully!

Created 7 tasks with total estimated effort: 32 hours

Task breakdown (in dependency order):

1. DEV-42: Create user and session database schema (4h)
   Status: backlog
   Dependencies: None
   ↓

2. DEV-43: Implement JWT token service (6h)
   Status: backlog
   Dependencies: DEV-42
   ↓

3. DEV-44: Integrate Google OAuth provider (5h)
   Status: backlog
   Dependencies: DEV-42, DEV-43

4. DEV-45: Create login and logout API endpoints (4h)
   Status: backlog
   Dependencies: DEV-43
   ↓

5. DEV-46: Build React login component (6h)
   Status: backlog
   Dependencies: DEV-44, DEV-45

6. DEV-47: Add authentication middleware (4h)
   Status: backlog
   Dependencies: DEV-43, DEV-45
   ↓

7. DEV-48: Create example protected routes (3h)
   Status: backlog
   Dependencies: DEV-47
```

### Step 5: Show Dependency Graph (Optional)

```bash
anyt graph --json
```

Or ASCII visualization:
```bash
anyt graph
```

### Step 6: Offer Next Actions

```
The epic has been broken down into 7 tasks with proper dependencies.

Would you like to:
  1. Start working on the first task (DEV-42)?
  2. Review the task board?
  3. Adjust priorities or dependencies?

Use /anytask:next to start working on the first available task.
```

## Task Size Guidelines

- **Too small**: "Add import statement" - combine with related work
- **Just right**: "Implement JWT token service" - clear scope, 4-8 hours
- **Too large**: "Build entire auth system" - needs breaking down

## Common Epic Patterns

1. **New Feature**:
   - Database/models → API/services → Frontend → Integration → Tests → Docs

2. **Refactoring**:
   - Analysis/planning → Extract interfaces → Migrate module 1 → Migrate module 2 → Cleanup → Tests

3. **Infrastructure**:
   - Setup/config → Core service → Monitoring → Documentation → Runbooks

4. **Bug Fix (Complex)**:
   - Investigation → Root cause fix → Add tests → Related fixes → Verification

## CLI Command Reference

### Task Creation

Create task with JSON:
```bash
anyt task add "Task title" --json
anyt task add "Task title" --priority 1 --json
anyt task add "Task title" --labels "feature,urgent" --json
```

### Task Editing

Update task description:
```bash
anyt task edit DEV-42 --description "$(cat <<'EOF'
Multi-line description
EOF
)" --json
```

Update task priority:
```bash
anyt task edit DEV-42 --priority 1 --json
```

### Dependency Management

Add single dependency:
```bash
anyt task dep add DEV-43 --on DEV-42 --json
```

Add multiple dependencies:
```bash
anyt task dep add DEV-45 --on DEV-42,DEV-43,DEV-44 --json
```

List dependencies:
```bash
anyt task dep list DEV-45 --json
```

Remove dependencies:
```bash
anyt task dep rm DEV-45 --on DEV-42 --json
```

### Visualization

View dependency graph:
```bash
anyt graph --json
anyt graph --format ascii
anyt graph --format dot
```

View specific task graph:
```bash
anyt graph DEV-42 --json
```

### Task Information

Show task details:
```bash
anyt task show DEV-42 --json
```

List all tasks:
```bash
anyt task list --json
anyt task list --status backlog,todo --json
```

Start creating the epic now based on the user's request.
