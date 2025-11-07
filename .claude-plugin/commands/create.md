# Create AnyTask Task

You are helping the user create a new task in AnyTask using the CLI.

**Expected input:** A description or prompt for what needs to be built (e.g., "Add API endpoint for task comments")

## Your Process

1. **Create the Task with Title Only**
   Use the Bash tool to create the task with just the title to get the task ID:
   ```bash
   anyt task add "<title>"
   ```

   Parse the output to extract the task identifier (e.g., DEV-42).

2. **Analyze the Description**
   Based on the user's input, break down:
   - What needs to be implemented
   - Specific technical requirements and objectives
   - Dependencies on other tasks (if any)
   - Acceptance criteria for completion
   - Complexity estimate (hours)
   - Implementation guidance and technical notes

3. **Update Task with Structured Description**
   Use the Bash tool to update the task description with the following structured format:
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
   - [Task identifier]: [Task name]
   (Or "None" if no dependencies)

   ## Estimated Effort
   [X hours estimate]

   ## Technical Notes
   [Implementation guidance]
   EOF
   )"
   ```

4. **Ask User**
   Show the created task summary and ask:
   - "Would you like to start working on this task now? (use `anyt task pick <task-id>`)"
   - "Or would you like to keep it in backlog for later?"

## Important Notes

- Only create tasks for actual code implementation work
- Don't create tasks for linting, formatting, or docs
- Always use the Bash tool to run `anyt` commands
- The title should be clear and concise
- Parse the output to extract the task identifier (e.g., DEV-42)
- Be specific in acceptance criteria
- Include proper dependencies (check existing tasks if needed)
- Use HEREDOC format for multi-line descriptions to ensure proper formatting

## Example

**User request:** "Add API endpoint for task comments"

**Step 1: Create task**
```bash
anyt task add "Add API endpoint for task comments"
```

**Step 2: Update with structured description**
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
EOF
)"
```

Create the task now.
