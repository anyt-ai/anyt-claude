---
name: anytask-context
description: Automatically loads and displays task details when user mentions task identifiers like DEV-42 or t_123. Provides context about tasks being discussed.
---

# AnyTask Context Loader

You automatically fetch and display task details when users reference tasks by their identifier.

## When to Activate

Activate this skill when the user:
- Mentions a task identifier (DEV-42, PROJ-123, WORK-5, etc.)
- References a task by UID (t_123, t_456abc, etc.)
- Asks about a specific task ("what's DEV-42?", "tell me about task DEV-50")
- Says "the task" or "that task" after previously mentioning a specific identifier
- Wants to see task details, status, or context

## When NOT to Activate

Do not activate for:
- General task discussions without specific identifiers
- When creating new tasks
- When listing or filtering tasks (use list/board commands instead)
- When the task details are already visible in the current conversation

## Your Process

1. **Detect identifier**
   Extract the task identifier from the conversation:
   - Workspace-scoped format: `[A-Z]+-[0-9]+` (e.g., DEV-42, PROJ-123)
   - UID format: `t_[a-zA-Z0-9]+` (e.g., t_1Z, t_abc123)

2. **Fetch task details**
   Use the Bash tool to get task information:
   ```bash
   anyt task show <identifier>
   ```

   This returns:
   - Task ID and identifier
   - Title and description
   - Status, priority, phase
   - Assignee/owner
   - Labels and estimates
   - Dependencies and blockers
   - Creation and update timestamps

3. **Display context concisely**
   Present information relevant to the conversation. Don't overwhelm with every detail.

   **Minimal format** (for quick references):
   ```
   DEV-42: Implement OAuth callback
   Status: todo | Priority: 2 (Urgent) | Assigned to: @john
   ```

   **Detailed format** (when asked explicitly or when details matter):
   ```
   DEV-42: Implement OAuth callback

   Status: todo
   Priority: 2 (Urgent)
   Phase: T3 - Authentication
   Assigned to: @john
   Labels: feature, auth, security

   Description:
   Implement the OAuth 2.0 callback endpoint that handles...

   Dependencies:
   - Depends on: DEV-40 (Complete) ✓
   - Blocks: DEV-45, DEV-46

   Created: 2025-01-15
   Updated: 2025-01-20
   ```

4. **Offer relevant actions**
   Based on the context, suggest appropriate actions:
   - "Would you like to pick this task?"
   - "Should I show the dependencies?"
   - "Want to update the status?"
   - "Need to see related tasks?"

5. **Handle multiple identifiers**
   If the user mentions multiple tasks, show a summary of each:
   ```
   I found 3 tasks you mentioned:

   1. DEV-42: Implement OAuth callback (todo, priority 2)
   2. DEV-45: Add user profile (in_progress, priority 1)
   3. DEV-50: Refactor auth module (done, priority 0)

   Would you like details on any specific one?
   ```

## Example Interactions

**Example 1: Single task reference**
```
User: Can you help me with DEV-42?
Assistant: [Runs: anyt task show DEV-42]
Sure! DEV-42: Implement OAuth callback

Status: todo
Priority: 2 (Urgent)
Description: Implement the OAuth 2.0 callback endpoint...

This task depends on DEV-40 which is already complete. Would you like to pick this task?
```

**Example 2: Comparing tasks**
```
User: What's the difference between DEV-42 and DEV-45?
Assistant: [Runs: anyt task show DEV-42 && anyt task show DEV-45]
Let me get details on both tasks:

DEV-42: Implement OAuth callback
- Status: todo | Priority: 2 (Urgent)
- Depends on: DEV-40 (complete)
- About: OAuth 2.0 callback endpoint

DEV-45: Add user profile page
- Status: in_progress | Priority: 1 (High)
- Assigned to: @sarah
- About: User profile UI with settings

DEV-42 is higher priority and is currently unassigned, while DEV-45 is already being worked on.
```

**Example 3: Using UID**
```
User: Show me task t_1Z
Assistant: [Runs: anyt task show t_1Z]
Task t_1Z (DEV-42): Implement OAuth callback
[Shows details]
```

**Example 4: Quick status check**
```
User: Is DEV-42 done yet?
Assistant: [Runs: anyt task show DEV-42]
DEV-42 is currently in "todo" status, not yet completed. It's assigned to @john with priority 2 (Urgent).
```

## Advanced Options

**Show with metadata:**
```bash
anyt task show <identifier> --show-metadata
```

**Show specific workspace:**
```bash
anyt task show <identifier> --workspace DEV
```

**Get dependencies:**
```bash
anyt task dep list <identifier>
```

**Get task comments:**
```bash
anyt comment list <identifier>
```

**View JSON output:**
```bash
anyt task show <identifier> --json
```

## Important Guidelines

- **Be concise**: Show only relevant details for the context
- **Parse carefully**: Extract and format information cleanly
- **Handle errors gracefully**: If task not found, explain clearly
- **Don't repeat**: If task details were just shown, don't fetch again
- **Suggest actions**: Offer next steps based on task status
- **Respect privacy**: Be aware that some task details might be sensitive
- **Format cleanly**: Use clear, readable formatting

## CLI Command Reference

Show task details:
```bash
anyt task show <identifier>
anyt task show <identifier> --json
anyt task show <identifier> --show-metadata
```

Show task by UID:
```bash
anyt task show t_123
```

List task dependencies:
```bash
anyt task dep list <identifier>
```

List task comments:
```bash
anyt comment list <identifier>
```

View active task:
```bash
anyt active
```

## Edge Cases

**Task not found:**
```
I couldn't find task DEV-99. Would you like to:
- See available tasks: anyt task list
- Create a new task: /anytask:create
```

**Ambiguous reference:**
```
User: "the task"
Assistant: Which task are you referring to? I see several mentioned recently:
- DEV-42: Implement OAuth callback
- DEV-45: Add user profile
```

**No workspace configured:**
```
It looks like you don't have a workspace configured. Run:
anyt workspace list
anyt workspace select <identifier>
```
