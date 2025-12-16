# Changelog

All notable changes to the Claude Code Skills will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2025-12-16

### Added

- **django-development** skill
  - Django best practices and coding standards
  - ORM patterns (N+1 prevention, transactions)
  - Testing patterns with TestCase
  - Quick commands (qnew, qplan, qcode, qcheck, qgit)
  - Supporting files: best-practices.md, testing-patterns.md

- **github-workflow** skill
  - Branch naming conventions (GRE-XXXX format)
  - Commit message format (conventional commits)
  - PR templates with UAT test plans
  - CI troubleshooting guide
  - Code formatting requirements (autopep8, prettier)
  - Supporting files: pr-templates.md, ci-troubleshooting.md

- **implementation-planning** skill
  - Structured implementation plan template
  - Jira ticket integration
  - Phase-based planning approach
  - Risk assessment and mitigation
  - Supporting files: plan-template.md

- **testing** skill
  - pytest-django patterns (NOT Django TestCase)
  - Vitest patterns for frontend
  - Fixture patterns and mocking
  - Troubleshooting guide
  - Supporting files: backend-patterns.md, frontend-patterns.md

### Infrastructure

- Skills directory structure for devcontainer integration
- README.md with usage instructions
- CHANGELOG.md for version tracking
- VERSION file for easy version checking

---

## Version History Summary

| Version | Date | Highlights |
|---------|------|------------|
| 1.0.0 | 2025-12-16 | Initial release with 4 skills |

---

## Migration Notes

### From CLAUDE.md Router System

If migrating from the previous CLAUDE.md router system:

1. Skills replace manual `Read` tool loading
2. No routing logic needed in CLAUDE.md
3. Project CLAUDE.md should be slim (project-specific only)
4. Skills auto-activate based on description matching

### Updating Skills

```bash
# Check current version
cat skills/VERSION

# Or via git
cd skills && git describe --tags
```

---

[Unreleased]: https://github.com/chunzhe-png/claude-skills/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/chunzhe-png/claude-skills/releases/tag/v1.0.0
