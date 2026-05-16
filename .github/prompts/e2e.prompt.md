---
mode: agent
description: Generate or fix Playwright E2E tests for critical user flows
---

# E2E Test Generator

Write or fix Playwright end-to-end tests for the described user flow. Follow Page Object Model (POM) structure.

## Test File Organization

```
tests/
├── e2e/
│   ├── auth/
│   │   ├── login.spec.ts
│   │   └── register.spec.ts
│   └── features/
│       ├── [feature].spec.ts
├── fixtures/
│   └── auth.ts
└── playwright.config.ts
```

## Page Object Model (POM) Pattern

Every page or major section gets a Page Object class:

```typescript
import { Page, Locator } from '@playwright/test';

export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly submitButton: Locator;
  readonly errorMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.locator('[data-testid="email-input"]');
    this.passwordInput = page.locator('[data-testid="password-input"]');
    this.submitButton = page.locator('[data-testid="submit-btn"]');
    this.errorMessage = page.locator('[data-testid="error-msg"]');
  }

  async goto() {
    await this.page.goto('/login');
    await this.page.waitForLoadState('networkidle');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
    await this.page.waitForLoadState('networkidle');
  }
}
```

## Test Structure

```typescript
import { test, expect } from '@playwright/test';
import { LoginPage } from '../pages/loginPage';

test.describe('Login flow', () => {
  test('redirects to dashboard on valid credentials', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login('user@example.com', 'password123');
    await expect(page).toHaveURL('/dashboard');
  });

  test('shows error on invalid credentials', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login('user@example.com', 'wrongpassword');
    await expect(loginPage.errorMessage).toBeVisible();
    await expect(loginPage.errorMessage).toContainText('Invalid credentials');
  });
});
```

## Selector Priority

Use in this order (most to least preferred):

1. `data-testid` attributes — `page.locator('[data-testid="submit-btn"]')`
2. ARIA roles — `page.getByRole('button', { name: 'Submit' })`
3. Text content — `page.getByText('Submit')`
4. CSS selectors — only as last resort, never use generated class names

## Anti-Patterns to Avoid

- `page.waitForTimeout(2000)` — brittle; use `waitForLoadState` or `waitForResponse` instead
- Hardcoded user credentials — use fixture files or environment variables
- Long tests that cover many flows — split into focused, independent tests
- Tests that depend on execution order — each test must be self-contained
- Asserting on internal state — test observable behavior only (what the user sees)

## Handling Flaky Tests

If a test is flaky:

1. Add explicit waits for the element or network response
2. Check if the test creates shared state that another test might interfere with
3. Add retry only as a last resort — fix the root cause first

```typescript
// Instead of arbitrary wait
await page.waitForTimeout(1000); // WRONG

// Wait for the specific thing that needs to be ready
await page.waitForResponse(resp => resp.url().includes('/api/data')); // CORRECT
await expect(page.locator('[data-testid="results"]')).toBeVisible(); // CORRECT
```

## Running Tests

```bash
npx playwright test                        # All tests
npx playwright test tests/e2e/auth/        # Specific folder
npx playwright test --headed               # See the browser
npx playwright test --debug                # Debug with inspector
npx playwright show-report                 # View HTML report after run
```
