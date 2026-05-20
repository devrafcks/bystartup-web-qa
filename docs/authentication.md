# Authentication

`bystartup-web-qa` supports authenticated E2E flows using Playwright storage state persistence.

Supported auth strategies:
- JWT
- cookies
- localStorage
- sessionStorage
- NextAuth
- Clerk
- Firebase Auth
- Auth0

---

# Generated Structure

```txt
playwright/.auth/user.json
testes/setup/auth.setup.ts
```

---

# Objective

Avoid repetitive login execution in every test while maintaining isolated and reproducible authenticated sessions.

---

# Flow

1. Execute login once
2. Persist authentication state
3. Reuse session across protected tests
4. Validate redirects and access control
