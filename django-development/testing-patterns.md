# Django Testing Patterns

## Test Checklist

- [ ] Unit tests grouped under `class TestFunctionName(TestCase):`
- [ ] Use Django's test fixtures and factories for consistent data
- [ ] Use strong assertions: `assertEqual(x, 1)` not `assertGreaterEqual(x, 1)`
- [ ] Test edge cases, realistic input, unexpected input, boundaries
- [ ] Do NOT test conditions caught by Python's type system
- [ ] Use `TestCase` for database tests, `SimpleTestCase` for non-database

## Test Structure

```python
from django.test import TestCase

class TestFeatureName(TestCase):
    def setUp(self):
        """Setup test fixtures."""
        self.user = User.objects.create_user(
            email='test@example.com',
            password='testpass123'
        )

    def test_specific_behavior(self):
        """Test description that matches the assertion."""
        # Arrange
        expected = Decimal('0.2500')

        # Act
        result = calculate_something(self.user)

        # Assert
        self.assertEqual(result, expected)
```

## Common Patterns

### Testing Models

```python
class TestUserModel(TestCase):
    def test_user_creation(self):
        user = User.objects.create_user(
            email='test@example.com',
            password='testpass123'
        )
        self.assertEqual(user.email, 'test@example.com')
        self.assertTrue(user.check_password('testpass123'))
```

### Testing Serializers

```python
class TestUserSerializer(TestCase):
    def test_serializer_fields(self):
        user = User.objects.create_user(email='test@example.com', password='pass')
        serializer = UserSerializer(instance=user)

        self.assertIn('email', serializer.data)
        self.assertEqual(serializer.data['email'], 'test@example.com')
```

### Testing Views

```python
from django.test import Client

class TestUserView(TestCase):
    def setUp(self):
        self.client = Client()
        self.user = User.objects.create_user(
            email='test@example.com',
            password='testpass123'
        )

    def test_authenticated_access(self):
        self.client.login(email='test@example.com', password='testpass123')
        response = self.client.get('/api/users/')
        self.assertEqual(response.status_code, 200)
```

### Testing with Mocks

```python
from unittest.mock import patch, MagicMock

class TestExternalService(TestCase):
    @patch('myapp.services.external_api')
    def test_service_call(self, mock_api):
        mock_api.return_value = {'status': 'success'}

        result = my_service_function()

        mock_api.assert_called_once()
        self.assertEqual(result['status'], 'success')
```

## Database Testing Best Practices

1. Use `select_related()` and `prefetch_related()` assertions
2. Test transaction rollback behavior
3. Verify migration compatibility
4. Test with realistic data volumes for performance
