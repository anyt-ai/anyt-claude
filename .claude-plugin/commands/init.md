# Initialize AnyTask in Current Repository

Connect the current repository to AnyTask, enabling task management and workflow commands.

**User intent:** "Set up AnyTask in this repository" or "Connect this repo to AnyTask"

## Before You Start

**IMPORTANT:** Before executing ANY AnyTask operations, use the `_ensure_ready` skill to verify:
1. `anyt` CLI is installed
2. `ANYT_API_KEY` environment variable is set

If pre-checks fail, stop and show the error message. Only proceed if both checks pass.

Note: For the init command, skip the `.anyt/anyt.json` check since we're creating it.

## Your Process

### Step 1: Check for Existing Configuration

Check if the current directory is already initialized:

```bash
test -f .anyt/anyt.json && echo "exists" || echo "not found"
```

#### If .anyt/anyt.json EXISTS:

The repository is already initialized. Read the configuration and show current status:

```bash
cat .anyt/anyt.json
anyt active --json
anyt task list --status active,todo --limit 3 --json
```

Display to user:
```
✅ This repository is already configured with AnyTask

Current Configuration:
  Workspace: <workspace_identifier> (ID: <workspace_id>)
  Project: <project_name> (ID: <current_project_id>)

Active/Todo Tasks:
  - <task-1>
  - <task-2>
  - <task-3>

You can now use:
  • /anytask:create - Create new tasks
  • /anytask:next - Pick and work on tasks
```

**STOP here** - do not reinitialize.

#### If .anyt/anyt.json DOES NOT EXIST:

Proceed to Step 2.

### Step 2: Run anyt init

For MVP, use the simple interactive mode (let the CLI handle workspace/project selection):

```bash
anyt init
```

**Interactive Mode:**
- The CLI will prompt the user to select a workspace
- Then prompt to select a project
- This provides a good user experience with clear choices

**For Future Non-Interactive Mode (NOT MVP):**
```bash
# Future enhancement - auto-select first workspace/project
anyt init --non-interactive
```

### Step 3: Verify Initialization

After `anyt init` completes, verify the configuration was created:

```bash
test -f .anyt/anyt.json && echo "success" || echo "failed"
```

**If initialization failed:**
- Check the stderr/error output from `anyt init`
- Display the error to the user with helpful context:

```
Initialization failed. Please try running manually in your terminal:

  anyt init

If you continue to have issues, check:
  • ANYT_API_KEY is set correctly
  • You have access to at least one workspace
  • Visit: https://anyt.dev/docs/getting-started
```

**If initialization succeeded:**
- Read the configuration file to confirm details
- Proceed to Step 4

### Step 4: Show Initial Workspace Status

Get an overview of the current workspace:

```bash
anyt task list --status active,todo,backlog --limit 5 --json
```

Display a summary:

```
✅ Successfully initialized AnyTask in this repository

Configuration:
  • Workspace: <workspace_identifier>
  • Project: <project_name>

Current Tasks:
  <list up to 5 tasks if any exist, or say "No tasks yet">

Next Steps:
  • Use /anytask:create to create your first task
  • Use /anytask:next to pick and start working on tasks
  • Run 'anyt task list' to see all tasks
```

### Step 5: Offer Next Action

Ask the user:

```
Would you like to:
  1. Create your first task? (use /anytask:create)
  2. Pick an existing task to work on? (use /anytask:next)
  3. Explore available tasks? (I can show you the task board)
```

## Important Notes

- **Always use _ensure_ready**: Check environment before any operation
- **Interactive by default**: Let the CLI handle workspace/project selection for better UX
- **Don't reinitialize**: If already configured, just show current status
- **Parse CLI output**: Extract workspace and project info from JSON responses
- **Handle errors gracefully**: Provide clear guidance when initialization fails
- **Be helpful**: Guide users to next steps after successful initialization

## Example Workflow

**User:** `/anytask:init`

**Step 1: Pre-check**
```
[Uses _ensure_ready skill]
✓ anyt CLI found
✓ ANYT_API_KEY is set
```

**Step 2: Check existing config**
```bash
test -f .anyt/anyt.json && echo "exists" || echo "not found"
# Output: not found
```

**Step 3: Initialize**
```bash
anyt init
```

Output from CLI:
```
? Select a workspace:
  > My Workspace (ID: 123)
    Team Workspace (ID: 456)

? Select a project:
  > Main Project (ID: 5)
    Side Project (ID: 7)

✓ Initialized AnyTask in /path/to/repo
  Workspace: MY (ID: 123)
  Project: Main Project (ID: 5)
  Config: .anyt/anyt.json
```

**Step 4: Verify and show status**
```bash
test -f .anyt/anyt.json && echo "success" || echo "failed"
# Output: success

anyt task list --status active,todo,backlog --limit 5 --json
```

**Step 5: Display summary**
```
✅ Successfully initialized AnyTask in this repository

Configuration:
  • Workspace: MY (ID: 123)
  • Project: Main Project (ID: 5)

Current Tasks:
  - DEV-1: Setup project structure (backlog)
  - DEV-2: Implement authentication (todo)
  - DEV-3: Add tests (backlog)

Next Steps:
  • Use /anytask:create to create your first task
  • Use /anytask:next to pick and start working on tasks

Would you like to:
  1. Create your first task? (use /anytask:create)
  2. Pick an existing task to work on? (use /anytask:next)
  3. Explore available tasks? (I can show you the task board)
```

## Error Handling

### CLI Not Installed
```
It looks like the AnyTask CLI is not installed.

To install: pip install anyt
Then run /anytask:init again.
```

### API Key Not Set
```
ANYT_API_KEY is not set.

1. Get your key: https://anyt.dev/home/settings/api-keys
2. Set it: export ANYT_API_KEY=anyt_agent_...
3. Run /anytask:init again
```

### No Workspaces Available
```
You don't have access to any workspaces yet.

Please:
1. Create a workspace at https://anyt.dev
2. Run /anytask:init again
```

### Already Initialized
```
✅ This repository is already configured with AnyTask

Current workspace: MY (ID: 123)
Current project: Main Project (ID: 5)

Use /anytask:create or /anytask:next to start working.
```

## CLI Command Reference

Check CLI version:
```bash
anyt --version
```

Initialize workspace (interactive):
```bash
anyt init
```

Initialize with specific workspace/project:
```bash
anyt init --workspace-id 123 --project-id 5
```

Non-interactive mode (auto-select first options):
```bash
anyt init --non-interactive
# or
anyt init -y
```

Check configuration:
```bash
cat .anyt/anyt.json
```

List tasks:
```bash
anyt task list --json
anyt task list --status active,todo --limit 5 --json
```
