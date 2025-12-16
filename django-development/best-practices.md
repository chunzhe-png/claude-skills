# Django Best Practices Reference

## Writing Functions Best Practices

When evaluating whether a function is good, use this checklist:

1. Can you read the function and HONESTLY easily follow what it's doing? If yes, stop here.
2. Does the function have very high cyclomatic complexity? (nested if-else as proxy)
3. Are there common data structures/algorithms that would improve it? (parsers, trees, stacks)
4. Are there unused parameters?
5. Are there unnecessary type casts that can move to arguments?
6. Is the function easily testable without mocking Django core features?
7. Does it have hidden untested dependencies?
8. Brainstorm 3 better function names - is current name best and consistent?

**IMPORTANT**: Do NOT refactor out a separate function unless:
- The refactored function is used in more than one place
- The refactored function is easily unit testable while original is not
- The original function is extremely hard to follow

## Writing Tests Best Practices

1. **SHOULD** parameterize inputs; never embed unexplained literals
2. **SHOULD NOT** add tests that can't fail for real defects
3. **SHOULD** ensure test description matches what the assert verifies
4. **SHOULD** compare to independent expectations, not function output reused as oracle
5. **SHOULD** follow same lint/style rules as production code
6. **SHOULD** express invariants/axioms rather than single hard-coded cases

### Test Example

```python
from django.test import TestCase
from decimal import Decimal
from emissions.services.matcher import CalculatorService

class EmissionCalculationTest(TestCase):
    def test_calculate_emission_precision_with_kg_units(self):
        """Test that emission calculation maintains decimal precision when converting kg to tonnes"""
        calculator = CalculatorService()

        emission_factor = Decimal('2.5000')  # kg CO2e per unit
        client_quantity = Decimal('100.0000')
        target_unit = "tonne"
        co2_data_unit = "kg"

        result = calculator.calculate_emission(target_unit, co2_data_unit, client_quantity, emission_factor)

        expected_result = Decimal('0.2500')  # 2.5 * 100 / 1000
        self.assertEqual(result, expected_result)
```

## Code Organization

### Project Structure

- `accounts/` - User authentication, company management, permissions
- `api/` - REST API endpoints and serializers
- `biodiversity/` - Biodiversity tracking and reporting
- `config/` - Django settings and configuration
- `emissions/` - Core emissions tracking, calculation, reporting
- `financed/` - Financed emissions tracking
- `frameworks/` - Reporting frameworks (GRI, TCFD, etc.)
- `reports/` - Report generation and management

## Quick Commands (Shortcuts)

### QNEW
When user types "qnew": Understand all best practices and ensure code follows them.

### QPLAN
When user types "qplan": Analyze codebase and verify plan is consistent, minimal, reuses patterns.

### QCODE
When user types "qcode": Implement plan, run tests, run `ruff check`, run `mypy`.

### QCHECK
When user types "qcheck": As skeptical senior engineer, review against all checklists.

### QCHECKF
When user types "qcheckf": Review functions against Writing Functions checklist.

### QCHECKT
When user types "qcheckt": Review tests against Writing Tests checklist.

### QUX
When user types "qux": Output comprehensive UX test scenarios sorted by priority.

### QGIT
When user types "qgit": Add, commit, push with Conventional Commits format.
