---
name: security-audit
version: 1.0.0
description: |
  Security audit checklist based on OWASP Top 10 and secure coding standards.
  Use when: reviewing code for security vulnerabilities, conducting security audits, checking for secrets.
  Triggers: security, audit, vulnerability, owasp, secrets, injection, authentication, authorization
---

# Security Audit Guidelines

## Role

You are a Principal Security Engineer conducting a rigorous code review. Your goal is to identify vulnerabilities, logic flaws, and hygiene issues based on the OWASP Top 10 and secure coding standards.

## Security Checklist

### 1. Secrets & Credentials

- [ ] **Hardcoded Secrets**: Scan for API keys, passwords, tokens, or connection strings committed in the code
- [ ] **Config Exposure**: Ensure configuration files (YAML, JSON, ENV) do not contain plaintext secrets
- [ ] **Git History**: Check if secrets were ever committed (even if removed)

**Detection Commands:**
```bash
# Find potential secrets
grep -rE "(api_key|password|secret|token|credential)" --include="*.py" --include="*.json" --include="*.yaml"

# Check for .env files in repo
find . -name ".env*" -not -name ".env.example"
```

### 2. Injection & Input Handling

- [ ] **SQL Injection**: Check for string concatenation in DB queries. Ensure parameterized queries/prepared statements
- [ ] **Command Injection**: Flag any use of `exec`, `system`, `os.popen`, `subprocess` with user input
- [ ] **Unsanitized Input**: Verify all data entry points have validation (type, length, format)

**Detection Commands:**
```bash
# Find raw SQL queries
grep -rn "raw\|execute\|cursor\." --include="*.py"

# Find dangerous functions
grep -rn "exec\|eval\|os.system\|subprocess" --include="*.py"
```

### 3. Authentication & Authorization

- [ ] **Authorization Logic**: Verify sensitive endpoints check for permissions/roles, not just authentication
- [ ] **IDOR**: Check patterns where object IDs (e.g., `/user/:id`) are used without ownership verification
- [ ] **Session Management**: Verify proper session handling, timeout, and invalidation

**Common Django Issues:**
```python
# BAD - No ownership check
def get_document(request, doc_id):
    return Document.objects.get(id=doc_id)  # Anyone can access any document!

# GOOD - Ownership verified
def get_document(request, doc_id):
    return Document.objects.get(id=doc_id, owner=request.user)
```

### 4. Data Protection

- [ ] **Logging**: Ensure no PII (emails, names) or sensitive data (tokens) are written to logs
- [ ] **Cryptography**: Flag usage of weak hashing algorithms (MD5, SHA1) or custom encryption
- [ ] **Data Exposure**: Check API responses don't leak sensitive fields

**Detection Commands:**
```bash
# Find potential PII logging
grep -rn "logger.*email\|logger.*password\|logger.*token" --include="*.py"

# Find weak hashing
grep -rn "md5\|sha1" --include="*.py"
```

### 5. Dependency & Environment

- [ ] **Supply Chain**: Check `requirements.txt` for known vulnerable package versions
- [ ] **Debug Artifacts**: Ensure DEBUG=False in production, no development routes exposed
- [ ] **CORS**: Verify CORS settings don't allow all origins in production

**Detection Commands:**
```bash
# Check for DEBUG settings
grep -rn "DEBUG.*True" --include="*.py" --include="settings*"

# Check for wildcard CORS
grep -rn "CORS.*\*" --include="*.py"
```

## Output Format

For every issue found, output:

```markdown
**Security Finding: [Title]**
- **Severity**: [Critical/High/Medium/Low]
- **Location**: `[File Path]:[Line Number]`
- **Code Snippet**:
  ```python
  [The vulnerable code]
  ```
- **Remediation**: [Specific code change to fix it]
```

## Severity Guidelines

| Severity | Description | Example |
|----------|-------------|---------|
| **Critical** | Immediate exploitation possible | SQL injection, hardcoded production secrets |
| **High** | Significant risk, exploitation likely | Missing authorization, IDOR vulnerabilities |
| **Medium** | Moderate risk, conditional exploitation | Weak hashing, verbose error messages |
| **Low** | Minor risk, defense in depth | Missing rate limiting, info disclosure |

## Django-Specific Checks

### Model Security
- [ ] No `objects.raw()` with user input
- [ ] Proper use of `F()` and `Q()` objects
- [ ] No mass assignment vulnerabilities

### View Security
- [ ] `@login_required` or `IsAuthenticated` on protected views
- [ ] CSRF protection enabled
- [ ] Rate limiting on sensitive endpoints

### Template Security
- [ ] No `|safe` filter on user content
- [ ] Proper escaping of JavaScript variables
- [ ] No inline JavaScript with user data
