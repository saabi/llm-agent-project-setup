# LLM Agent-Friendly Project Setup

A comprehensive guide and toolkit for structuring software projects to maximize effectiveness when working with LLM agents (like GitHub Copilot, Cursor, Claude, etc.).

## Purpose

This repository contains best practices, guidelines, and templates developed through iterative work across multiple projects. The goal is to help developers and teams structure their projects in ways that make LLM agents more effective collaborators.

## Contents

### Core Guides

- **[LLM Agent Project Setup Guide](./LLM_AGENT_PROJECT_SETUP.md)** - Complete guide for setting up new projects with LLM agent collaboration in mind
- **[LLM Agent Project Migration Guide](./LLM_AGENT_PROJECT_MIGRATION.md)** - Step-by-step guide for migrating existing projects to an LLM agent-friendly structure

### Process Guidelines

- **[Commit Guidelines](./docs/process/COMMITS.md)** - Conventional Commits format and best practices
- **[Ticket Guidelines](./docs/process/TICKETS.md)** - Ticket composition and issue tracking best practices
- **[Branch Management](./docs/process/BRANCH_MANAGEMENT.md)** - Git branch strategies for single and multiple agent workflows

### Example Files

- **`.cursorrules`** - Example Cursor IDE rules file that references the process guidelines

## Key Principles

1. **Clear Context** - Well-organized documentation with explicit cross-references
2. **Documented Decisions** - Rationale and alternatives are explained
3. **Consistent Structure** - Predictable patterns across the codebase
4. **Explicit Dependencies** - Clear relationships between components
5. **Preserved History** - Git history tells a coherent story

## Who Is This For?

- **Project Maintainers** setting up new projects
- **Technical Leads** migrating legacy systems
- **LLM Agent Operators** working with AI coding assistants
- **Development Teams** adopting AI-assisted workflows

## Quick Start

### For New Projects

1. Read the [LLM Agent Project Setup Guide](./LLM_AGENT_PROJECT_SETUP.md)
2. Copy the `.cursorrules` example and adapt it to your project
3. Set up your project structure following the recommended patterns
4. Configure your process guidelines in `docs/process/`

### For Existing Projects

1. Read the [LLM Agent Project Migration Guide](./LLM_AGENT_PROJECT_MIGRATION.md)
2. Assess your current project structure
3. Plan an incremental migration strategy
4. Apply the guidelines gradually, ensuring non-breaking changes

## Project Types Covered

- **Full-Stack Web Applications** (SvelteKit, Next.js, etc.)
- **Libraries and Packages**
- **Utilities and Tools**
- **Monorepos** with multiple applications/services
- **Legacy System Migrations**

## Branch Management Strategies

The guides include comprehensive branch management strategies for:
- Single agent workflows
- Multiple parallel agents
- Local-first development
- Remote collaboration
- Hybrid approaches

## Contributing

These guidelines were developed iteratively across multiple projects. Contributions, improvements, and real-world examples are welcome!

## License

This repository is provided as-is for educational and reference purposes. Feel free to adapt these guidelines to your projects.

## Related Resources

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)

---

**Note:** These guidelines are technology-agnostic but include examples from various stacks (SvelteKit, Next.js, PostgreSQL, etc.). Adapt the patterns to your specific technology choices.

