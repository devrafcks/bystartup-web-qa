# bystartup-web-qa

Autonomous web E2E testing skill for AI coding agents using Playwright.

`bystartup-web-qa` is a skill designed for AI agents such as Claude Code, Gemini CLI and similar coding assistants to automatically generate, execute and maintain Playwright E2E tests for web applications.

The skill analyzes frontend files, identifies flows and interactive elements, creates complete E2E coverage and automatically validates generated tests.

---

# Features

- Automatic Playwright E2E test generation
- Automatic test execution
- Automatic retry and failure correction
- Existing test analysis and incremental updates
- Authentication and session persistence support
- Responsive and mobile test generation
- API flow validation
- Accessibility-oriented selectors
- Semantic locator prioritization
- Error, loading and success state coverage
- Automatic test folder creation
- Multi-framework support

---

# Supported Technologies

Focused exclusively on web applications.

Supported:
- React
- Next.js
- Vue
- Angular
- SPA applications
- SSR applications
- Traditional web interfaces

Not supported:
- React Native
- Flutter
- Native mobile applications
- Desktop applications

---

# Main Stack

Install Playwright:

```bash
npm i -D @playwright/test
```

---

# What the Skill Does

The skill is capable of:

- Reading frontend source files
- Identifying buttons, forms, navigation and UI states
- Detecting API interactions
- Detecting authentication flows
- Generating complete E2E Playwright tests
- Creating the testing structure automatically
- Running generated tests automatically
- Detecting failures and retrying fixes
- Updating existing tests without losing valid coverage
- Generating responsive scenarios for mobile, tablet and desktop

---

# Generated Structure

The skill automatically detects or creates testing folders:

```txt
/testes
/tests
/src/testes
/src/tests
```

Generated file pattern:

```txt
testes/[feature].e2e.test.ts
```

Examples:

```txt
testes/login.e2e.test.ts
testes/dashboard.e2e.test.ts
testes/checkout.e2e.test.ts
```

---

# Authentication Support

Automatically detects:
- JWT authentication
- cookies
- localStorage auth
- sessionStorage auth
- NextAuth
- Clerk
- Firebase Auth
- Auth0
- protected routes

Generated auth structure:

```txt
playwright/.auth/user.json
testes/setup/auth.setup.ts
```

---

# Responsive Testing

Automatically generates responsive scenarios for:
- mobile
- tablet
- desktop

Including:
- hamburger menus
- drawers
- overlays
- responsive navigation
- adaptive layouts

---

# Automatic Execution Flow

After generating tests, the skill automatically:

1. Executes Playwright
2. Detects failures
3. Attempts automatic fixes
4. Re-runs the suite
5. Repeats until stabilization or retry limit

---

# Example Usage

```txt
Use the bystartup-web-qa skill to generate complete Playwright E2E tests for the LoginForm.tsx component.
```

```txt
Use the bystartup-web-qa skill to analyze this dashboard page and generate full E2E coverage.
```

```txt
Use the bystartup-web-qa skill to create responsive Playwright tests for the checkout flow.
```

```txt
Use the bystartup-web-qa skill to generate, execute and validate E2E tests for this authentication flow.
```

---

# Objective

Transform AI coding agents into autonomous E2E QA operators capable of generating, executing and maintaining modern Playwright web tests with minimal manual intervention.

---

# Internal Context

This project was developed during the Bystartup Labs internal initiative focused on experimentation, collaboration and AI engineering workflows.
