# Create Epic - Break Down Large Features into Tasks

You are helping the user create an epic (a large feature or initiative) by breaking it down into multiple smaller tasks that can each fit in a single pull request.

**Expected input:** A high-level feature description or epic requirement (e.g., "Build user authentication system with OAuth and JWT")

## Your Process

1. **Investigate and Analyze the Requirement**
   - Read the user's request carefully
   - Search the codebase to understand existing architecture, patterns, and dependencies
   - Identify the key components and workflows needed
   - Research any relevant technologies or APIs required
   - Consider edge cases and non-functional requirements
   - Think about the logical order of implementation

2. **Break Down into Tasks**
   Decompose the epic into smaller, focused tasks that each:
   - Can be completed in a single pull request (4-8 hours of work)
   - Have a clear, specific objective
   - Build logically on previous tasks
   - Are testable independently
   - Follow the existing codebase patterns

   Typical task categories to consider:
   - Database/schema changes
   - Backend API endpoints
   - Frontend components
   - Integration/middleware
   - Testing infrastructure
   - Documentation

3. **Create Each Task with Dependencies**
   For each task in the breakdown:

   a. **Create the task with title:**
   ```bash
   anyt task add "<clear, specific title>"
   ```

   b. **Parse the task identifier** (e.g., DEV-42) from the output

   c. **Update with structured description:**
   ```bash
   anyt task edit <task-id> --description "$(cat <<'EOF'
   ## Description
   [What needs to be built]

   ## Objectives
   - [Specific goal 1]
   - [Specific goal 2]

   ## Acceptance Criteria
   - [ ] [Criterion 1]
   - [ ] [Criterion 2]
   - [ ] Tests written and passing
   - [ ] Code reviewed and merged

   ## Dependencies
   [Task identifier]: [Task name]
   (Or "None" if no dependencies)

   ## Estimated Effort
   [X hours estimate]

   ## Technical Notes
   [Implementation guidance, architectural decisions, gotchas]
   EOF
   )"
   ```

   d. **Set priority if needed:**
   ```bash
   anyt task edit <task-id> --priority <-2 to 2>
   ```

   e. **Add dependencies for sequencing:**
   If this task depends on other tasks, use:
   ```bash
   anyt task dep add <task-id> --on <dependency-task-id-1>,<dependency-task-id-2>
   ```

   Example:
   ```bash
   # Task DEV-44 depends on DEV-42 and DEV-43 being completed first
   anyt task dep add DEV-44 --on DEV-42,DEV-43
   ```

4. **Create a Summary**
   After creating all tasks, display:
   - Total number of tasks created
   - Task identifiers and titles in dependency order
   - A dependency graph (use `anyt graph` if available)
   - Suggested next steps for the user

5. **Ask User**
   - "Would you like to start working on the first task now?"
   - "Or would you like to review the task breakdown first?"

## Important Notes

- **Investigation is critical** - Take time to understand the codebase before creating tasks
- Each task should be **independently testable and deployable**
- Tasks should be **logically ordered** - use dependencies to enforce sequence
- Keep tasks **small enough** for a single PR (4-8 hours max)
- Be **specific in titles** - avoid vague terms like "Setup" or "Implement feature"
- Include **technical context** in notes - help future implementers
- Use **consistent naming** across related tasks
- Set **realistic estimates** based on complexity
- Always use **HEREDOC format** for multi-line descriptions
- Dependencies use **comma-separated** task identifiers: `--on DEV-1,DEV-2,DEV-3`

## Dependency Guidelines

- **Sequential dependencies**: When Task B requires Task A to be complete, add dependency:
  ```bash
  anyt task dep add DEV-B --on DEV-A
  ```

- **Multiple dependencies**: When Task C requires both A and B:
  ```bash
  anyt task dep add DEV-C --on DEV-A,DEV-B
  ```

- **No circular dependencies**: Ensure tasks don't depend on each other in a loop

- **Foundation first**: Database/schema tasks typically come first, then API, then UI

## Example Workflow

**User request:** "Build user authentication system with OAuth and JWT"

**Step 1: Investigate**
```
Analyzing codebase...
- Current auth: None detected
- Database: PostgreSQL with SQLAlchemy
- API Framework: FastAPI
- Frontend: React
- No existing OAuth providers configured
```

**Step 2: Break down into tasks**
```
1. Database schema for users and sessions
2. JWT token generation and validation service
3. OAuth provider integration (Google)
4. Login/logout API endpoints
5. Frontend login component
6. Session management middleware
7. Protected route examples
```

**Step 3: Create tasks with dependencies**

```bash
# Task 1: Foundation - no dependencies
anyt task add "Create user and session database schema"
# Output: Created task DEV-42

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
)"

# Task 2: Depends on Task 1
anyt task add "Implement JWT token service"
# Output: Created task DEV-43

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
)"

# Add dependency
anyt task dep add DEV-43 --on DEV-42

# Task 3: Depends on Tasks 1 and 2
anyt task add "Integrate Google OAuth provider"
# Output: Created task DEV-44

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
)"

# Add dependencies
anyt task dep add DEV-44 --on DEV-42,DEV-43

# Task 4: Login/logout endpoints
anyt task add "Create login and logout API endpoints"
# Output: Created task DEV-45

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
)"

anyt task dep add DEV-45 --on DEV-43

# Task 5: Frontend component
anyt task add "Build React login component"
# Output: Created task DEV-46

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
)"

anyt task dep add DEV-46 --on DEV-44,DEV-45

# Task 6: Middleware
anyt task add "Add authentication middleware"
# Output: Created task DEV-47

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
)"

anyt task dep add DEV-47 --on DEV-43,DEV-45

# Task 7: Example protected routes
anyt task add "Create example protected routes"
# Output: Created task DEV-48

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
)"

anyt task dep add DEV-48 --on DEV-47
```

**Step 4: Summary**
```
Epic created successfully! 7 tasks created:

DEV-42: Create user and session database schema (4h) [No dependencies]
DEV-43: Implement JWT token service (6h) [Depends on: DEV-42]
DEV-44: Integrate Google OAuth provider (5h) [Depends on: DEV-42, DEV-43]
DEV-45: Create login and logout API endpoints (4h) [Depends on: DEV-43]
DEV-46: Build React login component (6h) [Depends on: DEV-44, DEV-45]
DEV-47: Add authentication middleware (4h) [Depends on: DEV-43, DEV-45]
DEV-48: Create example protected routes (3h) [Depends on: DEV-47]

Total estimated effort: 32 hours

Dependency graph:
```

```bash
anyt graph
```

**Step 5: Ask user**
```
The epic has been broken down into 7 tasks with proper dependencies.

Would you like to:
1. Start working on the first task (DEV-42)?
2. Review the task breakdown first?
3. Adjust priorities or dependencies?
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

Start creating the epic now based on the user's request.
