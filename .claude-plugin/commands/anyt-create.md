# Create AnyTask Task

You are helping the user create a new task in AnyTask using the CLI.

## Your Process

1. **Gather Task Information**
   Ask the user for the following details (use sensible defaults if not provided):
   - **Title** (required): What is the task about?
   - **Description** (optional): Any additional details?
   - **Priority** (optional): Priority level from -2 (lowest) to 2 (highest), default is 0
   - **Status** (optional): backlog, todo, in_progress, or done (default: backlog)
   - **Phase** (optional): Phase/milestone identifier (e.g., T3, Phase 1)
   - **Labels** (optional): Comma-separated labels
   - **Owner** (optional): User or agent ID to assign the task to
   - **Estimate** (optional): Time estimate in hours

2. **Create the Task**
   Use the Bash tool to create the task with the provided information:
   ```bash
   anyt task add "<title>" \
     --description "<description>" \
     --priority <priority> \
     --status <status> \
     --phase "<phase>" \
     --labels "<labels>" \
     --owner "<owner>" \
     --estimate <hours>
   ```

   Only include flags for information that was provided by the user.

3. **Display Results**
   - Parse the command output to show the created task details
   - Display the task identifier (e.g., DEV-42)
   - Show key fields: title, status, priority, and description

4. **Offer Next Actions**
   Ask the user:
   - "Would you like to start working on this task now? I can pick it for you."
   - "Would you like to add dependencies to this task?"
   - "Would you like to create another task?"

## Example Usage

**Basic task creation:**
```bash
anyt task add "Implement OAuth authentication"
```

**Task with full details:**
```bash
anyt task add "Implement OAuth authentication" \
  --description "Add OAuth 2.0 support for Google and GitHub" \
  --priority 2 \
  --status todo \
  --labels "feature,auth" \
  --estimate 8
```

## Important Notes

- Always use the Bash tool to run `anyt` commands
- The title argument is required and should be quoted if it contains spaces
- Priority ranges from -2 (lowest) to 2 (highest)
- Status options: backlog, todo, in_progress, done
- Parse the output to extract the task identifier (e.g., DEV-42)
- Offer to pick the task for the user after creation
