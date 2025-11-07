# Get Next Task to Work On

You are helping the user find and start working on their next task using intelligent AI-powered suggestions.

## Your Process

1. **Get Task Suggestions**
   Use the Bash tool to get intelligent task recommendations:
   ```bash
   anyt task suggest --limit 5
   ```

   This command uses the backend's AI-powered suggestion algorithm that:
   - Prioritizes tasks by importance and dependencies
   - Shows only tasks that are ready to work on (dependencies met)
   - By default shows unassigned tasks
   - Sorts by priority and readiness

2. **Parse and Display Recommendations**
   Analyze the output and present tasks in a clear, readable format:
   - Show task identifier and title
   - Display priority level (2 = highest, -2 = lowest)
   - Show current status
   - Highlight dependencies and blockers
   - Explain why each task is recommended (e.g., "High priority, no blockers")

   Format the display like this:
   ```
   Top Task Recommendations:

   1. DEV-42: Implement OAuth callback
      Priority: 2 (Urgent)
      Status: todo
      Why: High priority, all dependencies complete, unblocks 2 other tasks

   2. DEV-38: Add user profile page
      Priority: 1 (High)
      Status: todo
      Why: Ready to start, no dependencies
   ```

3. **Help User Select a Task**
   Ask the user which task they'd like to work on:
   - "Which task would you like to pick?"
   - "Enter the task number (1-5) or task identifier (e.g., DEV-42)"
   - Offer options to see more details about any task before picking

4. **Pick the Selected Task**
   Once the user chooses, use the Bash tool to pick the task:
   ```bash
   anyt task pick <identifier>
   ```

   This sets the task as the active task for the current session.

5. **Display Task Details and Context**
   After picking the task, show:
   - Full task details including description
   - Acceptance criteria (if available)
   - Dependencies that need to be considered
   - Suggest: "I'm ready to help you implement this task. What would you like to start with?"

## Advanced Options

If the user wants to customize the suggestions:

**Show more suggestions:**
```bash
anyt task suggest --limit 10
```

**Include already-assigned tasks:**
```bash
anyt task suggest --limit 5 --include-assigned
```

**Filter by status:**
```bash
anyt task suggest --status todo,in_progress --limit 5
```

## Alternative: View Active Task

If the user already has an active task, you can check it with:
```bash
anyt active
```

If there's an active task, ask: "You're currently working on [TASK]. Would you like to continue with this or pick a different one?"

## Important Notes

- Always use the Bash tool to run `anyt` commands
- Parse the output carefully to extract task details
- Make recommendations based on priority, dependencies, and readiness
- After picking a task, offer to help implement it immediately
- If no suggestions are available, explain why and suggest creating a new task with `/anytask:create`

## Example Workflow

```
User: /anyt-next

Assistant: Let me get task suggestions for you...

(Runs: anyt task suggest --limit 5)

Top Task Recommendations:

1. DEV-42: Implement OAuth callback
   Priority: 2 (Urgent) | Status: todo
   Why: High priority, all dependencies complete

2. DEV-38: Add user profile page
   Priority: 1 (High) | Status: todo
   Why: Ready to start, no dependencies

Which task would you like to work on?

User: Let's do DEV-42

Assistant: Great choice\! Let me pick that task for you...

(Runs: anyt task pick DEV-42)

Task DEV-42 is now your active task. I'm ready to help you implement the OAuth callback. What would you like to start with?
```
