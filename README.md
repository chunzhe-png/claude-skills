# Claude Code Skills

This directory contains reusable Claude Code skills for Pantas Green projects.

## What Are Skills?

Skills are auto-discoverable capabilities that extend Claude Code. Unlike manual context loading, skills:

- **Auto-activate** based on your prompt (no manual invocation needed)
- **Progressive disclosure** - details load only when needed
- **Composable** - multiple skills can work together

## Available Skills

| Skill | Description | Triggers |
|-------|-------------|----------|
| [django-development](django-development/) | Django best practices, ORM patterns, testing | django, model, view, serializer, migration |
| [github-workflow](github-workflow/) | Git conventions, PR templates, CI troubleshooting | commit, push, PR, branch, git, GRE- |
| [implementation-planning](implementation-planning/) | Structured feature planning template | plan, implement, design, feature, spec |
| [testing](testing/) | pytest-django and Vitest patterns | test, pytest, vitest, mock, fixture |

## How Skills Work

1. You write a prompt: "Create a Django model for tracking emissions"
2. Claude Code reads your prompt and checks skill descriptions
3. Matching skills activate automatically (django-development in this case)
4. Skill instructions guide Claude's response

## Directory Structure

```
skills/
├── README.md                      # This file
├── CHANGELOG.md                   # Version history
├── VERSION                        # Current version
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

## Usage

### In Devcontainer (Automatic)

Skills are automatically set up when the devcontainer starts:
1. `postCreateCommand` runs `setup-claude.sh`
2. Skills directory is symlinked to `~/.claude/skills/`
3. Claude Code discovers skills automatically

### Manual Setup

```bash
# Symlink to Claude's skills directory
ln -sf /path/to/devcontainer/skills ~/.claude/skills

# Or copy
cp -r /path/to/devcontainer/skills ~/.claude/
```

## Version Management

### Using Git Submodules

If skills are managed as a git submodule:

```bash
# Check current version
cd skills
git describe --tags

# Update to new version
git fetch --tags
git checkout v1.1.0

# Commit the update (in parent repo)
cd ..
git add skills
git commit -m "chore: update claude-skills to v1.1.0"
```

### Versioning Convention

We use semantic versioning:
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

### Symlink Issues

If symlinks don't work (e.g., Docker):
```bash
# Copy instead of symlink
cp -r /path/to/skills ~/.claude/skills
```

## Contributing

1. Make changes in a feature branch
2. Test skill activation
3. Update CHANGELOG.md
4. Create PR with description of changes
5. After merge, tag new version

---

*Version: 1.0.0*
*Last Updated: 2025-12-16*
