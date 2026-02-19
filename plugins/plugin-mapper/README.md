# Plugin Manager

**The Universal Bridge for Agent Plugins**

The **Plugin Manager** is a specialized tool designed to **convert and bridge** standard Claude Code Plugins for use in other agent environments. It enables your plugins to work seamlessly in **GitHub Copilot** (`.github`), **Antigravity** (`.agent`), and **Google Gemini** (`.gemini`).

## 🚀 Key Features

*   **Write Once, Run Everywhere**: Create plugins in the standard `.claude-plugin` format.
*   **Automatic Bridging**: 
    *   **GitHub Copilot**: Converts commands to `.prompt.md` files in `.github/prompts/`.
    *   **Google Gemini**: Wraps commands in TOML for `.gemini/commands`.
    *   **Antigravity**: Adapts workflows for `.agent/workflows`.
*   **Zero-Config**: Auto-detects your environment and installs the necessary adapters.

---

## 📦 Installation & Usage

### 1. Project Setup
Ensure your repository has the following structure:

```text
my-repo/
├── .github/          # (Optional) For Copilot prompts
├── .gemini/          # (Optional) For Gemini commands
├── .agent/           # (Optional) For Antigravity workflows
└── plugins/          # Your plugin directory
    ├── plugin-mapper/  <-- Drop THIS folder here
    └── my-tool/         <-- Drop your other plugins here
        ├── plugin.json
        └── ...
```

### 2. Install Plugins
Just drop any standard Claude Code plugin folder into the `plugins/` directory.

### 3. Run the Bridge
Run the installer script to automatically bridge all plugins in your `plugins/` folder to your active agent environments.

```bash
python3 plugins/plugin-mapper/scripts/install_all_plugins.py
```

That's it! Your plugins are now available in your configured agents.

---

## 🛠️ Advanced Usage

### Single Plugin Installation
If you only want to install a specific plugin:

```bash
python3 plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-target-plugin --target auto
```

### Targeted Installation
Force installation to a specific environment (e.g., only GitHub Copilot), even if the directory doesn't exist (it will be created):

```bash
python3 plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-tool --target github
```

---

## 🧩 Compatibility

The bridge supports any plugin that follows the [Claude Code Plugin Manifest](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/plugins) specification. It parses the `plugin.json` (or `.claude-plugin/manifest.json`) and generates the appropriate adapters.
