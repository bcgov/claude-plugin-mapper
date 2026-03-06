# Plugin Mapper (DEPRECATED)

> [!WARNING]
> **REPOSITORY DEPRECATED**
> 
> This repository and the local bridge installer scripts are no longer the recommended way to install agent skills. We recommend you use `npx add` instead.
> 
> The global ecosystem has moved to the open standard for direct GitHub-to-agent installation. 
> All active development, skills, plugins, and agents now live in the unified monorepo: **[richfrem/agent-plugins-skills](https://github.com/richfrem/agent-plugins-skills)**.

## How to Install Skills Now

You no longer need to download python mapper scripts. We strongly recommend using `npx skills add` to install all skills natively into your agent environments (Claude, Copilot, Gemini, Cursor, etc.) directly from the terminal. 

`npx skills` can be used with ANY GitHub repository. 

Here are a few examples:

```bash
# Install all plugins from this repo
npx skills add richfrem/agent-plugins-skills

# Install a single plugin
npx skills add richfrem/agent-plugins-skills/plugins/rlm-factory
npx skills add richfrem/agent-plugins-skills/plugins/vector-db
npx skills add richfrem/agent-plugins-skills/plugins/spec-kitty-plugin

# Update all installed skills to latest
npx skills update
```

### Broader Marketplaces and Ecosystems
You can also explore and add skills from the broader agent skills marketplace, including:
- **[Skills Marketplace (skillsmp.com)](https://skillsmp.com/)**: Discover and install a wide variety of skills for your agent environment.
- **[microsoft/skills](https://github.com/microsoft/skills)**: A comprehensive collection of over 125 skills installable via CLI (`npx skills add microsoft/skills`).
- **Claude Plugins**: Find various plugins optimized for Claude.

For historical reference, the bridge installer scripts that used to live here have been moved to the new unified repository under:
`plugins/plugin-mapper/skills/agent-bridge/scripts/bridge_installer.py`

They are retained there solely for contributors who need custom target manipulation.

---

*Copyright 2026 British Columbia — Licensed under the Apache License, Version 2.0*
