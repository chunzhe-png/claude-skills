---
name: github-workflow
version: 1.0.0
description: |
  Git and GitHub conventions for commits, branches, and PRs in Pantas Green projects.
  Use when: committing code, creating branches, making PRs, fixing CI failures, git operations.
  Triggers: commit, push, PR, pull request, branch, git, gh, GRE-, autopep8, CI, merge, rebase
---

# GitHub Workflow

## Quick Reference

| Action | Format |
|--------|--------|
| **Branch** | `GRE-<ticket>-<description>` |
| **Commit** | `<type>: <description>` |
| **PR Title** | `GRE-<ticket>: <description> (<component>)` |

## Branch Naming

**Format**: `GRE-<ticket-number>-<short-description>`

**Examples**:
- `GRE-4160-display-last-sign-on-info`
- `GRE-4111-fix-pim-backfill`

**Rules**:
- Always prefix with Jira ticket number
- Use lowercase letters
- Use hyphens to separate words
- Keep description short (3-5 words)

## Commit Message Format

```
<type>: <short description>

- <bullet point details>
- <bullet point details>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code refactoring
- `docs`: Documentation changes
- `test`: Adding/updating tests
- `chore`: Maintenance tasks
- `style`: Formatting changes

## Before Committing (MANDATORY)

**ALWAYS run code formatters:**

```bash
# Python (Backend)
autopep8 --in-place --aggressive --aggressive <files>

# TypeScript/JavaScript (Frontend)
npx prettier --write <files>
```

## Commit Workflow

1. Check git status
2. Ask which files (if not specified)
3. Ask for ticket number (if not known)
4. **Run code formatters**
5. Stage files: `git add <files>`
6. Create commit with proper format
7. Verify with `git status`

## PR Workflow

1. Check branch status (not on master)
2. Push if needed with `-u` flag
3. Gather context (ticket, diff, related PRs)
4. Create PR with `gh pr create`
5. Return PR URL

## Important Notes

- **ALWAYS run formatters before committing**
- Always ask for Jira ticket if not provided
- Never commit `.env`, credentials, or secrets
- Never commit `node_modules/` or generated files
- Always include UAT test plan in PRs
- Link related PRs for multi-repo changes

## Detailed Guides

- [PR Templates](pr-templates.md)
- [CI Troubleshooting](ci-troubleshooting.md)
