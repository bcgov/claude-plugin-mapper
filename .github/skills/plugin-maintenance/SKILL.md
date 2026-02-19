---
name: plugin-maintenance
description: Audit and maintain the health of your plugin ecosystem. Use this skill to verify directory structure compliance, generate documentation, and migrate legacy plugins.
allowed-tools: Bash, Write, Read
---

# Plugin Maintenance

## Overview
This skill provides tools for **keeping your plugins clean and compliant** with the official `plugin-structure` standards. It helps you catch drift, legacy artifacts, and structural issues before they cause problems.

## Usage

### 1. Audit Structure
Check all plugins in the repository against the `plugin-dev` structural guidelines.

```bash
python3 plugins/plugin-mapper/scripts/audit_structure.py
```

**Checks Performed:**
*   Presence of `skills/` directory.
*   Presence of `SKILL.md` in every skill folder.
*   Absence of deprecated top-level `scripts/` (for standard plugins).
*   Compliance with file naming conventions.

### 2. Sync Plugins (Inventory-Based)
Synchronize the local `plugins/` directory with the upstream `.vendor` collection, ensuring safe cleanup of deleted plugins.

```bash
python3 scripts/sync_with_inventory.py [--dry-run]
```

**Process:**
1.  Generates `vendor_inventory.json` (Source) and `local_inventory.json` (Local).
2.  **Identifies Removed Plugins**: Plugins present in Vendor but missing from Local.
3.  **Clean**: Removes artifacts of *removed* plugins from `.agent`, `.github`, `.gemini`, `.claude`.
4.  **Install**: Re-installs/Updates all plugins present in Local.

> **Note**: Project-specific plugins (not in Vendor) are preserved.

### 3. Coming Soon
*   `migrate_structure.py`: Helper to move legacy folders.
*   `generate_readmes.py`: Helper to document skills.

## When to use
*   **After adding a new plugin**: run audit to ensure you set it up correctly.
*   **Periodically**: to catch drift or accidental file placements.
*   **Before release**: to ensure clean distribution.
