---
name: secure-development
description: Secure software development practices for designing, writing, and reviewing code. Use when building or changing anything that handles user input, authentication, authorization, sessions, secrets, personal data, file uploads, database queries, external APIs, dependencies, or LLM/agent tool calls, and when asked for a security review, threat model, or OWASP check.
---

# Secure Development

Apply these practices whenever code touches a trust boundary: where data or control crosses from something you don't control (users, networks, files, third-party services, model output) into something you do.

## Workflow

1. **Identify trust boundaries.** Before writing code, list every input source and every privileged action the change touches. For a new feature or service, do a lightweight threat model (see `references/threat-modeling.md`).
2. **Apply the core rules below** while implementing.
3. **Self-review against the checklist** in `references/owasp-checklist.md` for the categories the change touches. Skip categories that clearly don't apply; say which ones you checked.
4. **Add security tests** for the cases that matter: rejected bad input, denied unauthorized access, no secret in logs or responses.
5. **Report findings** using the format at the end of this file. Never silently leave a known issue in place.

## Core rules

### Input and output
- Validate all external input on the server side with an allowlist (type, length, format, range). Client-side validation is UX, not security.
- Use parameterized queries or an ORM for every database call. Never build SQL, shell commands, LDAP, or XPath by string concatenation.
- Avoid shelling out. If unavoidable, pass arguments as an array, never through a shell string.
- Encode output for its context (HTML, attribute, JS, URL). Prefer frameworks that auto-escape; treat any "raw HTML" escape hatch as a review flag.
- Parse untrusted files (XML, YAML, images, archives) with safe settings: no external entities, no arbitrary object deserialization, size and path limits (guard against zip-slip).

### Authentication and sessions
- Don't write your own crypto or password hashing. Use argon2id, scrypt, or bcrypt through a maintained library.
- Use the framework's session management; set cookies `HttpOnly`, `Secure`, `SameSite=Lax` or stricter.
- Rate-limit and lock out login, password reset, and OTP endpoints. Return the same message for "unknown user" and "wrong password."
- Support MFA for privileged accounts where the product allows.

### Authorization
- Deny by default. Check authorization on the server for every request, on the specific object being accessed (prevents IDOR), not just "is logged in."
- Centralize authorization logic rather than scattering checks through handlers.
- Apply least privilege to service accounts, database users, cloud IAM roles, and API tokens.

### Secrets
- Never commit secrets. Load them from environment variables or a secrets manager; keep `.env` files out of git.
- Never log secrets, tokens, passwords, or full payment or personal data. Redact at the logging layer.
- If a secret is exposed, rotate it; deleting the commit is not enough.

### Data protection
- Use TLS for all network traffic. Encrypt sensitive data at rest.
- Collect and retain only the personal data the feature needs.
- Return generic error messages to clients; keep stack traces and internals in server logs.

### Dependencies and supply chain
- Pin versions with a lockfile. Run a dependency scanner (e.g. `npm audit`, `pip-audit`, Dependabot, Snyk, OSV-Scanner) in CI.
- Prefer well-maintained packages; check for typosquatted names before adding one.
- Pin CI actions and container base images to a digest or exact version.

### LLM and agent features
- Treat model output as untrusted input: validate it before it reaches a query, shell, file path, URL fetch, or HTML.
- Assume prompt injection through any content the model reads (web pages, documents, emails, tool results). Don't let such content grant permissions.
- Give agents the narrowest tools and scopes possible; require human confirmation for destructive or external actions.
- Keep secrets out of prompts and out of the model's context.

## Tooling to wire into CI

| Check | Examples |
|-------|----------|
| Secret scanning | gitleaks, trufflehog, GitHub secret scanning |
| SAST | Semgrep, CodeQL, Bandit (Python), ESLint security plugins |
| Dependency (SCA) | Dependabot, OSV-Scanner, npm audit, pip-audit |
| Container / IaC | Trivy, Checkov, tfsec |
| DAST (running app) | OWASP ZAP |

## Reporting findings

For each issue:

```
[SEVERITY: Critical | High | Medium | Low] Short title
Where: path/to/file.ext:line
Issue: what is wrong and how it could be exploited (concrete scenario)
Fix: the specific change, with code if short
```

Rank by severity. Critical and High block merge; Medium and Low get a tracked follow-up if not fixed now.

## References

- `references/owasp-checklist.md`: review checklist organized by OWASP Top 10 (2021) category.
- `references/threat-modeling.md`: a 30-minute STRIDE threat-modeling template.

## Changelog

- 2026-09-28: Initial version.
