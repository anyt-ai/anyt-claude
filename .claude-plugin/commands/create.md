# Create AnyTask Task

Create a single, well-structured task suitable for one pull request.

**User intent:** "Create a task for..." or "Add a task to..."

**Expected input:** A description of what needs to be built (e.g., "Add API endpoint for task comments")

## Before You Start

**IMPORTANT:** Before executing ANY AnyTask operations, use the `_ensure_ready` skill to verify:
1. `anyt` CLI is installed
2. `ANYT_API_KEY` environment variable is set
3. `.anyt/anyt.json` configuration file exists

If pre-checks fail, stop and show the error message. If not initialized, guide user to run `/anytask:init` first.

## Your Process

### Step 1: Understand the Requirement

From the user's natural language description:
- Extract what needs to be built
- Identify technical requirements
- Determine scope (should fit in one PR, 4-8 hours of work)
- Think about acceptance criteria
- Consider dependencies (optional - can check existing tasks)

### Step 2: Generate Title and Structured Description

Create:

**Title:** Clear, concise, actionable (e.g., "Add task comments API and UI")

**Structured Description:**
```markdown
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
None
(Or list task identifiers if there are dependencies)

## Estimated Effort
[X] hours

## Technical Notes
[Implementation guidance, architecture decisions, gotchas]
```

### Step 3: Create Task with JSON Output

Use JSON mode to capture the task identifier:

```bash
anyt task add "Title here" --json
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

Extract the `identifier` field (e.g., DEV-42).

### Step 4: Update Task Description

Add the structured description using HEREDOC:

```bash
anyt task edit DEV-42 --description "$(cat <<'EOF'
## Description
...

## Objectives
...

## Acceptance Criteria
...

## Dependencies
...

## Estimated Effort
...

## Technical Notes
...
EOF
)" --json
```

### Step 5: Display Summary

Show the user a concise summary:

```
✅ Created task DEV-42: Add task comments API and UI

Status: backlog
Estimated: 4 hours

Key objectives:
  • Implement POST /api/tasks/{id}/comments endpoint
  • Add comments list UI component
  • Write tests and documentation

Next steps:
  • Use /anytask:next to start working on this task
  • Or continue creating more tasks
```

### Step 6: Offer Next Actions

Ask the user:

```
Would you like to:
  1. Start working on this task now? (I'll run /anytask:next)
  2. Create more tasks?
  3. See the current task board?
```

## Important Guidelines

- **Single task focus**: Create ONE task suitable for ONE pull request
- **MVP scope**: Don't create epics or multi-task breakdowns (use /anytask:create-epic for that)
- **Clear titles**: Be specific and actionable
- **Structured descriptions**: Always use the standard format
- **Realistic estimates**: 3-8 hours typically, based on complexity
- **Parse JSON output**: Extract identifiers reliably
- **Use HEREDOC**: Ensures proper multi-line formatting
- **Include tests**: Always add "Tests written and passing" to acceptance criteria
- **Check dependencies**: Optionally query existing tasks to identify dependencies

## Example Workflow

**User:** `/anytask:create "Add API endpoint for task comments"`

**Step 1: Pre-check**
```
[Uses _ensure_ready skill]
✓ Environment ready
```

**Step 2: Analyze requirement**
```
Requirement: API endpoint for adding comments to tasks
Scope: Backend API only (suitable for one PR)
Estimate: ~4 hours
```

**Step 3: Create task**
```bash
anyt task add "Add API endpoint for task comments" --json
```

Response:
```json
{
  "success": true,
  "data": {
    "identifier": "DEV-42",
    "title": "Add API endpoint for task comments",
    "status": "backlog"
  }
}
```

**Step 4: Update description**
```bash
anyt task edit DEV-42 --description "$(cat <<'EOF'
## Description
Create REST API endpoint to add comments to tasks

## Objectives
- Implement POST /api/tasks/{task_id}/comments endpoint
- Store comments in database with timestamps
- Return created comment with proper status codes

## Acceptance Criteria
- [ ] Endpoint accepts task_id and comment text
- [ ] Comments are persisted to database
- [ ] Proper error handling for invalid task_id
- [ ] API documentation updated
- [ ] Tests written and passing
- [ ] Code reviewed and merged

## Dependencies
None

## Estimated Effort
4 hours

## Technical Notes
- Use existing task validation middleware
- Follow RESTful conventions
- Include user_id from auth context
- Return 201 Created with comment object
EOF
)" --json
```

**Step 5: Display summary**
```
✅ Created task DEV-42: Add API endpoint for task comments

Status: backlog
Priority: 0 (normal)
Estimated: 4 hours

Key objectives:
  • POST /api/tasks/{task_id}/comments endpoint
  • Database persistence with timestamps
  • Proper error handling and validation

Next steps:
  • Use /anytask:next to start working on this task
  • Or continue creating more tasks

Would you like to:
  1. Start working on this task now?
  2. Create more tasks?
```

## Advanced Options

### Set Priority

If the user mentions urgency:

```bash
anyt task add "Critical bug fix" --priority 2 --json
```

Priority levels:
- `-2`: Lowest
- `-1`: Low
- `0`: Normal (default)
- `1`: High
- `2`: Highest/Urgent

### Add Labels

If the user mentions categories:

```bash
anyt task add "Add OAuth support" --labels "feature,auth,security" --json
```

### Set Initial Status

Default is `backlog`, but you can set:

```bash
anyt task add "Fix login bug" --status todo --json
```

### Specify Phase

If working in phases/milestones:

```bash
anyt task add "Implement caching" --phase "T3" --json
```

## Error Handling

### Task Creation Failed

```
❌ Failed to create task

Error: <error message from CLI>

Please check:
  • ANYT_API_KEY is valid
  • You have permission to create tasks
  • The workspace/project is accessible
```

### Description Update Failed

```
⚠️ Task DEV-42 was created but description update failed

You can manually update it:
  anyt task edit DEV-42 --description "..."
```

## CLI Command Reference

Create basic task:
```bash
anyt task add "Title" --json
```

Create with options:
```bash
anyt task add "Title" \
  --description "Description" \
  --priority 1 \
  --status todo \
  --labels "feature,urgent" \
  --phase "T3" \
  --estimate 6 \
  --json
```

Update task description:
```bash
anyt task edit DEV-42 --description "$(cat <<'EOF'
Multi-line
description
here
EOF
)" --json
```

Show task:
```bash
anyt task show DEV-42 --json
```
