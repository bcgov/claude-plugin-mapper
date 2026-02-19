---
name: agent-bridge
description: Bridge plugin capabilities (commands, skills) to specific agent environments (GitHub Copilot, Gemini, Antigravity). Use this to adapting plugins to the specific formats required by these runtimes.
allowed-tools: Bash, Write, Read
---

# Agent Bridge

## Overview
This skill **adapters and transforms** plugin content into the specific formats required by different AI agents. While `plugin-replicator` moves the source code, `agent-bridge` ensures the runtime environment (Copilot, Gemini, etc.) can actually *see* and *use* the tools.

## Usage

### Deploy to Runtime Environment
Run the bridge installer to inject plugin capabilities into the detected agent configuration folders.

> **CRITICAL**: The installer only targets existing folders. You **MUST** create these directories in your project root first:
> ```bash
> mkdir .github .gemini .agent .claude
> ```

```bash
# Auto-detect environment and install (after creating directories)
python3 plugins/plugin-mapper/scripts/bridge_installer.py --plugin <plugin-path> --target auto


# Force install for GitHub Copilot (will create directories if missing)
python3 plugins/plugin-mapper/scripts/bridge_installer.py --plugin <plugin-path> --target github
```

**Example:**
```bash
python3 plugins/plugin-mapper/scripts/bridge_installer.py --plugin plugins/guardian-onboarding --target github
```

## Supported Environments
*   **Antigravity**: transforms commands for `.agent/workflows`.
*   **GitHub Copilot**: Converts commands to `.prompt.md` files in `.github/prompts`.
*   **Gemini**: Wraps commands in TOML format for `.gemini/commands`.
*   **Claude Code**: Native support (usually handled by `plugin-replicator`, but bridge can adapt older formats).

## When to use
*   **Copilot Integration**: When you want your Claude plugins to be available in VS Code Chat.
*   **Gemini Context**: When working in Google IDX or Geminified environments.
*   **Legacy Support**: When using older agent architectures that require scattered file placement.
