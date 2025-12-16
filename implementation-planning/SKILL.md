---
name: implementation-planning
version: 1.0.0
description: |
  Structured implementation planning template for features and fixes in Pantas Green projects.
  Use when: planning features, designing solutions, creating technical specs, writing implementation docs.
  Triggers: plan, implement, design, architecture, feature, spec, GRE-, jira, ticket, roadmap, proposal
---

# Implementation Planning

## Quick Reference

| Element | Format |
|---------|--------|
| **Branch** | `feature/GRE-XXXX-brief-description` or `bugfix/GRE-XXXX-brief-description` |
| **Commit** | `feat(scope): GRE-XXXX - description` |
| **PR Title** | `GRE-XXXX: [Feature/Fix Name]` |

## Before Starting

**IMPORTANT**: You MUST have a Jira ticket number before starting any implementation.

- **Jira Ticket**: GRE-XXXX
- **Branch Name**: `feature/GRE-XXXX-brief-description`
- **Commit Format**: `feat(scope): GRE-XXXX - description`
- **PR Title**: `GRE-XXXX: [Feature Name]`

## Plan Structure Overview

Every implementation plan should include:

1. **Executive Summary** - Problem, solution, impact, risk level
2. **Current State Analysis** - What exists, pain points, gaps
3. **Implementation Phases** - Tasks, deliverables, files changed
4. **Technical Details** - Database, code, configuration changes
5. **Testing Strategy** - Unit, integration, manual, edge cases
6. **Risk Assessment** - Technical/business risks, mitigation, rollback
7. **Success Criteria** - Measurable outcomes

## Output Format

**IMPORTANT**: Deliver as Markdown file (`.md`) that can be committed to repository.

- Use proper Markdown syntax
- Code blocks with language hints (```python, ```sql)
- Tables for structured data
- Checkboxes `- [ ]` for task lists
- Horizontal rules `---` between sections

## Detailed Template

See [plan-template.md](plan-template.md) for the full implementation plan template.
