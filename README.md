# Plugin Mapper

This repository hosts the source code for the **Plugin Mapper**, a meta-plugin that bridges standard Claude Code plugins to any agent environment.

## 📦 What's Inside?

The core of this repository is located in `plugins/plugin-mapper/`. This tool allows you to:

1. **Bridge Capabilities**: Make your Claude plugins work seamlessly in **GitHub Copilot**, **Google Gemini**, **Antigravity**, and **Claude Code**.
2. **Write Once, Run Everywhere**: Plugins follow the standard `.claude-plugin` format and are automatically converted per-environment.

## 🚀 Getting Started

### 1. Install Plugin Mapper
Copy `plugins/plugin-mapper` into your project's `plugins/` directory.

### 2. Create Target Directories
The bridge installer auto-detects existing environment folders. Create whichever you need:
```bash
mkdir .agent .github .gemini .claude
```

### 3. Run the Bridge
Bridge a single plugin:
```bash
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/my-plugin --target auto
```

Or bridge all plugins at once:
```bash
python plugins/plugin-mapper/scripts/install_all_plugins.py
```

For full documentation see the [Plugin Mapper README](plugins/plugin-mapper/README.md).

## 📂 Repository Structure

```
repo-root/
└── plugins/
    └── plugin-mapper/       <- The core product
        ├── scripts/          # Bridge installer and utilities
        ├── skills/           # Agent bridge skill definitions
        └── README.md         # Detailed documentation
```

---

## License

Copyright 2026 British Columbia — Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
