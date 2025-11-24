---
name: LLM Agent Configuration Examples
about: Add specific configuration examples for individual LLM agents
title: '[DOCS] Add configuration example for [AGENT_NAME]'
labels: 'documentation, enhancement, examples'
assignees: ''
---

## Summary

Add a configuration example and documentation for [AGENT_NAME] to demonstrate how to set up this project structure for use with this specific LLM agent.

## Agent Information

**Agent Name:** [e.g., GitHub Copilot, Claude, Codeium, JetBrains AI, Tabnine]

**Configuration Method:** [How does this agent read project rules/instructions?]
- File location: [e.g., `.github/copilot-instructions.md`]
- File format: [e.g., Markdown, YAML, JSON]
- Naming convention: [e.g., specific filename required?]

**Agent Capabilities:**
- Context window: [if known]
- Special features: [e.g., codebase indexing, chat interface, etc.]
- Integration method: [IDE plugin, web interface, CLI, etc.]

## Requirements

- [ ] **Create configuration file example:**
  - [ ] Add example file following agent's conventions
  - [ ] Reference shared documentation (process guidelines, etc.)
  - [ ] Include agent-specific optimizations if applicable

- [ ] **Document agent-specific considerations:**
  - [ ] How this agent differs from others
  - [ ] Best practices specific to this agent
  - [ ] Limitations or special requirements
  - [ ] Integration with project structure

- [ ] **Update main documentation:**
  - [ ] Add to README list of supported agents
  - [ ] Reference in multi-agent coexistence guide
  - [ ] Link from LLM_AGENT_PROJECT_SETUP.md

- [ ] **Provide usage examples:**
  - [ ] How to set up this agent for a new project
  - [ ] How to add this agent to an existing project
  - [ ] Example interactions or workflows

## Configuration File Structure

**Proposed file location:** `[path/to/agent-config]`

**Proposed content structure:**
```markdown
# [Agent Name] Configuration

[Brief description of what this file does]

## Project Guidelines

This project follows the LLM agent-friendly structure. See:
- [Process Guidelines](../docs/process/COMMITS.md)
- [Ticket Guidelines](../docs/process/TICKETS.md)
- [Branch Management](../docs/process/BRANCH_MANAGEMENT.md)

## Agent-Specific Instructions

[Agent-specific guidance, if any]

## Project Structure

[Reference to project structure documentation]

## Key Principles

[Reference to core principles from main docs]
```

## Acceptance Criteria

- [ ] Configuration file example created and tested
- [ ] Documentation added explaining agent-specific setup
- [ ] Example demonstrates integration with shared project guidelines
- [ ] README updated to include this agent
- [ ] Multi-agent guide references this agent
- [ ] Example shows how this agent can coexist with others

## Related Issues

- Related to: #[ISSUE_NUMBER] (Multi-Agent Support and Coexistence)
- Blocks: [Any issues this unblocks]
- Blocked by: [Any dependencies]

## Additional Information

**Research Sources:**
- [Link to agent documentation]
- [Link to community examples]
- [Link to relevant discussions]

**Testing:**
- [ ] Configuration file tested with actual agent
- [ ] Verified it works with project structure
- [ ] Confirmed coexistence with other agents (if applicable)

---

**Created:** YYYY-MM-DD  
**Type:** Documentation  
**Category:** Examples, Agent Configuration

