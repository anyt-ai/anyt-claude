# Continue or Pick Next Task

Continue working on the active task, or automatically pick and start on the next recommended task.

**User intent:** "What's next?" or "Continue my work" or "/next"

## Before You Start

**IMPORTANT:** Before executing ANY AnyTask operations, use the `_ensure_ready` skill to verify:
1. `anyt` CLI is installed
2. `ANYT_API_KEY` environment variable is set
3. `.anyt/anyt.json` configuration file exists

If pre-checks fail, stop and show the error message. If not initialized, guide user to run `/anytask:init` first.

## Your Process

### Step 1: Check for Active Task

First, check if there's already an active task:

```bash
anyt active --json
```

**Parse the JSON response:**
- If `success == true` and `data != null`: There IS an active task
- If `success == true` and `data == null`: There is NO active task

#### If Active Task EXISTS:

Display to user:

```
📍 You have an active task: <identifier> - <title>

<Show task details: description, acceptance criteria, etc.>

I'll continue working on this task.
```

Proceed directly to **Step 3: Start Implementation**.

#### If NO Active Task:

Proceed to **Step 2: Suggest Tasks**.

### Step 2: Suggest Tasks (No Active Task)

Get recommended tasks:

```bash
anyt task suggest --limit 3 --status todo,backlog --json
```

**Parse the JSON response:**
- If `items` is empty or count is 0: No tasks available
- If `items` has tasks: Show suggestions

#### If NO tasks available:

Display to user:

```
No tasks available to work on right now.

All tasks may be blocked, completed, or assigned to others.

Would you like to create a new task? Use /anytask:create
```

**STOP here** - do not proceed further.

#### If tasks ARE available:

**For MVP: Automatically select the first recommended task**

Display:

```
I found <count> recommended tasks. Starting with the highest priority:

<identifier>: <title>
  Priority: <priority>
  Status: <status>
  Estimate: <estimate> hours

<Brief description>
```

Extract the first task's identifier and proceed to pick it.

**Alternative (user choice mode - not MVP):**

```
I recommend these tasks:

1. <identifier>: <title> (Priority: <priority>)
2. <identifier>: <title> (Priority: <priority>)
3. <identifier>: <title> (Priority: <priority>)

I'll start with #1 unless you prefer another.
```

### Step 2b: Pick the Selected Task

Pick the task to make it active:

```bash
anyt task pick <identifier> --json
```

Verify success from JSON response.

### Step 3: Start Implementation

Now you have an active task (either from Step 1 or Step 2b).

#### 3a. Show Full Task Context

Fetch and display complete task details:

```bash
anyt task show <identifier> --json
```

Display to user:

```
Task: <identifier> - <title>

## Description
<description>

## Objectives
<list objectives>

## Acceptance Criteria
<list all criteria with checkboxes>

## Dependencies
<list any dependencies>

## Estimated Effort
<hours>

## Technical Notes
<notes>

---
Starting implementation now...
```

#### 3b. Implement the Task

**This is the core work phase:**

1. **Understand the task**: Read description, objectives, acceptance criteria
2. **Find relevant files**: Use Glob, Grep, Read tools to explore codebase
3. **Implement changes**: Use Edit/Write tools to make code changes
4. **Write tests**: Follow acceptance criteria - tests must be written
5. **Run tests**: Use Bash to run test suite and verify
6. **Track progress**: Update acceptance criteria as you complete items

**For EACH completed acceptance criterion:**

a. Add a comment:
```bash
anyt comment add <identifier> -m "Completed: <criterion description>" --json
```

b. (Optional) Update description to check off item:
```bash
# First, get current description
anyt task show <identifier> --json

# Then update with checked item
anyt task edit <identifier> --description "$(cat <<'EOF'
<updated description with - [x] for completed items>
EOF
)" --json
```

### Step 4: Mark Task as Done

When ALL acceptance criteria are satisfied:

```bash
anyt task done <identifier> --note "All acceptance criteria met, tests passing" --json
```

Display to user:

```
✅ Task <identifier> completed successfully!

All acceptance criteria met:
  ✓ <criterion 1>
  ✓ <criterion 2>
  ✓ <criterion 3>
  ✓ Tests written and passing
```

### Step 5: Commit Changes (Optional based on workflow)

If there are uncommitted changes, offer to commit:

```bash
# Check git status
git status

# If changes exist, show them
git diff

# Ask user if they want to commit
```

If yes, create a commit:

```bash
git add .
git commit -m "$(cat <<'EOF'
<identifier>: <title>

<Brief description of what was implemented>

- <Key change 1>
- <Key change 2>
- <Key change 3>

Resolves <identifier>
EOF
)"
```

### Step 6: Suggest Next Task

After completing a task, automatically suggest what's next:

```
Great work! Would you like to continue with another task?

I can suggest the next one if you run /anytask:next again.
```

## Important Guidelines

- **Auto-pick in MVP**: For MVP, automatically select the first suggested task
- **Focus on implementation**: The main value is in actually implementing the code
- **Track progress**: Add comments as you complete acceptance criteria
- **Run tests**: Always verify tests pass before marking done
- **Use JSON mode**: Parse JSON responses reliably
- **Clear communication**: Keep user informed of progress
- **Handle errors**: Check JSON `success` field and handle failures gracefully

## Example Workflow

**User:** `/anytask:next`

**Step 1: Check active**
```bash
anyt active --json
```

Response:
```json
{
  "success": true,
  "data": null
}
```

No active task → proceed to suggest.

**Step 2: Suggest**
```bash
anyt task suggest --limit 3 --status todo,backlog --json
```

Response:
```json
{
  "success": true,
  "items": [
    {
      "identifier": "DEV-42",
      "title": "Add task comments API",
      "priority": 1,
      "status": "todo"
    },
    {
      "identifier": "DEV-43",
      "title": "Implement caching",
      "priority": 0,
      "status": "backlog"
    }
  ],
  "count": 2
}
```

Display:
```
I found 2 recommended tasks. Starting with the highest priority:

DEV-42: Add task comments API
  Priority: 1 (High)
  Status: todo

Let me pick this task and get started...
```

**Step 2b: Pick**
```bash
anyt task pick DEV-42 --json
```

**Step 3: Show task and implement**
```bash
anyt task show DEV-42 --json
```

Display task details, then:

```
Starting implementation now...

[Claude explores codebase, implements features, writes tests]

[As each criterion is completed:]
```

```bash
anyt comment add DEV-42 -m "Completed: POST endpoint implementation" --json
anyt comment add DEV-42 -m "Completed: Database persistence" --json
anyt comment add DEV-42 -m "Completed: Tests and error handling" --json
```

**Step 4: Mark done**
```bash
anyt task done DEV-42 --note "All acceptance criteria met, tests passing" --json
```

Display:
```
✅ Task DEV-42 completed successfully!

All acceptance criteria met:
  ✓ POST endpoint implemented
  ✓ Database persistence working
  ✓ Error handling added
  ✓ Tests written and passing

Would you like to continue with another task?
```

## Advanced Options

### Include assigned tasks in suggestions
```bash
anyt task suggest --limit 5 --include-assigned --json
```

### Filter by specific status
```bash
anyt task suggest --status todo --limit 5 --json
```

### Show more suggestions
```bash
anyt task suggest --limit 10 --json
```

## Error Handling

### No active task and no suggestions
```
No tasks available to work on right now.

Would you like to create a new task? Use /anytask:create
```

### Task pick failed
```
❌ Failed to pick task <identifier>

Error: <error message>

The task may already be assigned or blocked.
Try: anyt task show <identifier>
```

### Task completion failed
```
❌ Failed to mark task as done

Error: <error message>

Possible reasons:
  • Task has incomplete dependencies
  • Task is blocked
  • Permission issue
```

## CLI Command Reference

Check active task:
```bash
anyt active --json
```

Get suggestions:
```bash
anyt task suggest --limit 3 --status todo,backlog --json
anyt task suggest --limit 5 --include-assigned --json
```

Pick a task:
```bash
anyt task pick <identifier> --json
```

Show task details:
```bash
anyt task show <identifier> --json
```

Add comment:
```bash
anyt comment add <identifier> -m "Comment text" --json
```

Mark task as done:
```bash
anyt task done <identifier> --json
anyt task done <identifier> --note "Completion note" --json
```

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
