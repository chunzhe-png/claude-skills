---
name: testing
version: 1.0.0
description: |
  Testing guide for GRE projects covering pytest-django (backend) and Vitest (frontend).
  Use when: writing tests, debugging test failures, setting up test fixtures, mocking.
  Triggers: test, pytest, vitest, unittest, mock, fixture, TestCase, assert, coverage, TDD
---

# Testing Guide

## Quick Reference

| Backend (pytest-django) | Frontend (Vitest) |
|------------------------|-------------------|
| `@pytest.mark.django_db` | `describe()`, `it()` |
| `@pytest.fixture` | `beforeEach()` |
| `assert x == y` | `expect(x).toBe(y)` |
| `./run_tests.sh backend` | `./run_tests.sh frontend` |

## Test Runner Commands

```bash
# All tests
./run_tests.sh

# Backend only
./run_tests.sh backend
./run_tests.sh backend/test_file.py
./run_tests.sh backend --fresh-db

# Frontend only
./run_tests.sh frontend
./run_tests.sh frontend ComponentName
```

## Backend: Use pytest Style (NOT Django TestCase)

**WRONG:**
```python
from django.test import TestCase

class MyTest(TestCase):  # DON'T USE
    def setUp(self):
        self.user = User.objects.create_user(...)
```

**CORRECT:**
```python
import pytest

@pytest.mark.django_db
class TestMyFeature:
    @pytest.fixture
    def user(self, django_user_model):
        return django_user_model.objects.create_user(
            email='test@example.com',
            password='testpass123'
        )

    def test_something(self, user):
        assert user.email == 'test@example.com'
```

## Key Differences

| Django TestCase | pytest-django |
|-----------------|---------------|
| `class MyTest(TestCase):` | `@pytest.mark.django_db` + `class TestMy:` |
| `def setUp(self):` | `@pytest.fixture` |
| `self.assertEqual(a, b)` | `assert a == b` |
| `self.assertIsNone(x)` | `assert x is None` |
| `self.assertTrue(x)` | `assert x is True` |

## Frontend: Vitest (NOT Jest)

```typescript
import { describe, it, expect, vi } from 'vitest';

describe('formatLastLogin', () => {
  it('returns "Never" for null dates', () => {
    expect(formatLastLogin(null)).toBe('Never');
  });
});
```

**Filter pattern**: `npm test -- ComponentName` (positional arg, NOT `--testPathPattern`)

## Detailed Guides

- [Backend Patterns](backend-patterns.md)
- [Frontend Patterns](frontend-patterns.md)
