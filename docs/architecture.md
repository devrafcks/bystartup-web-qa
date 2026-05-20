# Architecture

`bystartup-web-qa` was designed as an autonomous QA skill focused on web E2E testing using Playwright.

The skill operates through four main stages:

1. Source code analysis
2. Test generation
3. Test execution
4. Automatic retry and correction

The workflow is designed to reduce manual QA effort while maintaining semantic, accessible and resilient E2E coverage.

---

# Main Responsibilities

- Analyze frontend files
- Detect user flows and UI states
- Generate Playwright E2E tests
- Detect existing tests
- Expand or replace outdated coverage
- Execute tests automatically
- Retry failed scenarios
- Validate responsive behavior
- Handle authenticated flows

---

# Supported Flows

- Authentication
- Dashboards
- CRUD interfaces
- Multi-step forms
- Responsive navigation
- Async states
- Protected routes
- API-driven interfaces

---

# Testing Strategy

The skill prioritizes:
- semantic selectors
- accessibility-first locators
- isolated tests
- realistic user behavior
- automatic execution validation
