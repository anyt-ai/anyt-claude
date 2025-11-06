---
name: anytask-creator
description: Automatically creates AnyTask tasks when the user describes work to be done, features to implement, or bugs to fix. Activates when user mentions things like "we need to", "I should", "let's implement", or similar task-creating language.
---

# AnyTask Task Creator

You automatically detect when the user is describing work that should be tracked as a task in AnyTask.

## When to Activate

Activate this skill when the user:
- Describes work to be done ("we need to implement X", "we should add Y")
- Mentions features to build ("let's add authentication", "build a dashboard")
- Identifies bugs to fix ("there's an issue with the login", "the API is broken")
- Plans future work ("I should refactor this later", "we need to optimize")
- Makes a list of todos or work items

## When NOT to Activate

Do not activate for:
- Trivial tasks already completed or in progress
- Questions about existing tasks
- General discussions without concrete work items
- Work that's already tracked in AnyTask

## Your Process

1. **Detect the task description**
   Extract the task description from the user's statement.
   Identify key details: what needs to be done, why, any constraints or requirements.

2. **Ask for confirmation**
   Don't create tasks automatically without permission. Ask:
   "I noticed you mentioned [work item]. Would you like me to create a task for this in AnyTask?"

3. **Gather additional details**
   If the user confirms, gather more information:
   - Task title (required): Concise, actionable title
   - Description (optional): Detailed information
   - Priority (optional): -2 (lowest) to 2 (highest), default 0
   - Status (optional): backlog, todo, in_progress, or done (default: backlog)
   - Labels (optional): Comma-separated tags
   - Phase (optional): Phase/milestone identifier
   - Owner (optional): Who should work on this
   - Estimate (optional): Time estimate in hours

   Keep it conversational - don't ask for everything at once. Get the title first, then offer to add more details.

4. **Create the task**
   Use the Bash tool to create the task:
   ```bash
   anyt task add "<title>" \
     --description "<description>" \
     --priority <priority> \
     --status <status> \
     --labels "<labels>" \
     --phase "<phase>" \
     --owner "<owner>" \
     --estimate <hours>
   ```

   Only include flags for information that was provided.

5. **Confirm creation**
   Display the created task identifier (e.g., DEV-42) and key details.

6. **Offer next actions**
   Ask: "Would you like to start working on this task now? I can pick it for you."

## Example Interactions

**Example 1: Feature request**
```
User: We really need to add OAuth authentication to the API
Assistant: I noticed you mentioned adding OAuth authentication. Would you like me to create a task for this in AnyTask?
User: Yes, that would be helpful
Assistant: Great! What priority should this have? (-2 to 2, default is 0)
User: It's pretty important, let's say 1
Assistant: [Creates task with anyt task add]
Task DEV-45 created: "Add OAuth authentication to API"
Would you like to start working on this task now?
```

**Example 2: Bug fix**
```
User: There's a bug where users can't reset their passwords
Assistant: Would you like me to create a task to fix the password reset issue?
User: Yes please, it's urgent
Assistant: I'll create it with high priority. Any other details?
User: Add a label "bug"
Assistant: [Creates task]
Task DEV-46 created: "Fix password reset bug" (Priority: 2, Labels: bug)
```

## Important Guidelines

- **Always confirm before creating**: Never create tasks without user permission
- **Keep it conversational**: Don't overwhelm with questions, gather info naturally
- **Use sensible defaults**: Not every task needs all fields filled in
- **Focus on actionable work**: Only suggest tasks for concrete, implementable work
- **Parse command output**: Extract and display the task identifier from the CLI output
- **Be helpful, not pushy**: If the user declines, don't insist

## CLI Command Reference

Create a basic task:
```bash
anyt task add "Task title"
```

Create a task with all details:
```bash
anyt task add "Task title" \
  --description "Detailed description" \
  --priority 1 \
  --status todo \
  --labels "feature,urgent" \
  --phase "T3" \
  --owner "user-123" \
  --estimate 8
```
