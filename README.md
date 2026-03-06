# Plugin Mapper (DEPRECATED)

> [!WARNING]
> **REPOSITORY DEPRECATED**
> 
> This repository and the local bridge installer scripts are no longer the recommended way to install agent skills. We recommend you use `npx add` instead.
> 
> The global ecosystem has moved to the open standard for direct GitHub-to-agent installation. 
> All active development, skills, plugins, and agents now live in the unified monorepo: **[richfrem/agent-plugins-skills](https://github.com/richfrem/agent-plugins-skills)**.

## How to Install Skills Now

You no longer need to download python mapper scripts. We strongly recommend using `npx add` to install all skills natively into your agent environments (Claude, Copilot, Gemini, Cursor, etc.) directly from the terminal:

```bash
# Install ALL skills from the new monorepo:
npx add richfrem/agent-plugins-skills

# Install a specific skill (e.g. adr-management):
npx add richfrem/agent-plugins-skills/plugins/adr-management
```

## Legacy Source

For historical reference, the bridge installer scripts that used to live here have been moved to the new unified repository under:
`plugins/plugin-mapper/skills/agent-bridge/scripts/bridge_installer.py`

They are retained there solely for contributors who need custom target manipulation.

---

*Copyright 2026 British Columbia — Licensed under the Apache License, Version 2.0*
