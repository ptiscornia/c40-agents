# C40 Agents

Central repository of AI agents for C40 organization. These agents provide specialized capabilities that can be invoked from other projects.

## What is this?

This repository contains reusable **agent definitions** - specialized AI assistants with domain knowledge, conventions, and workflows for specific tasks. Instead of re-explaining context in every project, teams can invoke pre-configured agents.

## Available Agents

| Agent | Description | Use Case |
|-------|-------------|----------|
| [rhino-expert](agents/rhino-expert/) | Rhino framework (Appsilon) expert for Shiny apps | R/Shiny development, vanilla-to-rhino conversion |

## Quick Start

### Using an Agent in Your Project

**Option 1: System Prompt Injection**

Copy the contents of `agents/<name>/prompt.md` into your LLM's system prompt or project context file.

**Option 2: Reference in Project Context**

Add a reference to the agent in your project's AI context file:

```markdown
## Available Agents

When working with R/Shiny code, use the rhino-expert agent from:
https://github.com/ptiscornia/c40-agents/agents/rhino-expert/prompt.md
```

**Option 3: Platform-Specific Integration**

Package as Custom GPT (OpenAI), Gem (Google), or other platform-specific formats.

## Repository Structure

```
c40-agents/
├── AGENT.md           # Context for LLM agents working here
├── README.md          # This file (for humans)
├── agents/            # Agent definitions
│   └── <agent-name>/
│       ├── agent.yaml     # Configuration
│       ├── prompt.md      # System prompt
│       ├── README.md      # Documentation
│       └── examples/      # Usage examples
├── shared/            # Shared resources
│   ├── prompts/       # Reusable prompt fragments
│   ├── tools/         # Common tools
│   └── schemas/       # Data schemas
├── docs/              # Extended documentation
└── tests/             # Agent tests
```

## Creating a New Agent

1. Create directory: `agents/<agent-name>/`
2. Add required files:
   - `agent.yaml` - Configuration and metadata
   - `prompt.md` - Full system prompt
   - `README.md` - Human documentation
3. Add at least one example in `examples/`
4. Update `AGENT.md` agents table

See [AGENT.md](AGENT.md) for detailed conventions.

## Agent Structure

Each agent follows a standard structure:

### agent.yaml

```yaml
name: my-agent
version: "1.0.0"
description: What the agent does
tags: [domain, capability]
requires:
  tools: [Read, Write, Edit]
defaults:
  model: gpt-4o  # or any LLM
```

### prompt.md

The complete system prompt with:
- Role and identity
- Domain knowledge
- Conventions and patterns
- Examples and workflows

## Contributing

1. Fork this repository
2. Create your agent following the conventions
3. Test with real use cases
4. Submit a pull request

## License

MIT License - See [LICENSE](LICENSE) for details.

---

**C40 Cities Climate Leadership Group**
