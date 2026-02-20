# Plugin Mapper — Testing Issue Log

**Branch:** `feature/plugin-bridge-testing`  
**Test plugins:** `test-plugins/examples/01-beginner` → `04-real-world` (13 plugins total)

---

## Issue Table

| ID | Severity | Status | Plugin(s) Affected | Description |
|---|---|---|---|---|
| 001 | 🟡 Medium | **Fixed (existing)** | All | `--target auto` exits with no output if `.agent/.github/.gemini/.claude` dirs don't exist. Must create manually first. |
| 002 | 🟢 Low | **Working** | All | Namespace collisions prevented by `plugin-name_command.md` prefix on all targets. |
| 003 | 🔴 High | **✅ Fixed** | `code-snippets-plugin` | **Nested command subdirs ignored.** `commands/refactor/` and `commands/snippets/` are silently skipped. Only top-level `*.md` files are installed. 5 of 6 commands are missed. Fix: changed `glob("*.md")` → `rglob("*.md")` with flat `plugin_subdir_file` naming. |
| 004 | 🟡 Medium | **✅ Fixed** | `devops-plugin`, `api-testing-plugin`, `web-scraping-plugin` | **`agents/` directory not bridged.** Plugins with specialized sub-agents (e.g. `agents/incident-responder.md`, `agents/api-test-generator.md`) are completely ignored by the bridge. Fix: agents copied into `{target-skills}/{plugin-name}/agents/`. |
| 005 | 🟢 Low | **✅ Fixed** | `devops-plugin`, `productivity-plugin`, `testing-plugin`, `security-audit-plugin` | **`hooks/` directory not bridged.** `hooks/hooks.json` was not copied. Fix: `install_hooks()` copies to `.claude/hooks/{plugin-name}-hooks.json` with a review-before-activating note. Claude-specific; no-op on other targets. |
| 006 | 🟡 Medium | **✅ Fixed** | All (Gemini) | **Gemini TOML embeds raw YAML frontmatter.** Fix: `parse_frontmatter()` now extracts description and strips frontmatter from prompt body. |
| 007 | 🟢 Low | **✅ Fixed** | `devops-plugin`, `cloud-storage-plugin`, `knowledge-base-plugin`, `slack-integration-plugin`, `web-scraping-plugin` | **`.mcp.json` not translated.** Fix: `merge_mcp_config()` reads plugin `.mcp.json` and merges `mcpServers` into root `.mcp.json`, skipping duplicate server names. Creates root file if absent. |
| 008 | 🟢 Low | **✅ Working** | All | Skills are correctly copied to all 4 environments. |
| 009 | 🟢 Low | **✅ Working** | All | Rules (if present) are correctly copied to all 4 environments. |
| 010 | 🟢 Low | **✅ Working** | All | Naming convention (`plugin-name_command-name.md`) consistent across all environments. |
| 011 | 🟡 Medium | **✅ Fixed** | All | `--target auto` error message now instructs user to create dirs (`mkdir .agent .github .gemini .claude`). |

---

## Detailed Notes

### Issue 003 — Nested Commands
The `code-snippets-plugin` structures commands in subdirectories:
```
commands/
  explain.md          ✅ Installed
  refactor/
    extract.md        ❌ Missed
    simplify.md       ❌ Missed
  snippets/
    class.md          ❌ Missed
    function.md       ❌ Missed
    test.md           ❌ Missed
```
**Fix required:** Change `commands_dir.glob("*.md")` → `commands_dir.rglob("*.md")` and include the subdir name in the output filename, e.g. `plugin-name_refactor_extract.md`.

### Issue 004 — Agents Directory
Advanced plugins (devops, api-testing, web-scraping) include `agents/` with sub-agent persona definitions. These are `.md` files with frontmatter (`tools`, `proactive`, etc.). They represent specialized AI personas.
**Fix required:** Map `agents/*.md` → treat as additional skills or copy to a dedicated `agents/` subfolder within the target skill dir.

### Issue 006 — Gemini TOML Description 
Current output:
```toml
command = "code-snippets-plugin:explain"
description = "Imported from plugin"
prompt = """
---
description: Explain code in a file with clear breakdowns
---
# Explain Code Command
...
"""
```
Expected:
```toml
command = "code-snippets-plugin:explain"
description = "Explain code in a file with clear breakdowns"
prompt = """
# Explain Code Command
...
"""
```
**Fix required:** Parse YAML frontmatter from command `.md` file, extract `description`, use it in TOML and strip frontmatter block from `prompt` body.

---

## Action Items (Priority Order)

> **✅ All issues resolved!**

| Fix | Commit | Details |
|---|---|---|
| Nested commands (#003) | `bridge_installer.py` | `glob()` → `rglob()` with `plugin_subdir_cmd` flat naming |
| Agents bridging (#004) | `bridge_installer.py` | `agents/*.md` → `{target-skills}/{plugin}/agents/` on all 4 envs |
| Gemini frontmatter (#006) | `bridge_installer.py` | New `parse_frontmatter()`, real description in TOML, frontmatter stripped |
| Auto-detect message (#011) | `bridge_installer.py` | Clear error with `mkdir` command shown |
| Hooks bridging (#005) | `bridge_installer.py` | New `install_hooks()` → `.claude/hooks/{plugin}-hooks.json` |
| MCP merging (#007) | `bridge_installer.py` | New `merge_mcp_config()` → merges into root `.mcp.json` |