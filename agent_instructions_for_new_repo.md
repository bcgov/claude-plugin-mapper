# Repository Identity: Plugin Mapper Distribution

You are currently working in the standalone distribution repository for the **Plugin Mapper**.
This repository is designed to be shared with the community or other teams so they can adopt the "Universal Plugin" workflow.

## 📦 What is this Repo?
This is the home of the **Plugin Mapper** tool — a meta-plugin that converts and bridges standard Claude Code plugins to work in any agent environment.

## 📂 Structure
```
repo-root/
└── plugins/
    └── plugin-mapper/       <- The core product
        ├── .claude-plugin/   # Manifest
        ├── README.md         # User documentation
        ├── scripts/          # Bridge installer and utilities
        └── skills/
            └── agent-bridge/ # The bridge skill (SKILL.md)
```

## 🤖 Your Role Here
- **Maintain Portability**: Use relative paths. Don't hardcode user-specific paths.
- **Support Users**: `plugins/plugin-mapper/README.md` is the primary entry point. Keep it accurate.
- **Keep SKILL.md current**: `plugins/plugin-mapper/skills/agent-bridge/SKILL.md` is what agents read to understand how to bridge plugins. It must reflect actual bridge_installer.py behavior.

## 🚀 Key Script
- **`bridge_installer.py`**: `plugins/plugin-mapper/scripts/bridge_installer.py`
  - Takes `--plugin <path>` and `--target <auto|antigravity|github|gemini|claude>`
  - Auto-detects environments by checking for `.agent`, `.github`, `.gemini`, `.claude` directories
  - If none found, prints a helpful error with the `mkdir` command to run

## 🌐 Bridge Targets

| Target | Directory | Formats |
|---|---|---|
| Claude Code | `.claude/` | `commands/*.md`, `skills/`, `hooks/` |
| GitHub Copilot | `.github/` | `prompts/*.prompt.md`, `skills/` |
| Google Gemini | `.gemini/` | `commands/*.toml` (TOML-wrapped), `skills/` |
| Antigravity | `.agent/` | `workflows/`, `skills/` |

## 🔌 What Gets Bridged (bridge_installer.py behavior)

| Plugin Source | What Happens |
|---|---|
| `commands/*.md` | Converted to per-target format with `plugin-name_command` prefix |
| `commands/subdir/*.md` | Supported — flattened as `plugin_subdir_command.ext` |
| `skills/` | Copied directly to `{target}/skills/` |
| `agents/*.md` | Copied to `{target}/skills/{plugin}/agents/` on all targets |
| `rules/` | Copied to `{target}/rules/` |
| `hooks/hooks.json` | Copied to `.claude/hooks/{plugin}-hooks.json` (Claude only) |
| `.mcp.json` | MCP servers merged into root `.mcp.json` (de-duplicated) |

**Gemini TOML**: Frontmatter (`---description:...---`) is stripped from the prompt body. The `description` field is extracted and used as the TOML `description` value.

## 🔄 Self-Bridging This Repo

This repo includes the bridge skill at `.agent/skills/agent-bridge/scripts/`. You can bridge the `plugin-mapper` plugin into all four environments directly:

```bash
# Bridge to Antigravity (.agent/)
python3 .agent/skills/agent-bridge/scripts/bridge_installer.py --plugin plugins/plugin-mapper --target antigravity

# Bridge to Claude Code (.claude/)
python3 .agent/skills/agent-bridge/scripts/bridge_installer.py --plugin plugins/plugin-mapper --target claude

# Bridge to Gemini (.gemini/)
python3 .agent/skills/agent-bridge/scripts/bridge_installer.py --plugin plugins/plugin-mapper --target gemini

# Bridge to GitHub Copilot (.github/)
python3 .agent/skills/agent-bridge/scripts/bridge_installer.py --plugin plugins/plugin-mapper --target github
```

> **Note**: Run these from the repo root. `bridge_installer.py` uses `Path.cwd()` + `--plugin` arg so it resolves correctly regardless of where the script lives.

> **⚠️ `install_all_plugins.py` does NOT work in this repo.** When the script lives at `.agent/skills/agent-bridge/scripts/`, it goes 5 levels up to resolve `PROJECT_ROOT` — landing at `/Users/<you>/Projects/` instead of the repo root. Use `bridge_installer.py` directly (as above) when working in this repo.

## ⚠️ Critical: Path Translation in Scripts

Scripts assume this directory depth **when installed into a user's project** (i.e. at `plugins/plugin-mapper/skills/agent-bridge/scripts/`):
```
repo-root/                        ← PROJECT_ROOT
└── plugins/
    └── plugin-mapper/
        └── skills/
            └── agent-bridge/
                └── scripts/
                    └── script.py ← __file__
```

`SCRIPT_DIR.parent.parent.parent.parent.parent` (5 levels up) = `repo-root`.

| Script | Root Calc | Status (in user project) |
|---|---|---|
| `install_all_plugins.py` | `SCRIPT_DIR.parent.parent.parent.parent.parent` | ✅ Correct |
| `bridge_installer.py` | Uses `Path.cwd()` + `--plugin` arg | ✅ Correct |

## 📝 Context for Commits
This code will be cloned into other users' `plugins/` folders. Avoid assumptions about the host environment name.
