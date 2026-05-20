# bystartup-web-qa

Autonomous E2E testing skill for AI coding agents using Playwright.

`bystartup-web-qa` is a web-focused QA automation skill designed to allow AI agents such as Claude Code, Gemini CLI and similar coding assistants to analyze frontend codebases, generate Playwright E2E tests, execute them automatically and iteratively fix failures.

This project was created during the internal innovation initiative promoted by Bystartup.

---

# About Bystartup Labs

This project was developed as part of the **Bystartup Labs**, an internal weekly initiative created to encourage collaboration, experimentation, technical evolution and knowledge sharing among company members.

The Labs environment is focused on:
- emerging technologies
- AI workflows
- engineering experimentation
- developer tooling
- collaborative problem solving
- rapid prototyping

`bystartup-web-qa` was built as an experimental initiative to explore autonomous QA generation using AI agents.

---

# Features

- Automatic E2E test generation
- Playwright integration
- Existing test analysis and incremental updates
- Automatic test execution
- Automatic retry and failure correction
- Authentication flow support
- Session persistence
- Mobile and responsive testing
- Semantic locator prioritization
- Accessibility-oriented selectors
- Loading, error and success state coverage
- API interaction validation
- Multi-framework support

---

# Supported Technologies

The skill is focused exclusively on web applications.

Supported environments include:
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
- Native mobile apps
- Desktop applications

---

# Stack

Main testing library:

```bash
npm i -D @playwright/test
```

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

Example:

```txt
testes/login.e2e.test.ts
testes/dashboard.e2e.test.ts
testes/checkout.e2e.test.ts
```

---

# Authentication Support

The skill automatically detects:
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

The skill generates responsive scenarios for:
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

# Objective

The main goal of this project is to transform AI coding agents into autonomous QA operators capable of generating and validating modern web E2E tests with minimal manual intervention.

---

# Status

Experimental internal tooling developed during Bystartup Labs.
