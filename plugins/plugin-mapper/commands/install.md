---
description: Install and bridge a plugin into agent environments.
args:
  plugin_name:
    description: "The name of the plugin to install/bridge (e.g., agency-swarm)."
    type: string
    required: true
---

# Install/Bridge Plugin

This command installs a plugin into your project (if not present) and bridges its capabilities to all detected agent environments (.agent, .github, .gemini, .claude). It prioritizes local plugins first.

```bash
PLUGIN_NAME="${plugin_name}"
LOCAL_PATH="plugins/$PLUGIN_NAME"
VENDOR_PATH=".vendor/plugin-collection/plugins/$PLUGIN_NAME"

# 1. Check Local (Priority: User Created/Modified)
if [ -d "$LOCAL_PATH" ]; then
    echo "Processing local plugin: '$PLUGIN_NAME'..."

# 2. Check Vendor (Fallback: Downloadable)
elif [ -d "$VENDOR_PATH" ]; then
    echo "Plugin '$PLUGIN_NAME' found in vendor. Installing..."
    mkdir -p plugins
    cp -r "$VENDOR_PATH" "plugins/"
else
    echo "Error: Plugin '$PLUGIN_NAME' not found locally or in vendor collection."
    exit 1
fi

# 3. Bridge/Convert (The Core Action)
echo "Bridging '$PLUGIN_NAME' to agent environments..."
python plugins/plugin-mapper/scripts/bridge_installer.py --plugin "plugins/$PLUGIN_NAME" --target auto
```
