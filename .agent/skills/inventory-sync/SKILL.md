---
name: inventory-sync
description: Safely synchronize plugins from the vendor inventory to the local project, ensuring project-specific customizations are preserved while keeping vendor plugins up-to-date.
---

# Inventory Sync Skill

This skill guides you through the process of synchronizing your local plugin collection with the vendor's master inventory. It leverages the "Safe Sync" logic to ensure that while vendor plugins are updated and deleted ones are cleaned up, your **project-specific** plugins and artifacts are **NEVER** touched.

## Key Resources
- **Script**: `plugins/plugin-mapper/scripts/sync_with_inventory.py`
- **Inventory Generator**: `plugins/plugin-mapper/scripts/plugin_inventory.py`
- **Logic Guide**: `plugins/plugin-mapper/resources/cleanup_process.md`
- **Visual Flow**: `plugins/plugin-mapper/resources/cleanup_flow.mmd`

## The Core Rule
**"Only delete things that originated in .vendor. NEVER delete project-local unique plugins, skills, or workflows."**

## Usage

### 1. The Variance Check (Automatic)
The synchronization script performs a variance analysis between:
1.  **Vendor Inventory** (`.vendor/.../vendor-plugins-inventory.json`): The "Menu" of available upstream plugins.
2.  **Local Inventory**: A generated list of what is currently in your `plugins/` directory.

### 2. How to Run Sync
To synchronize your plugins (Update Active, Protect Local, Cleanup Deleted):

```bash
python plugins/plugin-mapper/scripts/sync_with_inventory.py
```

**Options:**
- `--dry-run`: View what *would* happen without making changes. Highly recommended for first-time runs.
- `--root .`: Specify the project root (default is current directory).

### 3. Verification Steps
After running the sync, you can verify the state:
1.  Check `local-plugins-inventory.json` (generated in the root) to see your current installed state.
2.  Ensure that custom plugins (those not in the vendor list) are still present in `plugins/`.
3.  Verify that artifacts (in `.agent`, `.gemini`, etc.) for *removed* vendor plugins are gone.

## Troubleshooting

### "It says 'Vendor directory not found'"
- Ensure you have the vendor repository cloned into `.vendor/plugin-collection` (or the configured vendor path).
- Without the vendor inventory, the script defaults to **Safety Mode** and will NOT perform any cleanup, preventing accidental deletion.

### "I want to delete a vendor plugin"
1.  Delete the plugin folder from `plugins/`.
2.  Run `sync_with_inventory.py`.
3.  The script will detect it is missing locally but present in vendor (Variant C) and clean up its artifacts.

### "I want to delete a LOCAL plugin"
1.  Delete the plugin folder from `plugins/`.
2.  You must manually remove its artifacts (skills, rules, etc.) because the script protects non-vendor items.
