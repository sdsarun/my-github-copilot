---
applyTo: '**/e2e/**,**/*.spec.ts,**/*.spec.js,**/playwright/**,**/tests/e2e/**'
---

# E2E Testing Standards

These rules apply when editing Playwright E2E test files.

## Structure — Page Object Model (POM)

Every major page or flow gets a Page Object class. Test files use POM classes — they do not use raw `page.locator()` directly:

```typescript
// pages/loginPage.ts
export class LoginPage {
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly submitButton: Locator;

  constructor(private page: Page) {
    this.emailInput = page.locator('[data-testid="email-input"]');
    this.passwordInput = page.locator('[data-testid="password-input"]');
    this.submitButton = page.locator('[data-testid="submit-btn"]');
  }

  async goto() {
    await this.page.goto('/login');
    await this.page.waitForLoadState('networkidle');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }
}
```

## Selector Priority

Use in this order:

1. `data-testid` attributes — `page.locator('[data-testid="submit"]')`
2. ARIA roles — `page.getByRole('button', { name: 'Submit' })`
3. Text — `page.getByText('Submit')`
4. CSS — last resort; never use generated class names or positional selectors

## Waiting — Never Use Fixed Timeouts

```typescript
// WRONG — brittle, fails under load
await page.waitForTimeout(2000);

// CORRECT — wait for the actual thing
await page.waitForLoadState('networkidle');
await page.waitForResponse(resp => resp.url().includes('/api/data'));
await expect(page.locator('[data-testid="results"]')).toBeVisible();
```

## Test Independence

Every test must:

- Set up its own data — do not rely on data from another test
- Clean up after itself, or use isolated test database/user
- Pass in any order
- Pass in isolation (`npx playwright test --grep "test name"`)

## What to Test in E2E

Focus on critical user flows only — unit and integration tests cover the rest:

- Login / logout / registration
- The primary "happy path" of each major feature
- Checkout / payment flows
- Permission boundaries (what a non-admin cannot do)

Do NOT write E2E tests for:

- Individual UI component rendering — use unit tests
- API response format — use integration tests
- All edge cases — those belong in unit/integration tests

## Error Handling in Tests

```typescript
// Test that error states are visible to users
test('shows error when payment fails', async ({ page }) => {
  await page.route('**/api/payment', route => route.fulfill({ status: 402, body: JSON.stringify({ error: 'Card declined' }) }));

  const checkoutPage = new CheckoutPage(page);
  await checkoutPage.goto();
  await checkoutPage.submitPayment();

  await expect(checkoutPage.errorBanner).toBeVisible();
  await expect(checkoutPage.errorBanner).toContainText('Card declined');
});
```
