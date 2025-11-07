# Get Next Task and Work On It

You are helping the user automatically get the next suggested task and start working on it.

## Your Process

1. **Get the Next Task**
   Use the Bash tool to get the top recommended task:
   ```bash
   anyt task suggest --limit 1
   ```

   This command returns the highest-priority task that:
   - Is ready to work on (all dependencies met)
   - Is unassigned (by default)
   - Has no blockers

2. **Handle No Tasks Available**
   If no task is suggested:
   - Reply to the user: "No tasks available to work on right now. All tasks may be blocked, completed, or assigned."
   - Suggest: "Would you like to create a new task? Use the `/create` command."
   - STOP here - do not proceed further

3. **Start Working on the Task**
   If a task is found, automatically start it:

   a. **Pick the task:**
   ```bash
   anyt task pick <task-identifier>
   ```

   b. **Add starting comment:**
   ```bash
   anyt comment add <task-identifier> -m "Starting work on this task"
   ```

   c. **Change status to inprogress:**
   ```bash
   anyt task edit <task-identifier> --status inprogress
   ```

4. **Display Task Details**
   Show the user:
   - Task identifier and title
   - Full description with all sections
   - Acceptance criteria checklist
   - Dependencies (if any)
   - Estimated effort

5. **Track Progress Through Acceptance Criteria**
   As you work on the task, for EACH completed acceptance criterion:

   a. **Update the task description** to check off the completed item:
   - Read the current description with `anyt task show <task-identifier>`
   - Update the description, changing `- [ ]` to `- [x]` for the completed item
   - Use the edit command with HEREDOC format:
   ```bash
   anyt task edit <task-identifier> --description "$(cat <<'EOF'
   [Updated description with checked item]
   EOF
   )"
   ```

   b. **Add a comment** about what was completed:
   ```bash
   anyt comment add <task-identifier> -m "Completed: [criterion description]"
   ```

6. **Mark Task as Done**
   When ALL acceptance criteria are checked:

   a. **Add final comment:**
   ```bash
   anyt comment add <task-identifier> -m "All acceptance criteria completed"
   ```

   b. **Mark task as done:**
   ```bash
   anyt task done <task-identifier>
   ```

7. **Commit Changes**
   After marking the task as done, commit all changes with a descriptive message:

   a. **Review changes:**
   ```bash
   git status
   git diff
   ```

   b. **Stage and commit changes:**
   ```bash
   git add .
   git commit -m "$(cat <<'EOF'
   <task-identifier>: <task-title>

   <Detailed description of what was implemented>

   - <Key change 1>
   - <Key change 2>
   - <Key change 3>

   Resolves <task-identifier>
   EOF
   )"
   ```

   The commit message should:
   - Start with task identifier (e.g., "DEV-42: Implement Redis caching")
   - Include a blank line after the title
   - Provide detailed description of implementation
   - List key changes as bullet points
   - End with "Resolves <task-identifier>"

## Important Notes

- Always use the Bash tool to run `anyt` commands
- Status values are: backlog, todo, inprogress, blocked, canceled, done, archived
- The status is "inprogress" (NOT "in_progress")
- Update acceptance criteria as you complete each one - don't wait until the end
- Add comments throughout the implementation to document progress
- Use HEREDOC format when updating descriptions to preserve formatting
- Always read the current task description before updating it
- Commit messages must start with the task identifier (e.g., "DEV-42: Title")
- Write clear, descriptive commit messages that explain what was implemented

## Example Workflow

**User:** `/next`

**Step 1: Get suggestion**
```bash
anyt task suggest --limit 1
```

Output: `DEV-42: Implement Redis caching for API endpoints`

**Step 2: Start the task**
```bash
anyt task pick DEV-42
anyt comment add DEV-42 -m "Starting work on this task"
anyt task edit DEV-42 --status inprogress
```

**Step 3: Show task details**
```
Task: DEV-42 - Implement Redis caching for API endpoints
Status: inprogress

## Description
Add Redis caching to improve API performance

## Objectives
- Configure Redis client with connection pooling
- Apply caching to read endpoints
- Implement cache invalidation

## Acceptance Criteria
- [ ] Redis client configured with connection pooling
- [ ] Caching middleware applied to read endpoints
- [ ] Cache invalidation working for updates/deletes
- [ ] Tests written for cache behavior
- [ ] Documentation updated

## Estimated Effort
6 hours
```

**Step 4: As work progresses** (after completing first criterion)
```bash
# Update description to check first item
anyt task edit DEV-42 --description "$(cat <<'EOF'
## Description
Add Redis caching to improve API performance

## Objectives
- Configure Redis client with connection pooling
- Apply caching to read endpoints
- Implement cache invalidation

## Acceptance Criteria
- [x] Redis client configured with connection pooling
- [ ] Caching middleware applied to read endpoints
- [ ] Cache invalidation working for updates/deletes
- [ ] Tests written for cache behavior
- [ ] Documentation updated

## Estimated Effort
6 hours
EOF
)"

# Add comment
anyt comment add DEV-42 -m "Completed: Redis client configured with connection pooling"
```

**Step 5: When all criteria completed**
```bash
anyt comment add DEV-42 -m "All acceptance criteria completed"
anyt task done DEV-42
```

**Step 6: Commit the changes**
```bash
# Review changes
git status
git diff

# Stage and commit
git add .
git commit -m "$(cat <<'EOF'
DEV-42: Implement Redis caching for API endpoints

Added Redis caching to improve API performance for read-heavy endpoints.
Configured connection pooling and implemented cache invalidation strategy.

- Added Redis client with connection pooling configuration
- Applied caching middleware to all read endpoints (GET /tasks, GET /users)
- Implemented cache invalidation for updates and deletes
- Added comprehensive tests for cache behavior
- Updated documentation with caching guidelines

Resolves DEV-42
EOF
)"
```

## No Task Available Example

```bash
anyt task suggest --limit 1
```

Output: `No suggestions available`

Response to user:
```
No tasks available to work on right now. All tasks may be blocked, completed, or assigned.

Would you like to create a new task? Use the `/create` command.
```
