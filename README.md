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

## Quick Start

### Prerequisites

1. **Install AnyTask CLI**: The plugin requires the `anyt` CLI to be installed and configured.

   ```bash
   # Using uvx (recommended)
   uvx anyt --help

   # Or using pipx
   pipx install anyt
   ```

2. **Configure AnyTask**: Set up your environment and authenticate.

   ```bash
   # Add environment (if using local dev server)
   anyt env add dev http://localhost:8000
   anyt env use dev

   # Authenticate
   anyt auth login

   # Select workspace
   anyt workspace list
   anyt workspace select DEV
   ```

### Installation

#### Option 1: Install from GitHub (Recommended)

```bash
# In Claude Code terminal
/plugin install https://github.com/anyt-ai/anyt-claude
```

#### Option 2: Install from Local Directory (Development)

```bash
# In Claude Code terminal, from this repository
/plugin install .
```

#### Option 3: Add to Marketplace

1. Add the marketplace to Claude Code:

```bash
/plugin marketplace add anyt-ai/anyt-claude
```

2. Install the plugin:

```bash
/plugin install anytask-claude@anytask-plugins
```

## Usage

### Slash Commands

#### `/anyt-create` - Create New Task

Create a new task in AnyTask with full control over all task properties.

```
User: /anyt-create
Claude: I'll help you create a new task. What would you like the task title to be?
```

#### `/anyt-next` - Get Next Task Recommendation

Get intelligent AI-powered suggestions for your next task and start working on it.

```
User: /anyt-next
Claude: Let me get task suggestions for you...

Top Task Recommendations:
1. DEV-42: Implement OAuth callback
   Priority: 2 (Urgent) | Status: todo
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
│   ├── anyt-create.md      # Create task command
│   └── anyt-next.md        # Next task command
├── skills/                  # Skills (model-invoked)
│   ├── task-creator.md     # Automatic task creation
│   ├── task-workflow.md    # Workflow management
│   └── task-context.md     # Context loading
└── README.md               # Full documentation
```

### Testing the Plugin Locally

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
   /anyt-create
   /anyt-next
   ```

4. After making changes, reinstall:
   ```
   /plugin uninstall anytask-claude
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
