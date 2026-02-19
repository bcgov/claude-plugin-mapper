# Repository Identity: Plugin Manager Distribution

You are currently working in the standalone distribution repository for the **Plugin Manager**. 
This repository is designed to be shared with the community or other teams so they can adopt the "Universal Plugin" workflow.

## 📦 What is this Repo?
This is the home of the **Plugin Manager** tool. It contains the source code for a meta-plugin that helps users:
1.  **Bridge** Claude Code plugins to work in GitHub Copilot, Google Gemini, and Antigravity (`agent-bridge`).
2.  **Replicate** plugins from a central source to local projects (`plugin-replicator`).
3.  **Audit** their plugin structure for compliance (`plugin-maintenance`).

## 📂 Structure
The repository follows the standard Claude Plugin architecture:

```
repo-root/
└── plugins/
    └── plugin-mapper/       <-- The core product
        ├── .claude-plugin/   # Manifest
        ├── README.md         # User documentation
        ├── scripts/          # Shared utilities (audit, install)
        └── skills/           # Distinct capabilities
            ├── agent-bridge/      # Runtime adapters
            ├── plugin-replicator/ # Sync logic
            └── plugin-maintenance/# Health checks
```

## 🤖 Your Role Here
As the agent managing this repository, your goals are:
- **Maintain Portability**: Ensure code and docs rely on relative paths (e.g., `parents[3]`) and generic terms (`central-repo`), not hardcoded user paths.
- **Enforce Standards**: Use the included `plugins/plugin-mapper/scripts/audit_structure.py` to self-validate the repo.
- **Support Users**: The `README.md` in `plugins/plugin-mapper/` is the primary entry point for users. Ensure it clearly explains how to "drop this folder into their `plugins/` directory".

## 🚀 Key Scripts
- **Bridge Installer**: `plugins/plugin-mapper/scripts/bridge_installer.py`
  - *Purpose*: Deploys plugin capabilities to `.github/`, `.gemini/`, etc.
- **Structure Audit**: `plugins/plugin-mapper/scripts/audit_structure.py`
  - *Purpose*: Checks that this plugin (and others) are well-formed.

## 🔌 Universal Bridge: Making Any Plugin Work Anywhere

The core superpower of this repo is the **Agent Bridge**. It allows you to take *any* standard Claude Code plugin (from the community or your own creation) and instantly make it available to:
- **GitHub Copilot**: As a slash command in VS Code.
- **Google Gemini**: As a command in Google IDX.
- **Antigravity**: As a workflow or skill.

### 1. Drop it in
Place your plugin folder in `plugins/`:
```
my-repo/
└── plugins/
    ├── plugin-mapper/     <-- This tool
    └── my-awesome-plugin/  <-- Any standard Claude plugin
        ├── plugin.json
        └── ...
```

### 2. Bridge it
Run the bridge installer to generate the necessary adapters for your active environments:

```bash
# Auto-detects .github, .gemini, etc. and installs adapters
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-awesome-plugin --target auto
```

Now `my-awesome-plugin` is available in Copilot (`/my-awesome-plugin`) and Gemini!

### 3. Deep Awareness (Agent Skills)
If you (the agent) need to understand *how* to convert plugins or need to perform this programmatically, refer to the **Agent Bridge Skill**:
- **Source**: `plugins/plugin-mapper/skills/agent-bridge/SKILL.md`
- **Deployed**: `.github/skills/agent-bridge/SKILL.md` (or `.agent/...`)

This skill definition contains the specific logic for mapping Claude commands to `.prompt.md`, `.toml`, and Antigravity workflows. Use it to "install" capabilities into your own runtime.

## ⚠️ Critical: Path Translation in Scripts

Scripts in `plugins/plugin-mapper/scripts/` use **relative parent traversal** to find the project root.
They assume this directory depth:

```
repo-root/                        # ← PROJECT_ROOT (target)
└── plugins/                      # parent 3
    └── plugin-mapper/           # parent 2
        └── scripts/              # parent 1 (SCRIPT_DIR)
            └── script.py         # __file__
```

So `SCRIPT_DIR.parent.parent.parent` (3 levels up) = `repo-root`.

### Path Calculations per Script

| Script | Code | Depth | Status |
|---|---|---|---|
| `audit_structure.py` | `SCRIPT_DIR.parent.parent.parent` | 3 | ✅ Correct |
| `plugin_replicator.py` | `current_script.parents[3]` | 3 | ✅ Correct |
| `bulk_replicator.py` | `current_script.parents[3]` | 3 | ✅ Correct |
| `install_all_plugins.py` | `SCRIPT_DIR.parent.parent.parent` | 3 | ✅ Correct |
| `generate_readmes.py` | `SCRIPT_DIR.parent.parent.parent` | 3 | ✅ Correct |
| `bridge_installer.py` | *(no root calc — uses `--plugin` arg)* | N/A | ✅ Correct |

### When Path Breaks
If `plugin-mapper` is installed at a **different depth** (e.g., user puts it at repo root instead of `plugins/plugin-mapper/`), ALL scripts using `parents[3]` will resolve to the wrong directory.

**Rule:** These scripts assume they live at `<repo>/plugins/plugin-mapper/scripts/`. The README instructs users to install at this exact path. Do not move scripts without updating the parent traversal depth.

## 📝 Context for Commits
When making changes, remember that this code will be cloned into other users' `plugins/` folders. Avoid assumptions about the host environment's name.
