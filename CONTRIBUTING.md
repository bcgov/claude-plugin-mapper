# Contributing to Claude Plugins & Plugin Manager

Thank you for your interest in contributing! This repository is the home for the **Plugin Manager** distribution and a collection of **Universal Plugins** designed to work across Claude Code, GitHub Copilot, Gemini, and Antigravity.

## 🌟 Our Philosophy: "Standardize Once, Run Everywhere"

We aim to build plugins that follow the [Agent Skills](https://agentskills.io) open standard. By keeping our tool logic in a standardized structure, we can use the **Plugin Manager** to "bridge" those capabilities into whatever AI agent or IDE the developer happens to be using.

---

## 🏗️ Standards for New Plugins

If you are contributing a new plugin to the `plugins/` directory, it **must** follow the standardized architecture:

```
plugins/
└── your-plugin-name/
    ├── .claude-plugin/   # Manifest (standard Claude format)
    ├── README.md         # Documentation for developers
    ├── scripts/          # Standalone python/bash utilities
    ├── skills/           # Actionable AI skills (following Agent Skills standard)
    └── commands/         # Command wrappers for standard shells
```

### Key Rules:
1.  **Relative Paths Only**: Code should never assume a specific user's home directory. Use relative paths or Python's `pathlib` (e.g., `Path(__file__).parents[2]`) to find resources.
2.  **Portability**: Scripts should be compatible with both Linux (bash) and Windows (PowerShell) wherever possible.
3.  **No Placeholders**: Ensure all scripts are functional and documented. Use `generate_image` or mock data if placeholders are needed for demonstration.

---

## 🛠️ Development Workflow

### 1. Set Up Your Environment
We recommend using the official **Plugin Dev** tool (included in `plugins/plugin-dev`) to help scaffold and test your plugins.

### 2. Audit Your Structure
Before submitting a PR, run the automated auditor to ensure your plugin follows the required structure:
```bash
python3 plugins/plugin-mapper/scripts/audit_structure.py
```

### 3. Verify the Bridge
If your plugin includes skills or commands, verify that they can be "bridged" correctly:
```bash
# Test bridging to a specific folder
python3 plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/your-plugin-name --target .github
```

---

## 🚀 Pull Request Process

1.  **Branching**: Create a feature branch for your changes.
2.  **Documentation**: Ensure you have updated the `README.md` within your plugin folder.
3.  **Testing**: Verify your scripts run in a clean environment.
4.  **Submit**: Open a PR against the `main` branch.

---

## 🔒 Security & Standards

1.  **Secrets**: Never commit API keys, tokens, or sensitive environment variables. Use `.env.example` files if configuration is required.
2.  **Licensing**: By contributing, you agree that your code will be licensed under the Apache License, version 2.0 (see `LICENSE`).

---

Copyright 2026 British Columbia — Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
