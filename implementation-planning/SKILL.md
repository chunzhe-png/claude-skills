---
name: implementation-planning
version: 2.0.0
description: |
  Comprehensive implementation planning guidelines for Pantas Green Django projects.
  Use when: planning features, designing solutions, creating technical specs, writing implementation docs.
  Triggers: plan, implement, design, architecture, feature, spec, GRE-, jira, ticket, roadmap, proposal
---

# Implementation Planning

## Overview

This skill guides you through creating comprehensive implementation plans. Unlike an agent, you (main Claude) execute each step directly.

## Quick Reference

| Element | Format |
|---------|--------|
| **Branch** | `feature/GRE-XXXX-brief-description` or `bugfix/GRE-XXXX-brief-description` |
| **Commit** | `feat(scope): GRE-XXXX - description` |
| **PR Title** | `GRE-XXXX: [Feature/Fix Name]` |

---

## Workflow

### Step 1: Get Jira Ticket Information

**CRITICAL**: You MUST have a Jira ticket number before proceeding.

If not provided, ask: "What is the Jira ticket number for this implementation?"

**Fetch ticket details using jira-agent:**
```
Use the Task tool with subagent_type="jira-agent" to fetch ticket GRE-XXXX details
```

Extract:
- Ticket title and description
- Priority and status
- Acceptance criteria (if available)

### Step 2: Read Project Standards

Before creating any plan:
1. Read `CLAUDE.md` for best practices
2. Read `docs/implementation_plan_template.md` for structure
3. Identify relevant rules (BP-*, C-*, T-*, D-*, L-*, G-*)

### Step 3: Analyze the Codebase

Use Grep and Glob to:
- Find existing related implementations
- Understand current patterns
- Identify files to modify
- Check for reusable functionality

### Step 4: Create the Plan

Generate a plan with these sections:

1. **Jira Ticket Information** - From jira-agent
2. **Executive Summary** - Problem, solution, impact, risk
3. **Current State Analysis** - What exists, gaps, pain points
4. **Implementation Phases** - Tasks, deliverables, files
5. **Technical Details** - Database, code, config changes
6. **Testing Strategy** - Unit, integration, manual tests
7. **Risk Assessment** - Risks, mitigation, rollback
8. **Success Criteria** - Measurable outcomes
9. **CLAUDE.md Compliance Checklist** - Rules to follow

---

## Plan Structure

Every implementation plan should include:

### Required Sections

```markdown
# [Feature/Fix Name] Implementation Plan

## Jira Ticket Information
- **Ticket**: GRE-XXXX
- **Title**: [From Jira]
- **URL**: https://pantas.atlassian.net/browse/GRE-XXXX

## Executive Summary
- **Problem**: [Brief description]
- **Solution**: [High-level approach]
- **Impact**: [Business/technical impact]
- **Risk Level**: [LOW/MEDIUM/HIGH]

## Current State Analysis
### What Exists
### Pain Points
### What's Missing
### What Can Be Reused

## Implementation Phases
### Phase 1: [Name]
- Tasks
- Files modified/created
- Deliverables

## Technical Details
### Database Changes
### Code Changes
### Configuration Changes

## Testing Strategy
### Unit Tests
### Integration Tests
### Manual Testing

## Risk Assessment
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|

### Rollback Plan

## Success Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Files Summary
### New Files
### Modified Files

## CLAUDE.md Compliance Checklist
[See below]
```

---

## CLAUDE.md Compliance Checklist

Every plan MUST end with this checklist:

```markdown
## CLAUDE.md Compliance Checklist

### Before Coding
- [ ] BP-1: Asked clarifying questions
- [ ] BP-2: Confirmed approach with user
- [ ] BP-3: Listed pros/cons of approach

### While Coding
- [ ] C-1: TDD workflow (tests first)
- [ ] C-6: Import ordering (stdlib, third-party, local)
- [ ] C-10: Constants for repeated strings
- [ ] C-11: No local imports inside functions

### Testing
- [ ] T-1: Tests in app's tests/ directory
- [ ] T-3: Unit tests use Mock (no database)
- [ ] T-7: Realistic test data (not 42 or "foo")

### Database
- [ ] D-1: N+1 prevention (select_related/prefetch_related)
- [ ] D-3: Migration strategy documented

### Logging
- [ ] L-1: exc_info=True in exception handlers
- [ ] L-4: No sensitive data in logs

### Quality Gates
- [ ] G-1: isort for import sorting
- [ ] G-2: autopep8 for formatting
- [ ] G-3: All tests passing
```

---

## Output Format

**IMPORTANT**: Deliver as Markdown file (`.md`) that can be committed to repository.

- Use proper Markdown syntax
- Code blocks with language hints (```python, ```sql)
- Tables for structured data
- Checkboxes `- [ ]` for task lists
- Horizontal rules `---` between sections

---

## Detailed Template

See [docs/implementation_plan_template.md](../../docs/implementation_plan_template.md) for the full template.

---

*Version: 2.0.0 - Converted from agent to skill*
