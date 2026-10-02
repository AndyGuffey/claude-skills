# Lightweight Threat Model (STRIDE)

Aim for 30 minutes. Save the result as `docs/threat-model/<feature>.md` in the project.

## 1. What are we building?
- One-paragraph description of the feature.
- Data flow: list components (client, API, database, queues, third parties, LLMs) and the arrows between them. Mark each arrow that crosses a trust boundary.
- Assets: what an attacker would want (accounts, personal data, money, API keys, compute).

## 2. What can go wrong?
For each trust-boundary crossing, ask the six STRIDE questions:

| Threat | Question | Typical control |
|--------|----------|-----------------|
| **S**poofing | Can someone pretend to be another user or service? | Strong auth, mTLS, signed tokens |
| **T**ampering | Can data be modified in transit or at rest? | TLS, integrity checks, server-side validation |
| **R**epudiation | Can someone deny doing something? | Audit logs with actor and timestamp |
| **I**nformation disclosure | Can data leak to the wrong party? | Authorization, encryption, minimal responses |
| **D**enial of service | Can someone exhaust resources? | Rate limits, quotas, timeouts, size limits |
| **E**levation of privilege | Can someone gain rights they shouldn't have? | Least privilege, deny by default, object checks |

For LLM or agent features, also ask: can untrusted content the model reads cause it to call a tool, leak data, or act outside the user's intent?

## 3. What are we doing about it?

| # | Threat | Likelihood (L/M/H) | Impact (L/M/H) | Mitigation | Owner | Status |
|---|--------|-------------------|----------------|------------|-------|--------|
| 1 |        |                   |                |            |       |        |

## 4. Did we do a good job?
- Every High-impact threat has a mitigation or an explicit, signed-off acceptance.
- Mitigations have tests or monitoring.
- Revisit when the data flow changes.
