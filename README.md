# Antigravity Custom Agent Creator (`agy-agent-creator`)

> **Last reviewed:** 2026-07-12

An [Antigravity](https://github.com/google/antigravity) skill that creates, registers, and verifies custom **agents** and **subagents**. It enforces the **v1.0.16+ YAML schema** for `agent.md` configs, so agents register correctly on the first try.

---

## Why This Exists

After an Antigravity CLI update, I asked the AI to create a custom agent. It generated a config, but `agy agent` never listed the new agent. The AI kept hallucinating fixes — different file paths, renamed fields, restructured YAML — none of which worked. I'm not a developer, so diagnosing the schema myself took hours of trial and error.

Once I found the exact v1.0.16+ requirements, I built this skill so no one else has to repeat that process.

---

## What's Here

| File/Dir                                                   | Purpose                                    |
| ---------------------------------------------------------- | ------------------------------------------ |
| `SKILL.md`                                                 | Agent creation instructions.               |
| [`references/valid-agent.md`](references/valid-agent.md)   | Valid v1.0.16+ config example.             |
| [`references/legacy-agent.md`](references/legacy-agent.md) | Deprecated config example (what to avoid). |

---

## Features

- **Guided setup:** Prompts for agent name, scope, description, and tools.
- **Schema enforcement:** Generates `agent.md` with the required `system_prompt_config` section and individually listed tools.
- **Verification:** Runs `agy agent` after generation to confirm the agent registered.

---

## Installation

Install globally (all projects) or locally (one project).

### Global

Copy into your global Gemini config directory:

```bash
# Create target directory
mkdir -p ~/.gemini/config/skills/agy-agent-creator

# Copy files
cp SKILL.md ~/.gemini/config/skills/agy-agent-creator/
cp -R references ~/.gemini/config/skills/agy-agent-creator/
```

### Workspace

Copy into the project's `.agents` folder:

```bash
# Create target directory
mkdir -p .agents/skills/agy-agent-creator

# Copy files
cp SKILL.md .agents/skills/agy-agent-creator/
cp -R references .agents/skills/agy-agent-creator/
```

---

## How It Works

Triggers when you mention "creating an agent," "defining an agent profile," or "setting up custom subagents." The agent then:

1. **Collects metadata:** Asks for name, scope, description, and tools via choice menus.
2. **Generates the config:** Creates the target folder and writes `agent.md`.
3. **Verifies:** Runs `agy agent` to confirm registration.

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

MIT License.
