# AnyTask Claude Code Plugin

AI-native task management for Claude Code. Create, track, and work on tasks with intelligent workflows.

## Overview

The AnyTask Claude Code Plugin seamlessly integrates AnyTask's powerful task management system directly into your Claude Code workflow. Get AI-powered task suggestions, create tasks on the fly, and work more efficiently with context-aware assistance.

## Features

- **3-Command MVP Workflow**: Simple, powerful workflow with init → create → next
- **Environment Pre-checks**: Automatic validation of CLI, API key, and configuration
- **Intelligent Task Creation**: Create single tasks or multi-task epics with dependencies
- **AI-Powered Suggestions**: Get smart recommendations based on priority and dependencies
- **Automatic Workflow**: Continue active tasks or auto-pick next suggested task
- **JSON-First Design**: Reliable CLI parsing with structured JSON responses
- **Dependency Management**: Proper task dependencies with comma-separated syntax
- **Context-Aware Skills**: Background assistance that activates based on conversation
- **Seamless Integration**: All actions happen within Claude Code - no context switching

## Quick Start

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

### Installation

Install the plugin from the Claude Code marketplace:

1. Add the marketplace to Claude Code:

```bash
/plugin marketplace add anyt-ai/anyt-claude
```

2. Install the plugin:

```bash
/plugin install anytask@anytask
```

That's it! The plugin is now ready to use.

## The MVP Workflow

AnyTask Claude Code Plugin follows a simple, powerful 3-command workflow:

1. **`/anytask:init`** - Connect your repository to AnyTask (one-time setup)
2. **`/anytask:create`** - Create tasks when you identify work to be done
3. **`/anytask:next`** - Let Claude intelligently pick and implement the next task

This workflow is designed to minimize friction and maximize productivity. Each command uses JSON mode for reliable parsing and includes comprehensive pre-checks to ensure your environment is ready.

### Key Design Principles

- **Environment Validation**: All commands check for `anyt` CLI, `ANYT_API_KEY`, and configuration
- **JSON-First**: Reliable CLI parsing with structured responses
- **Intelligent Defaults**: Auto-pick first suggested task, structured descriptions
- **Dependency Management**: Proper task sequencing with comma-separated syntax
- **Clear Guidance**: Error messages provide actionable steps for resolution

## Usage

The plugin provides a simple 3-command workflow for task management:

### Core Workflow Commands

#### `/anytask:init` - Initialize AnyTask in Repository

Connect the current repository to AnyTask workspace and project.

```
User: /anytask:init
Claude: Checking environment...
✓ anyt CLI installed
✓ ANYT_API_KEY set

Running anyt init...
[Interactive workspace/project selection]

✅ Successfully initialized AnyTask
Configuration: MY (ID: 123) / Main Project (ID: 5)

Use /anytask:create to create tasks or /anytask:next to start working.
```

#### `/anytask:create` - Create Single Task

Create a well-structured task suitable for one pull request (4-8 hours of work).

```
User: /anytask:create "Add API endpoint for task comments"
Claude: ✅ Created task DEV-42: Add API endpoint for task comments

Status: backlog
Estimated: 4 hours

Key objectives:
  • POST /api/tasks/{id}/comments endpoint
  • Database persistence with timestamps
  • Tests and error handling

Would you like to start working on this task now?
```

#### `/anytask:next` - Continue or Pick Next Task

Intelligent workflow that continues active tasks or auto-picks the next recommended task.

```
User: /anytask:next
Claude: Checking for active task...
No active task found.

I found 3 recommended tasks. Starting with the highest priority:

DEV-42: Add API endpoint for task comments
  Priority: 1 (High)
  Status: todo
  Estimate: 4 hours

Picking this task and starting implementation...
[Claude explores codebase, implements features, writes tests]
```

### Advanced Command

#### `/anytask:create-epic` - Break Down Large Features

Break down a large feature into multiple tasks with proper dependencies.

```
User: /anytask:create-epic "Build user authentication with OAuth and JWT"
Claude: Analyzing codebase and breaking down the epic...

✅ Epic created successfully!

Created 7 tasks with total estimated effort: 32 hours

Task breakdown (in dependency order):
1. DEV-42: Create user and session database schema (4h) - No dependencies
2. DEV-43: Implement JWT token service (6h) - Depends on: DEV-42
3. DEV-44: Integrate Google OAuth provider (5h) - Depends on: DEV-42, DEV-43
...

Use /anytask:next to start working on the first available task.
```

### Skills (Automatic Assistance)

Skills work in the background and activate automatically when relevant:

- **`anytask-creator`**: Detects when you describe work that should be tracked as a task
- **`anytask-workflow`**: Manages task workflow by detecting completions and suggesting next steps
- **`anytask-context`**: Loads task details automatically when you mention task identifiers

## Development

### Plugin Structure

```
.claude-plugin/
├── plugin.json              # Plugin manifest
├── marketplace.json         # Marketplace configuration
├── commands/                # Slash commands (user-invoked)
│   ├── init.md             # Initialize repository command
│   ├── create.md           # Create single task command
│   ├── next.md             # Continue/pick next task command
│   └── create-epic.md      # Create multi-task epic command
├── skills/                  # Skills (model-invoked & internal)
│   ├── ensure-ready.md     # Pre-check skill (internal)
│   ├── task-creator.md     # Automatic task creation
│   ├── task-workflow.md    # Workflow management
│   └── task-context.md     # Context loading
└── README.md               # Full documentation
```

### Testing the Plugin Locally

For development and testing:

1. Clone the repository:
   ```bash
   git clone https://github.com/anyt-ai/anyt-claude
   cd anyt-claude
   ```

2. Install in Claude Code:
   ```
   /plugin install .
   ```

3. Test the commands:
   ```
   /anytask:init
   /anytask:create
   /anytask:next
   /anytask:create-epic
   ```

4. After making changes, reinstall:
   ```
   /plugin uninstall anytask
   /plugin install .
   ```

### Contributing

We welcome contributions! To add new commands or improve existing ones:

1. Fork the repository
2. Create a new command file in `.claude-plugin/commands/` or skill in `.claude-plugin/skills/`
3. Update `plugin.json` to register the command/skill
4. Test thoroughly with Claude Code
5. Submit a pull request

## Documentation

For complete documentation, see [.claude-plugin/README.md](.claude-plugin/README.md).

## Support

- **GitHub Issues**: https://github.com/anyt-ai/anyt-claude/issues
- **Email**: contact@anytransformer.com
- **AnyTask CLI**: https://github.com/anyt-ai/AnyTaskCLI
- **Claude Code Docs**: https://docs.claude.com/claude-code

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Related Projects

- [AnyTask CLI](https://github.com/anyt-ai/AnyTaskCLI) - Command-line interface for AnyTask
- [Claude Code](https://code.claude.com) - AI-powered coding assistant
