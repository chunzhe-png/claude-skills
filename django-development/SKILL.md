---
name: django-development
version: 1.0.0
description: |
  Django development best practices, coding standards, and project patterns for Pantas Green.
  Use when: writing Django models, views, serializers, migrations, ORM queries, or any Python backend code.
  Triggers: django, model, view, serializer, migration, ORM, queryset, endpoint, API, DRF, emission, carbon, python backend
---

# Django Development Guidelines

## Quick Reference

| Rule | Description |
|------|-------------|
| **TDD** | Scaffold stub → write failing test → implement |
| **ORM** | Use `select_related()` and `prefetch_related()` to avoid N+1 |
| **Decimals** | Use `DecimalField` for financial/emissions, never `FloatField` |
| **Business Logic** | Keep in models and services, not views |
| **Type Hints** | Not required in this project |

## Before Coding

- **BP-1 (MUST)** Ask clarifying questions
- **BP-2 (SHOULD)** Draft and confirm approach for complex work
- **BP-3 (SHOULD)** List pros/cons if multiple approaches exist

## While Coding

- **C-1 (MUST)** Follow TDD: scaffold stub → write failing test → implement
- **C-2 (MUST)** Use existing domain vocabulary for naming
- **C-3 (SHOULD)** Use Django's class-based views and model methods
- **C-4 (SHOULD)** Prefer simple, composable, testable functions
- **C-5 (MUST)** No type hints required
- **C-6 (MUST)** Follow Django import conventions and PEP 8
- **C-7 (SHOULD NOT)** Add comments except for critical caveats
- **C-8 (SHOULD)** Use Django built-ins before custom solutions
- **C-9 (SHOULD NOT)** Extract functions unless reused or improves testability

## Database Rules

- **D-1 (MUST)** Avoid N+1 queries with `select_related()` / `prefetch_related()`
- **D-2 (SHOULD)** Use transactions for atomic operations
- **D-3 (MUST)** Create migrations for model changes
- **D-4 (SHOULD)** Use `Decimal` fields for financial/emissions calculations

## Testing Rules

- **T-1 (MUST)** Place tests in `tests/` directory within each app
- **T-2 (MUST)** Add/extend tests for any API change
- **T-3 (MUST)** Separate unit tests from integration tests
- **T-4 (SHOULD)** Use `TestCase` for DB tests, `SimpleTestCase` for pure logic
- **T-5 (SHOULD)** Unit-test complex business logic thoroughly

## Tooling Gates

- **G-1 (MUST)** `ruff check` passes
- **G-2 (MUST)** `mypy` passes
- **G-3 (MUST)** Django tests pass with `./manage.py test`

## Detailed Guides

For comprehensive guidelines, see:
- [Best Practices Reference](best-practices.md)
- [Testing Patterns](testing-patterns.md)
