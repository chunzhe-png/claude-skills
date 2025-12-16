# Pull Request Templates

## PR Title Format

`GRE-<ticket>: <description> (<component>)`

**Examples**:
- `GRE-4160: Display last sign-on information after successful login (Backend)`
- `GRE-4160: Display last sign-on information after successful login (Frontend)`

## PR Body Template

```markdown
## Summary

<1-2 sentence description of what this PR does>

### Changes
- **<Component/Area>**: <What changed>
- **<Component/Area>**: <What changed>

### Files Changed
| File | Change |
|------|--------|
| `path/to/file.py` | Description of change |
| `path/to/file.tsx` | Description of change |

## UAT Test Plan

### Prerequisites
- <Any setup required>
- <Dependencies on other PRs>

### Test Steps
1. **<Test Scenario>**
   - [ ] Step 1
   - [ ] Step 2
   - [ ] Expected result

2. **<Test Scenario>**
   - [ ] Step 1
   - [ ] Step 2
   - [ ] Expected result

### Related PR
- <Link to related PRs if applicable>
```

## Multi-Repository Workflow

When changes span multiple repos (backend + frontend):

1. **Use the same branch name** in both repos
2. **Use the same ticket number** in both PR titles
3. **Link PRs to each other** in "Related PR" section
4. **Indicate component** in PR title: `(Backend)` or `(Frontend)`

**Example**:
- Backend PR: `GRE-4160: Display last sign-on info (Backend)`
- Frontend PR: `GRE-4160: Display last sign-on info (Frontend)`

## Git Commands Reference

### Creating a Feature Branch
```bash
git checkout -b GRE-<ticket>-<description>
```

### Committing with HEREDOC
```bash
git commit -m "$(cat <<'EOF'
feat: Description here

- Detail 1
- Detail 2
EOF
)"
```

### Push and Set Upstream
```bash
git push -u origin GRE-<ticket>-<description>
```

### Create PR with GitHub CLI
```bash
gh pr create --title "GRE-<ticket>: Description (Component)" --body "$(cat <<'EOF'
## Summary
...
EOF
)"
```
