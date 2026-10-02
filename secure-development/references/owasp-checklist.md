# Security Review Checklist (OWASP Top 10, 2021)

Work through the sections the change touches. Mark each item pass, fail, or not applicable.

## A01 Broken Access Control
- [ ] Every endpoint enforces authorization server-side, deny by default.
- [ ] Object-level checks: user can only read or modify records they own or are granted (no IDOR via changed IDs).
- [ ] Admin and internal routes are not reachable by normal users.
- [ ] CORS allows only known origins; no `*` with credentials.
- [ ] Directory listing is off; file paths from input cannot escape the intended directory.

## A02 Cryptographic Failures
- [ ] TLS everywhere; HSTS set on web apps.
- [ ] Sensitive data encrypted at rest.
- [ ] Passwords hashed with argon2id, scrypt, or bcrypt; no MD5/SHA1 for anything security-relevant.
- [ ] Random tokens come from a CSPRNG.
- [ ] No hard-coded keys.

## A03 Injection
- [ ] SQL/NoSQL queries parameterized.
- [ ] No shell strings built from input; no `eval` on input.
- [ ] Templates auto-escape; any raw-HTML use is justified and sanitized.
- [ ] LLM output validated before use in queries, commands, paths, or HTML.

## A04 Insecure Design
- [ ] Threat model exists for new features touching sensitive data or money.
- [ ] Rate limits on expensive or abusable operations.
- [ ] Business-logic abuse considered (negative quantities, replayed requests, race conditions).

## A05 Security Misconfiguration
- [ ] Debug mode, default accounts, and sample apps disabled in production.
- [ ] Security headers set: `Content-Security-Policy`, `X-Content-Type-Options`, `Referrer-Policy`, frame protections.
- [ ] Error responses don't leak stack traces or versions.
- [ ] Cloud storage buckets are private unless intentionally public.

## A06 Vulnerable and Outdated Components
- [ ] Lockfile committed; dependency scan passes with no unaddressed High/Critical.
- [ ] New dependencies are maintained and correctly named.

## A07 Identification and Authentication Failures
- [ ] Login, reset, and OTP endpoints rate-limited.
- [ ] Session IDs rotate on login; sessions expire; logout invalidates server-side.
- [ ] Password reset tokens are single-use and short-lived.

## A08 Software and Data Integrity Failures
- [ ] No unsafe deserialization of untrusted data (pickle, Java native, YAML `load`).
- [ ] CI/CD actions and images pinned; build pipeline can't be modified by untrusted PRs.
- [ ] Updates and downloaded artifacts are verified (checksum or signature).

## A09 Security Logging and Monitoring Failures
- [ ] Auth failures, access-control denials, and admin actions are logged.
- [ ] Logs exclude secrets and sensitive personal data.
- [ ] Alerts exist for anomalous activity.

## A10 Server-Side Request Forgery
- [ ] Server-side fetches of user-supplied URLs use an allowlist of hosts and schemes.
- [ ] Requests to internal ranges and cloud metadata endpoints (169.254.169.254) are blocked.
