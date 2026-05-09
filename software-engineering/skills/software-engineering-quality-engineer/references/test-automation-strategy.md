---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[quality-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Test Automation Strategy & Frameworks (2025)

## Test Pyramid

```
          /\
         /  \  E2E (10-15%)
        /----\
       /      \  Integration (20-30%)
      /--------\
     /          \ Unit (55-70%)
    /____________\
```

**Unit tests**: Fast, isolated, catch logic errors early
**Integration tests**: Verify service interactions, API contracts
**E2E tests**: User workflows, critical paths, visual correctness

## Framework Selection

### Unit Testing
**Jest + React Testing Library**
```javascript
import { render, screen } from '@testing-library/react';

test('renders submit button', () => {
  render(<LoginForm />);
  expect(screen.getByRole('button', { name: /submit/i })).toBeInTheDocument();
});
```

### Integration Testing
**Jest + API mocking (MSW)**
```javascript
import { server } from './mocks/server';

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());

test('loads users from API', async () => {
  render(<UserList />);
  expect(await screen.findByText('User 1')).toBeInTheDocument();
});
```

### E2E Testing
**Playwright** (recommended 2025)
```javascript
test('checkout flow', async ({ page }) => {
  await page.goto('https://example.com/shop');
  await page.click('button:has-text("Add to cart")');
  await page.goto('/checkout');
  await page.fill('[name="email"]', 'user@example.com');
  await page.click('text=Complete Purchase');
  await expect(page).toHaveURL('/order-confirmation');
});
```

## Visual Regression Testing

**Storybook + Chromatic**
Automatically catch unintended UI changes.

```javascript
// In Storybook story
export const Primary = {
  args: { variant: 'primary' },
};
```

Chromatic takes screenshots, compares to baseline, flags changes.

## Accessibility Testing

**axe-core in Playwright**
```javascript
import { injectAxe, checkA11y } from 'axe-playwright';

test('page is accessible', async ({ page }) => {
  await page.goto('https://example.com');
  await injectAxe(page);
  await checkA11y(page);
});
```

## Content/CMS Testing

**Contentful API Contract Testing**
```javascript
test('validates content model fields', async () => {
  const response = await contentful.getEntries({ content_type: 'page' });
  expect(response.items[0].fields).toHaveProperty('title');
  expect(response.items[0].fields).toHaveProperty('slug');
  expect(response.items[0].fields.slug).toMatch(/^[a-z0-9-]+$/);
});
```

## CI/CD Integration

```yaml
# GitHub Actions example
name: Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm install
      - run: npm run test:unit  # Unit tests
      - run: npm run test:integration  # Integration
      - run: npm run test:e2e  # E2E (full chain)

      - name: Report coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info

      - name: Accessibility check
        run: npm run test:a11y
```

Quality gates: Tests must pass before merge.

## Test Data Management

**Factory Pattern** (for consistent test data)
```javascript
const userFactory = {
  build: () => ({
    id: crypto.randomUUID(),
    email: 'test@example.com',
    name: 'Test User'
  })
};

test('user profile', () => {
  const user = userFactory.build();
  render(<Profile user={user} />);
});
```

**Avoid**: Hard-coded data, shared state between tests, brittle test data.

## 2025 Best Practices

- **Test user flows, not implementation**: React Testing Library principle
- **Keep tests maintainable**: Brittle tests are worse than no tests
- **Automate the right things**: Automate repetitive regression; manual test for exploratory/UX
- **Measure coverage**: Track code and feature coverage; aim for 80%+
- **Fail fast**: Tests should run <10 minutes; optimize slow tests
