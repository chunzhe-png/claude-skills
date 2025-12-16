# Claude Code Skills

[![GitHub release](https://img.shields.io/github/v/release/pantas-green/pantas-claude-skills)](https://github.com/pantas-green/pantas-claude-skills/releases)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Reusable Claude Code skills for Pantas Green projects. Skills auto-activate based on context - no manual loading required.

**Repository**: https://github.com/pantas-green/pantas-claude-skills

## What Are Skills?

Skills are auto-discoverable capabilities that extend Claude Code:

- **Auto-activate** based on your prompt (no manual invocation needed)
- **Progressive disclosure** - details load only when needed
- **Composable** - multiple skills can work together
- **Version controlled** - semantic versioning with git tags

## Available Skills

| Skill | Description | Triggers |
|-------|-------------|----------|
| [django-development](django-development/) | Django best practices, ORM patterns, testing | django, model, view, serializer, migration |
| [github-workflow](github-workflow/) | Git conventions, PR templates, CI troubleshooting | commit, push, PR, branch, git, GRE- |
| [implementation-planning](implementation-planning/) | Structured feature planning template | plan, implement, design, feature, spec |
| [testing](testing/) | pytest-django and Vitest patterns | test, pytest, vitest, mock, fixture |

## Quick Start

### As Git Submodule (Recommended)

```bash
# Add to your project/devcontainer
git submodule add https://github.com/pantas-green/pantas-claude-skills.git skills

# Pin to specific version
cd skills
git checkout v1.0.0
cd ..
git add skills
git commit -m "Add claude-skills submodule at v1.0.0"
```

### Manual Installation

```bash
# Clone directly
git clone https://github.com/pantas-green/pantas-claude-skills.git ~/.claude/skills

# Or symlink
ln -sf /path/to/claude-skills ~/.claude/skills
```

## How Skills Work

1. You write a prompt: "Create a Django model for tracking emissions"
2. Claude Code reads your prompt and checks skill descriptions
3. Matching skills activate automatically (django-development in this case)
4. Skill instructions guide Claude's response

## Directory Structure

```
claude-skills/
├── README.md                      # This file
├── CHANGELOG.md                   # Version history
├── VERSION                        # Current version (1.0.0)
│
├── django-development/
│   ├── SKILL.md                   # Main skill definition
│   ├── best-practices.md          # Detailed guidelines
│   └── testing-patterns.md        # Testing reference
│
├── github-workflow/
│   ├── SKILL.md
│   ├── pr-templates.md
│   └── ci-troubleshooting.md
│
├── implementation-planning/
│   ├── SKILL.md
│   └── plan-template.md
│
└── testing/
    ├── SKILL.md
    ├── backend-patterns.md
    └── frontend-patterns.md
```

## Version Management

### Check Current Version

```bash
cat VERSION
# or
git describe --tags
```

### Update to New Version

```bash
# If using as submodule
cd skills
git fetch --tags
git tag -l                    # List available versions
git checkout v1.1.0           # Checkout new version

# Commit in parent repo
cd ..
git add skills
git commit -m "chore: update claude-skills to v1.1.0"
```

### Versioning Convention

We use [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes (skill renamed, incompatible changes)
- **MINOR**: New features (new skill added, new supporting files)
- **PATCH**: Bug fixes (typos, clarifications)

## Adding a New Skill

1. Create skill directory:
   ```bash
   mkdir skills/new-skill-name
   ```

2. Create SKILL.md with frontmatter:
   ```yaml
   ---
   name: new-skill-name
   version: 1.0.0
   description: |
     What it does. When to use it.
     Triggers: keyword1, keyword2, keyword3
   ---

   # New Skill Name

   ## Quick Reference
   ...
   ```

3. Add supporting files if needed

4. Update this README

5. Tag new version:
   ```bash
   git add .
   git commit -m "feat: add new-skill-name skill"
   git tag -a v1.x.0 -m "Add new-skill-name"
   git push origin main --tags
   ```

## Testing Skills

To verify a skill activates correctly:

1. Start Claude Code
2. Use a prompt with trigger keywords
3. Check if skill guidance appears in response

Example tests:
- "commit this code" → should use github-workflow
- "create a Django model" → should use django-development
- "plan this feature" → should use implementation-planning
- "write tests for this" → should use testing

## Integration with Devcontainer

This repository is designed to be used as a git submodule in the [devcontainer](https://github.com/pantas-green/pantas-devcontainer) setup:

```
devcontainer/
├── skills/              # This repo as submodule
├── scripts/
│   └── setup-claude.sh  # Symlinks skills to ~/.claude/skills/
└── devcontainer.json    # Runs setup on container start
```

## Troubleshooting

### Skill Not Activating

1. Check skill is in `~/.claude/skills/`:
   ```bash
   ls ~/.claude/skills/
   ```

2. Verify SKILL.md has valid frontmatter:
   ```bash
   head -20 ~/.claude/skills/skill-name/SKILL.md
   ```

3. Check description includes trigger keywords

### Submodule Issues

```bash
# Update to committed version
git submodule update --force

# Re-initialize
git submodule deinit -f skills
git submodule update --init skills
```

## Contributing

1. Fork this repository
2. Create a feature branch
3. Make changes and test skill activation
4. Update CHANGELOG.md
5. Create PR with description of changes
6. After merge, maintainer will tag new version

## Related Repositories

- **Devcontainer**: https://github.com/pantas-green/pantas-devcontainer - Shared devcontainer setup that uses these skills

## License

MIT License - see [LICENSE](LICENSE) for details.

---

*Version: 1.0.0*
*Last Updated: 2025-12-16*
*Repository: https://github.com/pantas-green/pantas-claude-skills*
