---
name: agent-bridge
description: Bridge plugin capabilities (commands, skills, agents, hooks, MCP) to specific agent environments (Claude Code, GitHub Copilot, Gemini, Antigravity). Use this skill when converting or installing a plugin to a target runtime.
allowed-tools: Bash, Write, Read
---

# Agent Bridge

## Overview
This skill **adapts and transforms** plugin content into the specific formats required by different AI agent environments. It ensures each runtime can see and use the plugin's capabilities in its native format.

## Prerequisite
The auto-detect mode only targets **existing** directories. Create them first:
```bash
mkdir .agent .github .gemini .claude
```
> If no directories are found, the installer will print this exact error with the mkdir command.

## Usage

### Bridge a Single Plugin
```bash
# Auto-detect all present environments
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin <plugin-path> --target auto

# Force a specific environment (creates directory if needed)
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin <plugin-path> --target github
```

**Example:**
```bash
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-plugin --target auto
```

### Bridge All Plugins
```bash
python plugins/plugin-mapper/scripts/install_all_plugins.py
```

---

## What Gets Bridged

| Plugin Source | Output |
|---|---|
| `commands/*.md` | Converted to target format with `plugin-name_command` prefix |
| `commands/subdir/*.md` | Supported — flattened as `plugin_subdir_command.ext` |
| `skills/` | Copied to `{target}/skills/` |
| `agents/*.md` | Copied to `{target}/skills/{plugin}/agents/` on all targets |
| `rules/` | Copied to `{target}/rules/` |
| `hooks/hooks.json` | → `.claude/hooks/{plugin}-hooks.json` (Claude only; review before activating) |
| `.mcp.json` | MCP servers merged into root `.mcp.json` (de-duplicated) |

---

## Supported Environments

| Target | Flag | Command Directory | Notes |
|---|---|---|---|
| **Claude Code** | `claude` | `.claude/commands/` | Native format; also gets hooks |
| **GitHub Copilot** | `github` | `.github/prompts/` | `.prompt.md` extension |
| **Google Gemini** | `gemini` | `.gemini/commands/` | TOML format; frontmatter stripped, description extracted |
| **Antigravity** | `antigravity` | `.agent/workflows/` | Standard markdown |

### Gemini TOML Format
Command `.md` files are wrapped in TOML. Frontmatter is parsed — the `description` field is extracted and used as the TOML `description`. The frontmatter block is stripped from the prompt body.

```toml
command = "plugin-name:command-name"
description = "Description from frontmatter"
prompt = """
# Command content without frontmatter
...
"""
```

---

## When to Use
- **Installing a new plugin**: Run bridge after dropping a plugin into `plugins/`.
- **Adding a new target environment**: Existing plugins need to be re-bridged after adding `.gemini/` etc.
- **Upgrading a plugin**: Re-run bridge to overwrite with latest command content.
