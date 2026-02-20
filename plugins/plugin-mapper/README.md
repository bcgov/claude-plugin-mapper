# Plugin Mapper

**The Universal Bridge for Agent Plugins**

The **Plugin Mapper** converts and bridges standard Claude Code plugins for use in any agent environment — write your plugin once and deploy it everywhere.

## 🌐 Supported Targets

| Target | Directory | What Gets Created |
|---|---|---|
| **Claude Code** | `.claude/` | `commands/*.md`, `skills/`, `hooks/` |
| **GitHub Copilot** | `.github/` | `prompts/*.prompt.md`, `skills/` |
| **Google Gemini** | `.gemini/` | `commands/*.toml`, `skills/` |
| **Antigravity** | `.agent/` | `workflows/`, `skills/` |

---

## 🚀 Quick Start

### 1. Create Target Directories
Create whichever agent environment directories you use (the bridge auto-detects them):
```bash
mkdir .agent .github .gemini .claude
```

> ⚠️ The `--target auto` mode requires at least one of these directories to exist. If none are found, the installer will print the exact `mkdir` command to run.

### 2. Drop Your Plugin In
Place any standard Claude Code plugin in `plugins/`:
```
my-repo/
├── .github/          ← GitHub Copilot target
├── .gemini/          ← Gemini target
├── .agent/           ← Antigravity target
├── .claude/          ← Claude Code target
└── plugins/
    ├── plugin-mapper/   ← This tool
    └── my-plugin/       ← Your plugin (standard .claude-plugin format)
```

### 3. Run the Bridge

**Single plugin:**
```bash
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-plugin --target auto
```

**All plugins at once:**
```bash
python plugins/plugin-mapper/scripts/install_all_plugins.py
```

---

## 🛠️ Advanced Usage

### Force a Specific Target
Install to a specific environment only (creates directory if needed):
```bash
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-plugin --target github
```

### What Gets Bridged

The installer maps every component of a plugin:

| Plugin Component | Output |
|---|---|
| `commands/*.md` | Per-target command format (`.md`, `.prompt.md`, `.toml`) |
| `commands/subdir/*.md` | Flattened as `plugin_subdir_command.ext` |
| `skills/` | Copied directly into `{target}/skills/` |
| `agents/*.md` | Copied into `{target}/skills/{plugin}/agents/` |
| `rules/` | Copied into `{target}/rules/` |
| `hooks/hooks.json` | Copied to `.claude/hooks/{plugin}-hooks.json` (Claude only) |
| `.mcp.json` | MCP servers merged into root `.mcp.json` |

---

## 🧩 Plugin Format

The bridge supports any plugin following the standard `.claude-plugin` manifest structure:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json      ← Plugin metadata (name, version, etc.)
├── commands/            ← Slash commands (supports subdirectories)
├── skills/              ← Persistent knowledge/behavior files
├── agents/              ← Sub-agent persona definitions
├── rules/               ← Behavioral rules
├── hooks/
│   └── hooks.json       ← Claude Code lifecycle hooks
└── .mcp.json            ← MCP server declarations
```

All components are optional — the bridge gracefully skips missing directories.
