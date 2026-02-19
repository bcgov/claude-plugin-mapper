---
name: plugin-replicator
description: Replicate, clone, or update plugins from the `central-repo` to other project repositories. Use this when setting up a new project or pulling the latest plugin source code.
allowed-tools: Bash, Write, Read
---

# Plugin Replicator

## Overview
This skill manages the **synchronization of plugin source code** between your central repository and other projects. It ensures your tools are consistent across all your workspaces.

## Usage

### 1. Replicate a Single Plugin
Install or update a specific plugin in another project.

```bash
# Copy mode (Default - for stable deployment)
python3 plugins/plugin-mapper/scripts/plugin_replicator.py --plugin <plugin-name> --target <project-root>

# Link mode (Dev - changes reflect instantly)
python3 plugins/plugin-mapper/scripts/plugin_replicator.py --plugin <plugin-name> --target <project-root> --link
```

**Example:**
```bash
python3 plugins/plugin-mapper/scripts/plugin_replicator.py --plugin guardian-onboarding --target ../project-sanctuary --link
```

### 2. Bulk Replication
Replicate ALL plugins (or a subset) to a target project.

```bash
# Sync all plugins
python3 plugins/plugin-mapper/scripts/bulk_replicator.py --target <project-root>

# Sync only specific plugins (e.g., investment tools)
python3 plugins/plugin-mapper/scripts/bulk_replicator.py --target <project-root> --filter "investment-*"
```

## When to use
*   **New Project Setup**: Populate `.agent/plugins/` with your standard toolkit.
*   **Update**: Pull the latest fixes from `my-plugins` into `project-sanctuary`.
*   **Development**: Link plugins to work on them centrally while testing in a specific project context.
