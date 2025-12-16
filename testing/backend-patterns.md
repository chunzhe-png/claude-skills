# Backend Testing Patterns (pytest-django)

## Project Structure

```
project-root/
├── pantas-green/           # Django app (mounted as /code in Docker)
│   └── test/               # COPIED from ../test for Docker access
└── test/                   # Source test files (single source of truth)
    └── backend/            # Backend tests (pytest)
```

**Key**: Test files are **copied** from `test/` to `pantas-green/test/` because symlinks don't work with Docker mounts.

## Built-in Fixtures

```python
@pytest.mark.django_db
class TestExample:
    def test_user(self, django_user_model):
        # django_user_model = get_user_model()
        user = django_user_model.objects.create_user(...)

    def test_settings(self, settings):
        settings.DEBUG = True
```

## Custom Fixtures

```python
@pytest.mark.django_db
class TestFeature:
    @pytest.fixture
    def user(self, django_user_model):
        """Create a standard test user."""
        user = django_user_model.objects.create_user(
            email='test@example.com',
            password='testpass123',
            first_name='Test',
            last_name='User',
        )
        user.is_active = True
        user.save()
        return user

    @pytest.fixture
    def admin_user(self, django_user_model):
        """Create an admin user."""
        user = django_user_model.objects.create_user(
            email='admin@example.com',
            password='adminpass123',
        )
        user.is_staff = True
        user.is_superuser = True
        user.save()
        return user

    def test_something(self, user, admin_user):
        # Both fixtures available as parameters
        pass
```

## Mocking Django Requests

**WRONG:**
```python
request = MagicMock()
request.session = {}  # Plain dict - will fail!
```

**CORRECT:**
```python
from unittest.mock import MagicMock

@pytest.fixture
def mock_request(self):
    request = MagicMock()
    request.session = MagicMock()
    request.session.session_key = 'test-session-key'
    request.META = {'REMOTE_ADDR': '127.0.0.1'}
    return request
```

## Testing Django Signals

```python
from django.contrib.auth.signals import user_logged_in

@pytest.mark.django_db
class TestLoginSignal:
    @pytest.fixture
    def mock_request(self):
        request = MagicMock()
        request.session = MagicMock()
        request.session.session_key = 'test-session-key'
        request.META = {'REMOTE_ADDR': '127.0.0.1'}
        return request

    def test_signal_handler(self, user, mock_request):
        user_logged_in.send(
            sender=user.__class__,
            request=mock_request,
            user=user
        )
        user.refresh_from_db()
        # Assert expected behavior
```

## Testing Serializers

```python
@pytest.mark.django_db
class TestUserSerializer:
    def test_serializer_fields(self, user):
        from api.v1.app.serializers.accounts.user import UserSerializer

        serializer = UserSerializer(instance=user)
        data = serializer.data

        assert 'email' in data
        assert data['email'] == 'test@example.com'
```

## Testing with Database Updates

```python
@pytest.mark.django_db
class TestDatabaseOperations:
    def test_update_field(self, user, django_user_model):
        # Update via queryset (doesn't update instance in memory)
        django_user_model.objects.filter(pk=user.pk).update(
            last_login=timezone.now()
        )

        # MUST refresh to see changes
        user.refresh_from_db()

        assert user.last_login is not None
```

## Testing API Endpoints

```python
from rest_framework.test import APIClient

@pytest.mark.django_db
class TestAPI:
    @pytest.fixture
    def api_client(self):
        return APIClient()

    @pytest.fixture
    def authenticated_client(self, api_client, admin_user):
        api_client.force_authenticate(user=admin_user)
        return api_client

    def test_api_endpoint(self, authenticated_client):
        response = authenticated_client.get('/api/v1/users/')
        assert response.status_code == 200
```

## Backend Test Template

```python
"""
test_feature_name.py - Tests for [Feature Name]

To run: ./run_tests.sh backend/test_feature_name.py
"""

import pytest
from unittest.mock import patch, MagicMock
from django.utils import timezone


@pytest.mark.django_db
class TestFeatureName:
    """Tests for [Feature Name]."""

    @pytest.fixture
    def user(self, django_user_model):
        """Create a test user."""
        user = django_user_model.objects.create_user(
            email='test@example.com',
            password='testpass123',
        )
        user.is_active = True
        user.save()
        return user

    @pytest.fixture
    def mock_request(self):
        """Create a mock request with proper session."""
        request = MagicMock()
        request.session = MagicMock()
        request.session.session_key = 'test-session-key'
        request.META = {'REMOTE_ADDR': '127.0.0.1'}
        return request

    def test_example(self, user, mock_request):
        """Test description."""
        # Arrange
        # Act
        # Assert
        assert user.email == 'test@example.com'
```

## Troubleshooting

### Database Connection Issues
```bash
cd pantas-green
docker compose exec -T postgres psql -U postgres -c "SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'test_postgres';"
docker compose exec -T postgres psql -U postgres -c "DROP DATABASE IF EXISTS test_postgres;"
./run_tests.sh backend --fresh-db
```

### AttributeError: 'dict' object has no attribute 'session_key'
Use `MagicMock()` for `request.session`, not a plain dict.

## Backend Checklist

- [ ] Use `@pytest.mark.django_db` class decorator
- [ ] Use `@pytest.fixture` instead of `setUp()`
- [ ] Use `assert` statements instead of `self.assertX()`
- [ ] Use `django_user_model` fixture for creating users
- [ ] Mock `request.session` with `MagicMock()` including `session_key`
- [ ] Call `refresh_from_db()` after queryset updates
