# AnyTask CLI - Complete Command Reference

Auto-generated from Typer app introspection.

================================================================================


## anyt

--------------------------------------------------------------------------------

AnyTask - AI-native task management from the command line

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```

**Parameters:**

  **--version / -v** [Option]
    Show version and exit


**Subcommands:**

- `active`
- `artifact`
- `attempt`
- `board`
- `comment`
- `graph`
- `health`
- `init`
- `summary`
- `task`
- `timeline`
- `worker`


### anyt active

--------------------------------------------------------------------------------

Show the currently active task.

**Usage:**
```
Usage:  [OPTIONS]
```


================================================================================


### anyt artifact

--------------------------------------------------------------------------------

Manage workflow execution artifacts

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `download`


#### anyt artifact download

--------------------------------------------------------------------------------

Download artifact content.

**Usage:**
```
Usage:  [OPTIONS] ARTIFACT_ID
```

**Parameters:**

  **artifact_id** [Argument]
    Artifact ID
  **--output / -o** [Option]
    Output file path
  **--stdout** [Option]
    Print to stdout instead of file


================================================================================


================================================================================


### anyt attempt

--------------------------------------------------------------------------------

Manage workflow execution attempts

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `list`
- `show`


#### anyt attempt list

--------------------------------------------------------------------------------

List all attempts for a task.

**Usage:**
```
Usage:  [OPTIONS] TASK_ID
```

**Parameters:**

  **task_id** [Argument]
    Task identifier


================================================================================


#### anyt attempt show

--------------------------------------------------------------------------------

Show attempt details and artifacts.

**Usage:**
```
Usage:  [OPTIONS] ATTEMPT_ID
```

**Parameters:**

  **attempt_id** [Argument]
    Attempt ID


================================================================================


================================================================================


### anyt board

--------------------------------------------------------------------------------

Display tasks in a Kanban board view.

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--mine** [Option]
    Show only tasks assigned to you
  **--assignee / -a** [Option]
    Filter by assignee (user ID or agent ID)
  **--me** [Option]
    Show only my tasks (alias for --mine)
  **--labels** [Option]
    Filter by labels (comma-separated)
  **--status** [Option]
    Filter by status (comma-separated)
  **--phase** [Option]
    Filter by phase/milestone
  **--group-by** [Option]
    Group by: status, priority, owner, labels
  **--sort** [Option]
    Sort within groups: priority, updated_at
  **--compact** [Option]
    Compact display mode
  **--limit** [Option]
    Max tasks per lane
  **--json** [Option]
    Output in JSON format


================================================================================


### anyt comment

--------------------------------------------------------------------------------

Manage task comments

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `add`
- `list`


#### anyt comment add

--------------------------------------------------------------------------------

Add a comment to a task.

Examples:
    # Add comment to active task
    anyt comment add -m "Completed implementation"

    # Add comment to specific task
    anyt comment add DEV-123 -m "Found edge case"

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42). Uses active task if not provided.
  **--message / -m** [Option]
    Comment content
  **--json** [Option]
    Output as JSON


================================================================================


#### anyt comment list

--------------------------------------------------------------------------------

List all comments on a task.

Examples:
    # List comments on active task
    anyt comment list

    # List comments on specific task
    anyt comment list DEV-123

    # JSON output
    anyt comment list DEV-123 --json

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42). Uses active task if not provided.
  **--json** [Option]
    Output as JSON


================================================================================


================================================================================


### anyt graph

--------------------------------------------------------------------------------

Visualize task dependencies as ASCII art or DOT format.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier to show dependencies for (shows all if not specified)
  **--format** [Option]
    Output format: ascii, dot, json
  **--status** [Option]
    Filter by status (comma-separated)
  **--priority-min** [Option]
    Filter by minimum priority
  **--labels** [Option]
    Filter by labels (comma-separated)
  **--phase** [Option]
    Filter by phase/milestone
  **--mine** [Option]
    Show only tasks assigned to you
  **--depth** [Option]
    Max dependency depth to show
  **--compact** [Option]
    Compact display mode
  **--json** [Option]
    Output in JSON format


================================================================================


### anyt health

--------------------------------------------------------------------------------

Check backend server health

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `check`


#### anyt health check

--------------------------------------------------------------------------------

Check if the AnyTask backend server is healthy.

Calls the /health endpoint on the configured API server
and displays the server status.

Examples:
    anyt health          # Check current environment
    anyt health check    # Explicit check command

**Usage:**
```
Usage:  [OPTIONS]
```


================================================================================


================================================================================


### anyt init

--------------------------------------------------------------------------------

Initialize AnyTask in the current directory.

Requires ANYT_API_KEY environment variable to be set.
Creates .anyt/ directory structure and anyt.json configuration.

Examples:
    export ANYT_API_KEY=anyt_agent_...
    anyt init                                    # Production API (https://api.anyt.dev)
    anyt init --dev                              # Development API (http://localhost:8000)
    anyt init --workspace-id 123 --identifier DEV  # Link to specific workspace

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--workspace-id / -w** [Option]
    Workspace ID to link to
  **--workspace-name / -n** [Option]
    Workspace name (optional)
  **--identifier / -i** [Option]
    Workspace identifier (e.g., DEV, PROJ)
  **--dir / -d** [Option]
    Directory to initialize (default: current)
  **--dev** [Option]
    Use development API (http://localhost:8000)


================================================================================


### anyt summary

--------------------------------------------------------------------------------

Generate workspace summary with done, active, blocked, and next priorities.

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--period** [Option]
    Summary period: today, weekly, monthly
  **--phase** [Option]
    Filter by phase/milestone
  **--format** [Option]
    Output format: text, markdown, json
  **--json** [Option]
    Output in JSON format


================================================================================


### anyt task

--------------------------------------------------------------------------------

Manage tasks

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `add`
- `bulk-update`
- `create`
- `dep`
- `done`
- `edit`
- `list`
- `note`
- `pick`
- `rm`
- `share`
- `show`
- `suggest`


#### anyt task add

--------------------------------------------------------------------------------

Create a new task.

**Usage:**
```
Usage:  [OPTIONS] TITLE
```

**Parameters:**

  **title** [Argument]
    Task title
  **-d / --description** [Option]
    Task description
  **--phase** [Option]
    Phase/milestone identifier (e.g., T3, Phase 1)
  **-p / --priority** [Option]
    Priority (-2 to 2, default: 0)
  **--labels** [Option]
    Comma-separated labels
  **--status** [Option]
    Task status (default: backlog)
  **--owner** [Option]
    Assign to user or agent ID
  **--estimate** [Option]
    Time estimate in hours
  **--project** [Option]
    Project ID (uses current/default project if not specified)
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task bulk-update

--------------------------------------------------------------------------------

Update multiple tasks at once.

Examples:
    anyt task bulk-update DEV-1,DEV-2,DEV-3 --status in_progress
    anyt task bulk-update DEV-4,DEV-5 --priority 2
    anyt task bulk-update DEV-6,DEV-7,DEV-8 --assignee john --yes
    anyt task bulk-update DEV-9,DEV-10 --status todo --priority 1

**Usage:**
```
Usage:  [OPTIONS] TASK_IDS
```

**Parameters:**

  **task_ids** [Argument]
    Comma-separated task identifiers (e.g., 'DEV-1,DEV-2,DEV-3')
  **--status / -s** [Option]
    New status for all tasks
  **--priority / -p** [Option]
    New priority for all tasks (-2 to 2)
  **--assignee / -a** [Option]
    Assignee username or ID
  **--project** [Option]
    Project identifier or ID
  **--yes / -y** [Option]
    Skip confirmation prompt
  **--json** [Option]
    Output as JSON


================================================================================


#### anyt task create

--------------------------------------------------------------------------------

Create a new task from a template.

Opens the template in your editor ($EDITOR) for customization before creating the task.
The template content will be stored in the task's description field.

**Usage:**
```
Usage:  [OPTIONS] TITLE
```

**Parameters:**

  **title** [Argument]
    Task title
  **--template / -t** [Option]
    Template name to use (default: default)
  **--phase** [Option]
    Phase/milestone identifier (e.g., T3, Phase 1)
  **-p / --priority** [Option]
    Priority (-2 to 2, default: 0)
  **--project** [Option]
    Project ID (uses current/default project if not specified)
  **--no-edit** [Option]
    Skip opening editor, use template as-is
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task dep

--------------------------------------------------------------------------------

Manage task dependencies

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `add`
- `list`
- `rm`


##### anyt task dep add

--------------------------------------------------------------------------------

Add dependency/dependencies to a task.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **--on** [Option]
    Task(s) this depends on (comma-separated identifiers)
  **identifier** [Argument]
    Task identifier (e.g., DEV-43) or ID. Uses active task if not specified.
  **--json** [Option]
    Output in JSON format


================================================================================


##### anyt task dep list

--------------------------------------------------------------------------------

List task dependencies and dependents.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-43) or ID. Uses active task if not specified.
  **--json** [Option]
    Output in JSON format


================================================================================


##### anyt task dep rm

--------------------------------------------------------------------------------

Remove dependency/dependencies from a task.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **--on** [Option]
    Task(s) to remove dependency on (comma-separated identifiers)
  **identifier** [Argument]
    Task identifier (e.g., DEV-43) or ID. Uses active task if not specified.
  **--json** [Option]
    Output in JSON format


================================================================================


================================================================================


#### anyt task done

--------------------------------------------------------------------------------

Mark one or more tasks as done.

Optionally add a completion note to the task's Events section.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIERS]...
```

**Parameters:**

  **identifiers** [Argument]
    Task identifier(s) (e.g., DEV-42 DEV-43). Uses active task if not specified.
  **--note / -n** [Option]
    Add a completion note to the task
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task edit

--------------------------------------------------------------------------------

Edit a task's fields.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42) or ID. Uses active task if not specified.
  **--title** [Option]
    New title
  **-d / --description** [Option]
    New description
  **--status** [Option]
    New status
  **-p / --priority** [Option]
    New priority (-2 to 2)
  **--labels** [Option]
    Comma-separated labels (replaces all labels)
  **--owner** [Option]
    New owner ID
  **--estimate** [Option]
    New time estimate in hours
  **--ids** [Option]
    Multiple task IDs to edit (comma-separated)
  **--if-match** [Option]
    Expected version for optimistic concurrency control
  **--dry-run** [Option]
    Preview changes without applying
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task list

--------------------------------------------------------------------------------

List tasks with filtering.

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--status** [Option]
    Filter by status (comma-separated)
  **--phase** [Option]
    Filter by phase/milestone
  **--mine** [Option]
    Show only tasks assigned to you
  **--assignee / -a** [Option]
    Filter by assignee (user ID or agent ID)
  **--me** [Option]
    Show only my tasks (alias for --mine)
  **--labels** [Option]
    Filter by labels (comma-separated)
  **--sort** [Option]
    Sort field (priority, updated_at, created_at, status)
  **--order** [Option]
    Sort order (asc/desc)
  **--limit** [Option]
    Max number of tasks to show
  **--offset** [Option]
    Pagination offset
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task note

--------------------------------------------------------------------------------

Add a timestamped note/event to a task's description.

DEPRECATED: Use 'anyt comment add' instead for better structure and timestamps.

The note will be appended to the Events section of the task description
with a timestamp.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42) or use active task
  **--message / -m** [Option]
    Note message to append
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task pick

--------------------------------------------------------------------------------

Pick a task to work on (sets as active task).

If identifier is provided, picks that specific task.
Otherwise, shows an interactive picker to select a task.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42) or ID. Leave empty for interactive picker.
  **--status** [Option]
    Filter by status (comma-separated)
  **--project** [Option]
    Filter by project ID
  **--mine** [Option]
    Show only tasks assigned to you
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task rm

--------------------------------------------------------------------------------

Delete one or more tasks (soft delete).

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIERS]...
```

**Parameters:**

  **identifiers** [Argument]
    Task identifier(s) (e.g., DEV-42 DEV-43). Uses active task if not specified.
  **--force / -f** [Option]
    Skip confirmation prompt
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task share

--------------------------------------------------------------------------------

Generate a shareable link for a task.

Creates a public URL that can be shared with anyone who has access to the task.
The link uses the task's UID for global accessibility.

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42, t_1Z for UID). Uses active task if not specified.
  **--copy / -c** [Option]
    Copy link to clipboard
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task show

--------------------------------------------------------------------------------

Show detailed information about a task.

Supports both workspace-scoped identifiers (DEV-42) and UIDs (t_1Z).

**Usage:**
```
Usage:  [OPTIONS] [IDENTIFIER]
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42, t_1Z for UID). Uses active task if not specified.
  **--workspace / -w** [Option]
    Workspace identifier or ID (uses current workspace if not specified)
  **--show-metadata** [Option]
    Display workflow execution metadata
  **--json** [Option]
    Output in JSON format


================================================================================


#### anyt task suggest

--------------------------------------------------------------------------------

Suggest tasks to work on next based on priority and dependencies.

Uses the backend's suggestion algorithm to recommend tasks that are ready
to work on (all dependencies complete). Results are sorted by priority.

By default, only shows unassigned tasks. Use --include-assigned to see
tasks that are already assigned.

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--limit** [Option]
    Number of suggestions to return
  **--status** [Option]
    Filter by status (comma-separated)
  **--include-assigned** [Option]
    Include already-assigned tasks
  **--json** [Option]
    Output in JSON format


================================================================================


================================================================================


### anyt timeline

--------------------------------------------------------------------------------

Show chronological timeline of task events, attempts, and artifacts.

**Usage:**
```
Usage:  [OPTIONS] IDENTIFIER
```

**Parameters:**

  **identifier** [Argument]
    Task identifier (e.g., DEV-42)
  **--since** [Option]
    Show events since date (YYYY-MM-DD)
  **--last** [Option]
    Show events from last N hours/days (e.g., 24h, 7d)
  **--compact** [Option]
    Compact format
  **--json** [Option]
    Output in JSON format


================================================================================


### anyt worker

--------------------------------------------------------------------------------

Automated task worker commands

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `list-workflows`
- `secret`
- `start`
- `validate-workflow`


#### anyt worker list-workflows

--------------------------------------------------------------------------------

List available workflows.

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--workflows** [Option]
    Workflows directory (default: .anyt/workflows)


================================================================================


#### anyt worker secret

--------------------------------------------------------------------------------

Manage workflow secrets

**Usage:**
```
Usage:  [OPTIONS] COMMAND [ARGS]...
```


**Subcommands:**

- `delete`
- `get`
- `set`
- `test`


##### anyt worker secret delete

--------------------------------------------------------------------------------

Delete a secret from the keyring.

Example:
    anyt worker secret delete OLD_API_KEY
    anyt worker secret delete OLD_API_KEY --yes

**Usage:**
```
Usage:  [OPTIONS] NAME
```

**Parameters:**

  **name** [Argument]
    Secret name to delete
  **--yes / -y** [Option]
    Skip confirmation


================================================================================


##### anyt worker secret get

--------------------------------------------------------------------------------

Retrieve a secret from the keyring.

By default, the secret value is masked. Use --show to display it.

Example:
    anyt worker secret get API_KEY
    anyt worker secret get API_KEY --show

**Usage:**
```
Usage:  [OPTIONS] NAME
```

**Parameters:**

  **name** [Argument]
    Secret name to retrieve
  **--show** [Option]
    Show the secret value


================================================================================


##### anyt worker secret set

--------------------------------------------------------------------------------

Store a secret securely in the system keyring.

If value is not provided, you will be prompted to enter it securely.

Example:
    anyt worker secret set PRODUCTION_API_KEY --value abc123
    anyt worker secret set DB_PASSWORD  # Will prompt

**Usage:**
```
Usage:  [OPTIONS] NAME
```

**Parameters:**

  **name** [Argument]
    Secret name (e.g., API_KEY)
  **--value / -v** [Option]
    Secret value (or will prompt)


================================================================================


##### anyt worker secret test

--------------------------------------------------------------------------------

Test secret interpolation with a sample text.

This is useful for verifying your secrets are configured correctly.

Example:
    anyt worker secret test "API_KEY=\${{ secrets.API_KEY }}"

**Usage:**
```
Usage:  [OPTIONS] TEXT
```

**Parameters:**

  **text** [Argument]
    Text with secret placeholders to test


================================================================================


================================================================================


#### anyt worker start

--------------------------------------------------------------------------------

Start the Claude task worker.

The worker continuously polls for tasks and executes workflows automatically.

Setup:
1. Set ANYT_API_KEY environment variable for API authentication
2. Get your agent identifier from https://anyt.dev/home/agents
3. Pass agent identifier via --agent-id flag

Note: ANYT_API_KEY (API key) and agent-id (agent identifier) are different:
- ANYT_API_KEY: API key for authentication (e.g., anyt_agent_...)
- agent-id: Agent identifier to filter tasks (e.g., agent-xxx)

Workflow Options:
- No options: Runs ALL workflows from .anyt/workflows/
- --workflow local_dev: Runs ONLY local_dev workflow (from package or workspace)
- --workflows-dir /custom: Runs ALL workflows from /custom directory
- --workflow local_dev --workflows-dir /custom: Runs ONLY local_dev from /custom

Built-in workflows (bundled with CLI, no setup required):
- local_dev: Direct implementation on current repository
- feature_development: Full feature workflow with branch management
- general_task: General-purpose task execution

Example:
    export ANYT_API_KEY=anyt_agent_...  # API key for authentication
    anyt worker start --agent-id agent-xxx  # Agent identifier from https://anyt.dev/home/agents
    anyt worker start --workflow local_dev --agent-id agent-xxx
    anyt worker start --agent-id agent-xxx --project-id 123
    anyt worker start --poll-interval 10 --workspace /path/to/project --agent-id agent-xxx

**Usage:**
```
Usage:  [OPTIONS]
```

**Parameters:**

  **--workspace / -w** [Option]
    Workspace directory (default: current directory)
  **--workflow** [Option]
    Built-in workflow to run (local_dev, feature_development, general_task). When specified, runs ONLY this workflow.
  **--workflows** [Option]
    Workflows directory (default: .anyt/workflows). If --workflow is specified, loads from this directory.
  **--poll-interval / -i** [Option]
    Polling interval in seconds
  **--max-backoff** [Option]
    Maximum backoff interval in seconds
  **--agent-id / -a** [Option]
    Agent identifier to filter tasks (e.g., agent-xxx). Get from https://anyt.dev/home/agents.
  **--project-id / -p** [Option]
    Project ID to scope task suggestions. If not provided, loads from .anyt/anyt.json (current_project_id field).


================================================================================


#### anyt worker validate-workflow

--------------------------------------------------------------------------------

Validate a workflow definition.

**Usage:**
```
Usage:  [OPTIONS] WORKFLOW_FILE
```

**Parameters:**

  **workflow_file** [Argument]
    Workflow file to validate


================================================================================


================================================================================


================================================================================
