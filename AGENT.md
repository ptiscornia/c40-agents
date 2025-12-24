# C40 Agents Repository

> **For LLM Agents**: This file describes the project structure and conventions for AI assistants working on this codebase.

## Project Overview

This repository serves as the **central agent library for C40 organization**. It contains reusable, well-documented agents that can be invoked from other projects when specialized AI capabilities are needed.

### Purpose

- Centralize agent definitions for consistency across C40 projects
- Enable easy discovery and integration of agents
- Maintain quality standards and documentation for all agents
- Facilitate sharing of specialized capabilities across teams

## Repository Structure

```
c40-agents/
├── AGENT.md              # This file - context for LLM agents
├── README.md             # Human-readable documentation
├── agents/               # Agent definitions
│   ├── <agent-name>/     # Each agent in its own directory
│   │   ├── agent.yaml    # Agent configuration and metadata
│   │   ├── prompt.md     # System prompt / instructions
│   │   ├── tools/        # Custom tools (if any)
│   │   └── examples/     # Usage examples
│   └── ...
├── shared/               # Shared utilities across agents
│   ├── prompts/          # Reusable prompt fragments
│   ├── tools/            # Common tools
│   └── schemas/          # Shared data schemas
├── docs/                 # Extended documentation
│   ├── creating-agents.md
│   ├── integration-guide.md
│   └── best-practices.md
└── tests/                # Agent tests and evaluations
```

## Agent Definition Standard

Each agent MUST include:

### 1. `agent.yaml` - Configuration

```yaml
name: agent-name
version: "1.0.0"
description: Brief description of what the agent does
author: team-name
tags: [category, domain, capability]

# Capabilities and requirements
requires:
  tools: [list, of, required, tools]
  context: [types, of, context, needed]

# Invocation settings
defaults:
  model: gpt-4o  # or any supported model (claude, gemini, etc.)
  temperature: 0.7
  max_tokens: 4096
```

### 2. `prompt.md` - System Instructions

The agent's system prompt defining:
- Role and identity
- Capabilities and limitations
- Output format expectations
- Domain-specific knowledge

### 3. `examples/` - Usage Examples

At minimum one example showing:
- Input format
- Expected output
- Integration code snippet

## Conventions

### Naming

- Agent directories: `kebab-case` (e.g., `data-analyst`, `code-reviewer`)
- Files: `snake_case.py` or `kebab-case.md`
- Agent names in configs: `kebab-case`

### Documentation

- Every agent MUST have a README.md in its directory
- Include at least one practical example
- Document all required inputs and expected outputs
- List any external dependencies

### Quality Standards

- Agents should be focused on a specific task or domain
- Avoid overlapping capabilities between agents
- Include test cases for validation
- Version agents semantically

## How to Use Agents from Other Projects

### Discovery

```bash
# List available agents
ls agents/

# View agent details
cat agents/<agent-name>/agent.yaml
```

### Integration Patterns

**1. System Prompt Injection**
Copy the agent's `prompt.md` content into your LLM's system prompt.

**2. As MCP Server**
Agents can be exposed as MCP servers for tool-based integration.

**3. As Custom GPT / Gem / Skill**
Package the agent for platform-specific integrations (OpenAI GPTs, Google Gems, etc.).

## For LLM Agents Working Here

### When Creating a New Agent

1. Create directory under `agents/<agent-name>/`
2. Add required files: `agent.yaml`, `prompt.md`, `README.md`
3. Include at least one example
4. Add tests if applicable
5. Update the agents index if one exists

### When Modifying Existing Agents

1. Maintain backward compatibility when possible
2. Update version in `agent.yaml`
3. Document changes in agent's CHANGELOG if present
4. Run existing tests to ensure no regressions

### Code Style

- Python: Follow PEP 8, use type hints
- TypeScript: Use strict mode, prefer interfaces
- YAML: 2-space indentation
- Markdown: Use ATX headers (#), one sentence per line

## Current Agents

| Agent | Description | Status |
|-------|-------------|--------|
| [rhino-expert](agents/rhino-expert/) | Expert in rhino framework (Appsilon) for enterprise Shiny apps. Converts vanilla Shiny to rhino patterns. | Active |

## Contact

- Organization: C40
- Repository maintainers: [Add maintainer info]

---

*Last updated: 2024-12*
