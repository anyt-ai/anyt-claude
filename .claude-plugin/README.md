# AnyTask Claude Code Plugin

AI-native task management for Claude Code. Create, track, and work on tasks with intelligent workflows.

## Overview

The AnyTask Claude Code Plugin seamlessly integrates AnyTask's powerful task management system directly into your Claude Code workflow. Get AI-powered task suggestions, create tasks on the fly, and work more efficiently with context-aware assistance.

## Features

- **Slash Commands**: Explicit commands for creating tasks and getting recommendations
- **Automatic Skills**: Background skills that activate based on conversation context
- **Intelligent Task Creation**: Create tasks with full metadata through conversational interface
- **AI-Powered Suggestions**: Get smart recommendations for your next task based on priority and dependencies
- **Active Task Tracking**: Automatically pick and track the task you're working on
- **Context Loading**: Automatic task details when you mention identifiers
- **Workflow Management**: Automatic completion detection and next-task suggestions
- **Seamless Workflow**: All actions happen within Claude Code - no context switching

## Commands

### `/anyt-create` - Create New Task

Create a new task in AnyTask with full control over all task properties.

**What it does:**
- Guides you through task creation with prompts for title, description, priority, status, labels, and more
- Creates the task using the AnyTask CLI
- Offers to pick the task immediately so you can start working on it

**Example usage:**
```
User: /anyt-create
Claude: I'll help you create a new task. What would you like the task title to be?
User: Implement OAuth authentication
Claude: Great! Would you like to add any additional details?
  - Description: (optional)
  - Priority: -2 to 2, default is 0
  - Status: backlog, todo, in_progress, or done
  - Labels: comma-separated tags
  ...
```

### `/anyt-next` - Get Next Task Recommendation

Get intelligent AI-powered suggestions for your next task and start working on it.

**What it does:**
- Fetches task suggestions using AnyTask's recommendation algorithm
- Shows top 5 tasks prioritized by importance, dependencies, and readiness
- Helps you pick a task and sets it as your active task
- Loads full task context so you can start implementing immediately

**Example usage:**
```
User: /anyt-next
Claude: Let me get task suggestions for you...

Top Task Recommendations:

1. DEV-42: Implement OAuth callback
   Priority: 2 (Urgent) | Status: todo
   Why: High priority, all dependencies complete

2. DEV-38: Add user profile page
   Priority: 1 (High) | Status: todo
   Why: Ready to start, no dependencies

Which task would you like to work on?
```

## Skills (Automatic Assistance)

Skills work in the background and activate automatically when relevant. You don't need to invoke them explicitly—Claude detects when they're needed.

### `anytask-creator` - Automatic Task Creation

Detects when you describe work that should be tracked as a task.

**Activates when you:**
- Describe work to be done ("we need to implement X")
- Mention features to build ("let's add authentication")
- Identify bugs to fix ("there's an issue with the login")
- Plan future work ("I should refactor this later")

**What it does:**
- Detects the work item in your conversation
- Asks for confirmation before creating a task
- Guides you through adding details (priority, labels, etc.)
- Creates the task using `anyt task add`
- Offers to pick the task so you can start immediately

**Example:**
```
User: We really need to add OAuth authentication to the API
Claude: I noticed you mentioned adding OAuth authentication. Would you like me to create a task for this in AnyTask?
User: Yes, that would be helpful
Claude: Great! What priority should this have? (-2 to 2, default is 0)
User: It's pretty important, let's say 1
Claude: [Creates task]
Task DEV-45 created: "Add OAuth authentication to API"
Would you like to start working on this task now?
```

### `anytask-workflow` - Workflow Management

Helps manage your task workflow by detecting completions and suggesting next steps.

**Activates when you:**
- Complete a piece of work ("done", "finished")
- Ask "what's next?" or "what should I work on?"
- Finish implementing a feature
- Seem to be wrapping up current work

**What it does:**
- Checks your active task when you complete work
- Asks if you want to mark it as done
- Suggests next tasks using AI recommendations
- Helps you pick the next task to maintain momentum
- Manages smooth transitions between tasks

**Example:**
```
User: Okay, I've finished the OAuth implementation and all tests are passing
Claude: Great! It looks like you've completed work on DEV-45: Add OAuth authentication to API. Should I mark it as done?
User: Yes
Claude: [Marks task as done]
Excellent! Here are your top 3 recommended tasks:
1. DEV-46: Fix password reset bug (Priority: 2)
2. DEV-42: Implement OAuth callback (Priority: 1)

Would you like to pick one of these to work on next?
```

### `anytask-context` - Automatic Context Loading

Loads task details automatically when you mention task identifiers.

**Activates when you:**
- Mention a task identifier (DEV-42, PROJ-123, etc.)
- Reference a task by UID (t_123)
- Ask about a specific task
- Say "the task" after mentioning an identifier

**What it does:**
- Detects task identifiers in conversation
- Fetches and displays task details
- Shows status, priority, dependencies, and description
- Offers relevant actions (pick, update, etc.)
- Handles multiple tasks mentioned at once

**Example:**
```
User: Can you help me with DEV-42?
Claude: Sure! DEV-42: Implement OAuth callback

Status: todo
Priority: 2 (Urgent)
Description: Implement the OAuth 2.0 callback endpoint...

This task depends on DEV-40 which is already complete. Would you like to pick this task?
```

## Installation

### Prerequisites

1. **Install AnyTask CLI**: The plugin requires the `anyt` CLI to be installed and configured.

   ```bash
   # Using uvx (recommended)
   uvx anyt --help

   # Or using pipx
   pipx install anyt
   ```

2. **Configure AnyTask**: Set up your API key and initialize your workspace.

   ```bash
   # Set your API key (get from https://anyt.dev)
   export ANYT_API_KEY=anyt_agent_...

   # Initialize AnyTask in your project directory
   cd /path/to/your/project
   anyt init

   # Or initialize with specific workspace
   anyt init --workspace-id 123 --identifier DEV
   ```

   For development/local server:
   ```bash
   # Use development API
   anyt init --dev
   ```

### Install Plugin

#### Option 1: Install from GitHub (Recommended)

```bash
# In Claude Code terminal
/plugin install https://github.com/anyt-ai/anyt-claude
```

#### Option 2: Install from Local Directory (Development)

```bash
# In Claude Code terminal, from the AnyTaskCLI repository
/plugin install .
```

#### Option 3: Add to Marketplace

1. Create or edit `~/.claude/config/marketplaces.json`:

```json
{
  "marketplaces": [
    {
      "name": "AnyTask",
      "url": "https://raw.githubusercontent.com/anyt-ai/anyt-claude/main/.claude-plugin/marketplace.json"
    }
  ]
}
```

2. In Claude Code:
```
/plugin marketplace add anytask-plugins
/plugin install anytask-claude
```

## Quick Start

1. **Create your first task:**
   ```
   /anyt-create
   ```

2. **Get task recommendations:**
   ```
   /anyt-next
   ```

3. **Start working!** Claude will help you implement the task with full context awareness.

## Workflow Examples

### Daily Development Workflow

```
User: /anyt-next
Claude: [Shows top 5 recommended tasks]
User: Let's work on DEV-42
Claude: [Picks DEV-42, shows full context]
        I'm ready to help you implement the OAuth callback. What would you like to start with?
User: Let's create the callback endpoint
Claude: [Helps implement the feature]
```

### Planning and Creating Tasks

```
User: /anyt-create
Claude: What would you like the task title to be?
User: Add email verification
Claude: [Guides through task creation]
        Task DEV-50 created successfully! Would you like to start working on this now?
User: Not yet, let me create a few more tasks first
User: /anyt-create
Claude: [Ready to create another task]
```

## Configuration

The plugin uses your workspace-based AnyTask CLI configuration. Configuration is stored in `.anyt/anyt.json` within your project workspace. No global configuration file is required.

### Workspace Configuration

After initializing AnyTask in your workspace with `anyt init`, the plugin will use the workspace configuration from `.anyt/anyt.json`:

```json
{
  "workspace_id": 1,
  "workspace_name": "My Workspace",
  "identifier": "DEV",
  "current_project_id": 1
}
```

### Environment Variables

The plugin respects the same environment variables as the AnyTask CLI:
- `ANYT_API_KEY` - API key for authentication (required)
- `ANYT_ENV` - Current environment (dev/staging/prod)
- `ANYT_AUTH_TOKEN` - Authentication token (if using user auth)
- `ANYT_AGENT_KEY` - Agent API key (alternative to ANYT_API_KEY)

## Troubleshooting

### Plugin commands not working

Make sure the AnyTask CLI is installed and configured:
```bash
anyt --version
anyt auth whoami
```

### No task suggestions available

Check if you have tasks in your workspace:
```bash
anyt task list
```

If you have no tasks, create some with `/anyt-create` or directly via CLI:
```bash
anyt task add "My first task" --status todo
```

### Command returns errors

Verify your workspace configuration:
```bash
# Check if workspace is initialized
cat .anyt/anyt.json

# Verify API connection
anyt health check

# Check API key is set
echo $ANYT_API_KEY
```

## Development

### Plugin Structure

```
.claude-plugin/
├── plugin.json              # Plugin manifest
├── marketplace.json         # Marketplace configuration
├── commands/                # Slash commands (user-invoked)
│   ├── anyt-create.md      # Create task command
│   └── anyt-next.md        # Next task command
├── skills/                  # Skills (model-invoked)
│   ├── task-creator.md     # Automatic task creation
│   ├── task-workflow.md    # Workflow management
│   └── task-context.md     # Context loading
└── README.md               # This file
```

### Testing the Plugin Locally

1. Clone the repository:
   ```bash
   git clone https://github.com/anyt-ai/anyt-claude
   cd anyt-claude
   ```

2. Install in Claude Code:
   ```
   /plugin install /path/to/anyt-claude
   ```

3. Test the commands:
   ```
   /anyt-create
   /anyt-next
   ```

### Contributing

We welcome contributions! To add new commands or improve existing ones:

1. Fork the repository
2. Create a new command file in `.claude-plugin/commands/`
3. Update `plugin.json` to register the command
4. Test thoroughly with Claude Code
5. Submit a pull request

## Support

- **GitHub Issues**: https://github.com/anyt-ai/anyt-claude/issues
- **Email**: contact@anytransformer.com
- **Documentation**: https://github.com/anyt-ai/anyt-claude

## License

MIT License - see LICENSE file for details

## Related Projects

- **AnyTask CLI**: https://github.com/anyt-ai/AnyTaskCLI
- **Claude Code**: https://code.claude.com
- **Claude Code Plugin Docs**: https://code.claude.com/docs/en/plugin-marketplaces
