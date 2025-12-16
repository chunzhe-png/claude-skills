# CI Troubleshooting Guide

## PR Information Retrieval

### Step 1: Check PR CI Status
```bash
gh pr checks <pr_number>
```

**Example output**:
```
Autopep8         fail    0       https://github.com/.../runs/58187704221
Python tests     pending 0       https://github.com/.../runs/58187525362
Run linters      pass    1m58s   https://github.com/.../runs/58187525358
```

### Step 2: Get Review Comments
```bash
gh api repos/<owner>/<repo>/pulls/<pr_number>/comments
```

### Step 3: Get CI Check Annotations
Extract `check_run_id` from failed check URL, then:
```bash
gh api repos/<owner>/<repo>/check-runs/<check_run_id>/annotations
```

**Example output**:
```json
[{
  "path": "accounts/tests/test_previous_login.py",
  "start_line": 104,
  "end_line": 111,
  "annotation_level": "failure",
  "message": "-        self.assertEqual(...)\n+        self.assertEqual(\n+                         ...)"
}]
```

### Step 4: View Full CI Logs
```bash
gh run view <run_id> --log
gh run view <run_id> --log-failed 2>&1 | head -100
```

## CI Fix Workflows

### Autopep8/Linting Failures

1. Check which check failed:
   ```bash
   gh pr checks <pr_number>
   ```

2. Get check_run_id from failed check URL

3. Get exact error locations:
   ```bash
   gh api repos/<owner>/<repo>/check-runs/<check_run_id>/annotations
   ```

4. Fix issues based on diff in message field

5. Verify locally:
   ```bash
   autopep8 --diff <file>  # Should show no output if fixed
   ```

6. Commit and push:
   ```bash
   git add <files> && git commit -m "style: fix autopep8 formatting" && git push
   ```

### Test Failures

1. Check which test failed:
   ```bash
   gh run view <run_id> --log-failed 2>&1 | grep -E "FAILED|ERROR|AssertionError" | head -20
   ```

2. Get detailed failure info:
   ```bash
   gh run view <run_id> --log 2>&1 | grep -A 10 "FAILED"
   ```

3. Fix the test or code issue

4. Run tests locally:
   ```bash
   docker compose exec django python manage.py test <test_path> -v 2
   ```

5. Commit and push

### CodeRabbit Issues

1. Get all review comments:
   ```bash
   gh api repos/<owner>/<repo>/pulls/<pr_number>/comments
   ```

2. Address each issue:
   - Valid concern: Fix the code
   - False positive: Reply to explain

3. Commit fixes:
   ```bash
   git commit -m "fix: address CodeRabbit review feedback"
   ```

## Quick Reference Commands

| Task | Command |
|------|---------|
| Check CI status | `gh pr checks <pr>` |
| View PR details | `gh pr view <pr>` |
| Get review comments | `gh api repos/.../pulls/<pr>/comments` |
| Get check annotations | `gh api repos/.../check-runs/<id>/annotations` |
| View CI logs | `gh run view <run_id> --log` |
| View failed logs | `gh run view <run_id> --log-failed` |

## Common CI Failure Patterns

### Autopep8 (Python Formatting)
- **Symptom**: `Autopep8` check fails
- **Fix**: `autopep8 --in-place <file>`
- **Common issues**: Line too long (E501), whitespace, indentation

### Test Import Errors
- **Symptom**: `ModuleNotFoundError: No module named 'pytest'`
- **Cause**: Test uses pytest but CI uses Django test runner
- **Fix**: Convert pytest-style to Django TestCase

### Database Migration Conflicts
- **Symptom**: `duplicate key value violates unique constraint`
- **Cause**: Migration conflicts with existing data
- **Fix**: Check migration necessity, consider squash/reset
