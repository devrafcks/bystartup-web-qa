name: bystartup-web-qa

description: |
  Automated web E2E testing engineering skill using Playwright.
  Analyzes web interface files, identifies user flows, visual states,
  forms, API calls, authentication, responsiveness and navigation,
  automatically generating, executing and fixing end-to-end tests.

lib-dependency: "npm i -D @playwright/test"

triggers:
  - "use the bystartup-web-qa skill to generate tests for this file"
  - "use the bystartup-web-qa skill to test this flow"
  - "generate e2e tests for this component"
  - "generate playwright tests for this file"
  - "create end-to-end tests for this feature"
  - "analyze this component and generate e2e tests"

---

# Objective

Receive one or more web interface files and generate intelligent end-to-end (E2E) tests using Playwright.

The skill must:

- Read and interpret source code
- Identify user flows
- Detect visual and conditional states
- Understand forms and validations
- Detect API calls
- Detect authentication and sessions
- Detect responsive behavior
- Check existing tests
- Create, update or replace tests automatically
- Execute generated tests
- Automatically fix failures
- Enforce modern QA and accessibility best practices

---

# Activation

The skill must only run through explicit user commands.

Never activate automatically.

Example:

```txt
use the bystartup-web-qa skill to generate tests for this file
```

---

# Step 1 — Validate Playwright Environment

Check whether `@playwright/test` exists in `devDependencies`.

If missing:

```bash
npm i -D @playwright/test
```

Check for:

```txt
playwright.config.ts
```

If missing, suggest:

```ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './testes',

  use: {
    baseURL: 'http://localhost:3000',
    headless: true,
    trace: 'on-first-retry',
  },
});
```

---

# Step 2 — Test Structure

Search for:

- `/testes`
- `/tests`
- `/src/testes`
- `/src/tests`

If none exist:
- create `/testes` at the project root

Generated files must follow:

```txt
testes/[feature].e2e.test.ts
```

Examples:

| Source | Generated Test |
|---|---|
| LoginForm.tsx | testes/login.e2e.test.ts |
| Checkout.vue | testes/checkout.e2e.test.ts |
| Dashboard.tsx | testes/dashboard.e2e.test.ts |

---

# Step 3 — Existing Test Analysis

Before generating code:

| Situation | Action |
|---|---|
| Test file does not exist | Create from scratch |
| Test file is outdated or broken | Fully overwrite |
| Test file is partially valid | Preserve useful scenarios and expand coverage |

The skill should reuse valid coverage whenever possible.

---

# Step 4 — Analyze Source Code

The skill must automatically map:

## Interactive Elements
- buttons
- links
- inputs
- textareas
- selects
- checkboxes
- radios
- switches

## Forms
- required fields
- validations
- error messages
- masks
- invalid states

## Conditional Flows
- loading
- error
- success
- empty states
- conditional rendering
- skeletons
- modals
- drawers

## APIs

Detect:
- fetch
- axios
- graphql
- useQuery
- useMutation
- SWR
- React Query

Cover:
- success
- failure
- loading
- retry
- empty states

## Navigation

Detect:
- router.push
- navigate()
- Link
- href
- redirects

## State Management

Detect:
- conditional props
- state hooks
- reducers
- contexts
- global stores

---

# Authentication Strategy

When authentication or protected routes are detected, the skill must implement session persistence.

Detect:
- JWT
- cookies
- localStorage auth
- sessionStorage auth
- NextAuth
- Clerk
- Firebase Auth
- Auth0
- middleware
- auth guards

---

## Required Global Setup

Generate:

```txt
playwright/.auth/user.json
```

and:

```txt
testes/setup/auth.setup.ts
```

---

## Session Persistence

Example:

```ts
import { test as setup } from '@playwright/test';

setup('authenticate user', async ({ page }) => {
  await page.goto('/login');

  await page.getByLabel(/e-mail/i).fill('test@test.com');
  await page.getByLabel(/password/i).fill('123456');

  await page.getByRole('button', {
    name: /login/i,
  }).click();

  await page.context().storageState({
    path: 'playwright/.auth/user.json',
  });
});
```

---

## Authentication Reuse

For protected tests:

```ts
test.use({
  storageState: 'playwright/.auth/user.json',
});
```

---

## Authentication Rules

Never repeat login in every test.

Use shared setup whenever possible.

Cover:
- authorized access
- denied access
- expired sessions
- logout
- refresh token
- redirects

---

# Responsive and Mobile Strategy

When detecting:
- media queries
- Tailwind breakpoints
- responsive hidden/block states
- drawers
- mobile menus
- collapsed navigation
- viewport rendering
- matchMedia
- adaptive components

The skill must generate dedicated mobile tests.

---

## Required Breakpoints

Cover:
- mobile
- tablet
- desktop

---

## Playwright Strategy

Use official Playwright devices:

```ts
import { devices } from '@playwright/test';

test.use({
  ...devices['iPhone 13'],
});
```

Or custom viewport:

```ts
test.use({
  viewport: {
    width: 390,
    height: 844,
  },
});
```

---

## Required Mobile Scenarios

Cover:
- hamburger menus
- drawers
- overlays
- mobile modals
- unintended horizontal scrolling
- conditional rendering
- hidden elements
- touch interactions
- adaptive components

Example:

```ts
test.describe('Mobile Navbar', () => {
  test.use({
    ...devices['iPhone 13'],
  });

  test('should open mobile menu', async ({ page }) => {
    await page.goto('/');

    await page.getByRole('button', {
      name: /menu/i,
    }).click();

    await expect(
      page.getByRole('navigation')
    ).toBeVisible();
  });
});
```

---

# Locator Strategy

Required priority order:

## 1. getByRole
```ts
page.getByRole('button', { name: /login/i })
```

## 2. getByLabel
```ts
page.getByLabel('Password')
```

## 3. getByPlaceholder
```ts
page.getByPlaceholder('Enter your e-mail')
```

## 4. getByText
```ts
page.getByText('Registration completed successfully!')
```

## 5. getByTestId
```ts
page.getByTestId('submit-button')
```

---

## Last Resort

Only if necessary:
- CSS selectors
- XPath

Never use CSS/XPath if a semantic alternative exists.

---

# Assertion Strategy

Every `test()` must contain at least one explicit `expect()`.

Always use asynchronous assertions:

```ts
await expect(page).toHaveURL('/dashboard');

await expect(
  page.getByText(/success/i)
).toBeVisible();
```

Never use fragile synchronous assertions.

---

# Required Test Structure

Use:
- `test.describe`
- `test.beforeEach`
- fully isolated tests

Example:

```ts
import { test, expect } from '@playwright/test';

test.describe('E2E Flow — Login', () => {

  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('should login successfully', async ({ page }) => {

    await page.getByLabel('E-mail')
      .fill('user@test.com');

    await page.getByLabel('Password')
      .fill('123456');

    await page.getByRole('button', {
      name: /login/i,
    }).click();

    await expect(page)
      .toHaveURL('/dashboard');
  });

});
```

---

# APIs and Async States

Prioritize real visual validation.

Avoid unnecessary mocks.

Cover:
- loading
- error
- retry
- timeout
- success
- empty states

When necessary, use:

```ts
page.route()
```

Example:

```ts
await page.route('**/api/products', route =>
  route.fulfill({
    status: 500,
    body: 'Internal Server Error',
  })
);
```

---

# Automatic Test Execution

After generating or updating tests, the skill must automatically execute the related Playwright suite.

Preferably execute:

```bash
npx playwright test
```

Or execute only the related file:

```bash
npx playwright test testes/[feature].e2e.test.ts
```

---

# Automatic Validation Strategy

After executing tests:

## If tests pass
- finalize normally
- report success
- summarize generated coverage

## If tests fail

The skill must:

1. analyze the returned error
2. identify:
   - invalid selector
   - timeout
   - missing element
   - incorrect flow
   - broken authentication
   - responsive issue
   - asynchronous state issue
3. automatically fix the issue
4. execute tests again

---

# Stabilization Loop

The skill must repeat the cycle:

1. generate or fix
2. execute
3. validate

until:
- tests pass
or
- maximum retry limit is reached

---

# Retry Limit

Recommended maximum:
- 3 automatic retries per file

After reaching the limit:
- report remaining failures
- explain probable causes
- suggest manual fixes

---

# Required Final Report

After completion, report:

- created files
- updated files
- generated tests count
- passed tests count
- remaining failures
- identified coverage
- authenticated flows covered
- mobile scenarios covered

---

# Required Scenarios

Every identified feature must include:

- primary scenario
- alternative scenario
- error scenario
- visual validation
- explicit assertions

---

# Forbidden Rules

Never use:
- `page.waitForTimeout()`
- interdependent tests
- unnecessary CSS/XPath selectors
- tests without assertions
- happy-path only
- repeated login in every test

---

# Required Checklist

Before finalizing:

- [ ] All identified features have coverage
- [ ] At least one alternative scenario exists
- [ ] At least one error scenario exists
- [ ] All tests contain assertions
- [ ] No fragile selectors were unnecessarily used
- [ ] File follows `.e2e.test.ts`
- [ ] `describe` contains descriptive naming
- [ ] APIs have coverage
- [ ] Loading states have coverage
- [ ] Navigation was validated
- [ ] Authenticated flows were covered
- [ ] Session persistence was implemented
- [ ] Responsive components have mobile coverage
- [ ] Breakpoints were considered
- [ ] Tests were executed
- [ ] Failures were automatically fixed when possible
