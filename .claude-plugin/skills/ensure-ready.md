---
name: _ensure_ready
description: Internal pre-check logic for all AnyTask commands. Ensures anyt CLI is available, ANYT_API_KEY is set, and optionally checks for .anyt/anyt.json configuration.
---

# AnyTask Environment Pre-Check

This is an internal skill used by all AnyTask commands to ensure the environment is properly configured before executing any operations.

## Purpose

Verify that:
1. The `anyt` CLI is installed and available
2. The `ANYT_API_KEY` environment variable is set
3. (Optional) The current directory has been initialized with `.anyt/anyt.json`

## When to Use

All `/anytask:*` commands should call this skill at the beginning of their execution to ensure a working environment.

## Pre-Check Steps

### Step 1: Check if `anyt` CLI is available

Run:
```bash
anyt --version
```

**If command fails:**
- DO NOT attempt to install anything automatically
- Display this message to the user:

```
It looks like the AnyTask CLI is not installed in your environment.

To install:
  pip install anyt

Or visit: https://anyt.dev/docs/install

Once installed, please try again.
```

- STOP execution and return from the skill

**If successful:**
- Continue to next step

### Step 2: Check if ANYT_API_KEY is set

Run:
```bash
env | grep ANYT_API_KEY
```

**If not found:**
- Display this message to the user:

```
You haven't set your ANYT_API_KEY yet.

To get your API key:
  1. Visit: https://anyt.dev/home/settings/api-keys
  2. Create a new API Key (type: CLI/Agent)
  3. Set it in your environment:
     export ANYT_API_KEY=anyt_agent_...

Then try again.
```

- STOP execution and return from the skill

**If found:**
- Continue to next step

### Step 3: Check if .anyt/anyt.json exists (OPTIONAL)

This check is optional and depends on the command's requirements. Some commands like `/anytask:init` don't require this.

Run:
```bash
test -f .anyt/anyt.json && echo "exists" || echo "not found"
```

**If not found:**
- This is expected for `/anytask:init` - proceed with initialization
- For other commands that require initialization, display:

```
The current directory hasn't been initialized with AnyTask yet.

Please run: /anytask:init

This will connect your repository to an AnyTask workspace and project.
```

- STOP execution and return from the skill

**If exists:**
- The environment is fully configured
- Continue with the command's main logic

## Return Values

This skill should communicate back to the calling command:
- ✅ **Ready**: All checks passed, proceed with command
- ❌ **Not Ready**: Show appropriate error message, stop execution

## Example Usage in Commands

In each command's markdown file, include:

```
Before executing any AnyTask operations, use the _ensure_ready skill to verify the environment.

If the pre-check fails, stop execution and display the error message to the user.
Only proceed if all checks pass.
```

## Important Notes

- **Never auto-install**: Do not install the CLI or set environment variables automatically
- **Clear guidance**: Provide step-by-step instructions for fixing issues
- **Stop on failure**: Do not proceed with the command if pre-checks fail
- **Be helpful**: Include relevant links and documentation
- **Respect user choice**: Let the user configure their environment manually
