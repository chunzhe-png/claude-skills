# Frontend Testing Patterns (Vitest)

## Project Structure

```
test/
└── frontend/               # Frontend tests (runs on host, not Docker)
    ├── node_modules/       # Dependencies
    ├── package.json        # Vitest configuration
    └── *.test.tsx          # Test files
```

**Key**: Frontend tests run directly from `test/frontend/` on the host (not in Docker).

## Vitest vs Jest

| Feature | Jest | Vitest |
|---------|------|--------|
| Filter by pattern | `--testPathPattern=Pattern` | `vitest run Pattern` (positional) |
| Run once | `--run` | `vitest run` |
| Watch mode | Default | `vitest` (without `run`) |
| Config file | `jest.config.js` | `vitest.config.ts` |

## Running Tests

```bash
# From test/frontend directory
npm test                      # Run all tests
npm test -- UsersColumn       # Filter by pattern

# Via test runner
./run_tests.sh frontend
./run_tests.sh frontend UsersColumn
```

## Package.json Setup

```json
{
  "name": "frontend",
  "scripts": {
    "test": "vitest run"
  },
  "devDependencies": {
    "@tanstack/react-query": "^5.90.12",
    "@testing-library/react": "^16.3.0",
    "vitest": "^3.2.4"
  }
}
```

## Basic Test Structure

```typescript
import { describe, it, expect, vi } from 'vitest';
import { render, screen } from '@testing-library/react';

describe('formatLastLogin', () => {
  it('returns "Never" for null dates', () => {
    expect(formatLastLogin(null)).toBe('Never');
  });

  it('formats dates correctly', () => {
    const date = new Date('2025-01-15T10:30:00Z');
    const result = formatLastLogin(date.toISOString());
    expect(result).toContain('15 Jan 2025');
  });
});
```

## Component Testing

```typescript
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

// Mock dependencies
vi.mock('./api', () => ({
  fetchData: vi.fn(),
}));

describe('ComponentName', () => {
  let queryClient: QueryClient;

  beforeEach(() => {
    queryClient = new QueryClient({
      defaultOptions: {
        queries: { retry: false },
      },
    });
    vi.clearAllMocks();
  });

  const renderComponent = () => {
    return render(
      <QueryClientProvider client={queryClient}>
        <ComponentName />
      </QueryClientProvider>
    );
  };

  it('renders correctly', () => {
    renderComponent();
    expect(screen.getByText('Expected Text')).toBeInTheDocument();
  });

  it('handles user interaction', async () => {
    const user = userEvent.setup();
    renderComponent();

    await user.click(screen.getByRole('button'));

    await waitFor(() => {
      expect(screen.getByText('Updated Text')).toBeInTheDocument();
    });
  });
});
```

## Frontend Test Template

```typescript
/**
 * ComponentName.test.tsx - Tests for [Component Name]
 *
 * To run: ./run_tests.sh frontend ComponentName
 */

import { describe, it, expect, vi, beforeEach } from 'vitest';
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ComponentName } from './ComponentName';

vi.mock('./api', () => ({
  fetchData: vi.fn(),
}));

describe('ComponentName', () => {
  let queryClient: QueryClient;

  beforeEach(() => {
    queryClient = new QueryClient({
      defaultOptions: {
        queries: { retry: false },
      },
    });
    vi.clearAllMocks();
  });

  const renderComponent = () => {
    return render(
      <QueryClientProvider client={queryClient}>
        <ComponentName />
      </QueryClientProvider>
    );
  };

  it('renders correctly', () => {
    renderComponent();
    expect(screen.getByText('Expected Text')).toBeInTheDocument();
  });
});
```

## Troubleshooting

### "vitest: not found"
```bash
cd test/frontend
npm install
npm test
```

### "Unknown option --testPathPattern"
This is Jest syntax. Use positional argument:
```bash
# WRONG (Jest)
npm test -- --testPathPattern=UsersColumn

# CORRECT (Vitest)
npm test -- UsersColumn
```

### Tests not found
Ensure files match `*.test.tsx` or `*.test.ts` pattern in `test/frontend/`.

## Frontend Checklist

- [ ] Use Vitest positional args for filtering, not `--testPathPattern`
- [ ] Run tests from `test/frontend/` directory
- [ ] Install dependencies with `npm install` if `node_modules` missing
- [ ] Use `vi.mock()` for mocking modules
- [ ] Use `vi.fn()` for mock functions
- [ ] Wrap components with necessary providers (QueryClientProvider, etc.)
- [ ] Use `vi.clearAllMocks()` in `beforeEach`
