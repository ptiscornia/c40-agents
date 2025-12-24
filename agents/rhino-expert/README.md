# Rhino Expert Agent

Expert agent for the **rhino** framework (Appsilon) specialized in enterprise Shiny applications for C40 Data Portal.

## Quick Start

### When to Use

Use this agent when:
- Creating new Shiny modules for C40 Data Portal
- Converting vanilla Shiny code to rhino patterns
- Writing database queries against C40 PostgreSQL schema
- Debugging `box::use()` import issues
- Creating testthat or Cypress tests

### Invocation

**Claude Code CLI:**
```bash
# Reference this agent in your project's CLAUDE.md or settings
claude --agent c40-agents/agents/rhino-expert
```

**As Task subagent:**
```
Use the Task tool with the rhino-expert agent for Shiny/R code
```

## Capabilities

| Capability | Description |
|------------|-------------|
| Code Creation | Write rhino-compliant R code with `box::use()` imports |
| Code Review | Detect vanilla Shiny patterns, propose conversions |
| Debugging | Troubleshoot box imports, module exports, Sass/JS builds |
| Refactoring | Convert existing Shiny code to rhino architecture |
| Testing | Create testthat unit tests and Cypress E2E specs |
| Database | Write PostgreSQL queries using C40 schema |

## Key Conventions

### box::use() Imports (Never library())

```r
# Correct
box::use(
  shiny[moduleServer, NS, reactive],
  app/logic/queries[get_cities_list],
)

# Wrong - never use
library(shiny)
source("R/functions.R")
```

### Module Pattern

```r
# app/view/example.R
box::use(
  shiny[moduleServer, NS, tagList],
)

#' @export
ui <- function(id) {

  ns <- NS(id)
  tagList(...)
}

#' @export
server <- function(id) {
  moduleServer(id, function(input, output, session) {
    ...
  })
}
```

### Logic vs View Separation

| Folder | Purpose |
|--------|---------|
| `app/logic/` | Non-reactive business logic, queries |
| `app/view/` | Reactive Shiny modules |

## Project Context

- **Project**: C40 Data Portal
- **Framework**: rhino (Appsilon)
- **Database**: PostgreSQL on AWS RDS
- **UI**: bslib (Bootstrap 5)
- **Maps**: mapgl (Mapbox GL JS)
- **Hosting**: shinyapps.io Professional

## Files

| File | Purpose |
|------|---------|
| `agent.yaml` | Configuration and metadata |
| `prompt.md` | Full system prompt with all conventions |
| `examples/` | Usage examples |

## Related Agents

- None yet (first agent in repository)

## Changelog

### 1.0.0 (2024-12)
- Initial release
- Full rhino conventions documentation
- C40 database schema reference
- Brand guidelines included
