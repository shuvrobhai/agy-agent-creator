# Antigravity Custom Agent Creator (`agy-agent-creator`)

> **Last reviewed:** 2026-07-12

An [Antigravity](https://github.com/google/antigravity) skill designed to help AI agents and users interactively create, register, and verify custom subagents or profiles. It enforces the strictly validated **v1.0.16+ YAML schema** for `agent.md` configs, preventing silent registration failures.

---

## What's Here

| File/Dir | Purpose |
|----------|---------|
| `SKILL.md` | The core instruction file providing the agent creation protocol. |
| [`references/valid-agent.md`](references/valid-agent.md) | Example of a valid configuration adhering to the v1.0.16+ schema. |
| [`references/legacy-agent.md`](references/legacy-agent.md) | Example of a legacy configuration showing deprecated patterns. |

---

## Features

- **Interactive Setup:** Prompts the user step-by-step for the agent name, scope, description, and tools list.
- **Strict Schema Enforcement:** Generates `agent.md` with the mandatory `system_prompt_config` section and individually listed tools.
- **Auto-verification:** Instructs the creating agent to verify successful registration with the `agy` CLI post-generation.

---

## Installation

You can install this skill globally (for all projects) or workspace-locally (for a specific project).

### Global Installation

To make the custom agent creator skill available globally across all of your workspaces, copy it into your global Gemini config directory:

```bash
# Create target directory
mkdir -p ~/.gemini/config/skills/agy-agent-creator

# Copy files
cp SKILL.md ~/.gemini/config/skills/agy-agent-creator/
cp -R references ~/.gemini/config/skills/agy-agent-creator/
```

### Workspace Installation

To restrict the skill to a specific workspace, copy it into the project's local `.agents` folder:

```bash
# Create target directory
mkdir -p .agents/skills/agy-agent-creator

# Copy files
cp SKILL.md .agents/skills/agy-agent-creator/
cp -R references .agents/skills/agy-agent-creator/
```

---

## How It Works

Once installed, this skill is triggered whenever you mention "creating an agent", "defining an agent profile", or "setting up custom subagents". The agent will then:

1. **Collect Metadata Interactively:** Ask for the name, scope, description, and tools required using choice menus when possible.
2. **Generate the Config File:** Create the target folder and write `agent.md` with the strictly validated schema structure.
3. **Verify:** Check if the agent registers correctly with the `agy` CLI and report back.

### The Target v1.0.16+ Schema

This skill outputs configurations using the format:

```yaml
---
name: backend-expert
description: Expert in designing and building robust APIs and backend systems.
hidden: false
tools:
  - send_message
  - grep_search
  - view_file
  - list_dir
  - search_web
  - read_url_content
system_prompt_config:
  include_sections:
    - user_information
    - mcp_servers
    - skills
    - messaging
    - artifacts
    - user_rules
---

# Backend Expert Persona

System prompt directives and instructions for the agent go here...
```

> [!IMPORTANT]
> - Tool groups (e.g. `Read`, `Grep`, `Glob`, `Write`) are **deprecated** in v1.0.16+. Tools must be listed individually.
> - The `system_prompt_config` structure is **mandatory**; omitting it causes validation to fail.

---

## License

This project is licensed under the MIT License.
