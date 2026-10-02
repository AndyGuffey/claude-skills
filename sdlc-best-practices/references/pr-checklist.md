# Pull Request Template and Review Checklist

Save the template as `.github/pull_request_template.md` in a project.

## Template

```markdown
## What and why
<!-- What changes for a user or developer, and why. Link the issue: Closes #123 -->

## How
<!-- Key implementation decisions a reviewer should know -->

## Testing
<!-- How this was verified: tests added, manual steps, screenshots -->

## Risk and rollback
<!-- What could break; how to roll back; feature flag name if any -->

## Checklist
- [ ] Tests added or updated, and passing locally
- [ ] Docs and CHANGELOG updated
- [ ] No secrets, debug code, or commented-out code
- [ ] Security considered (input, auth, data exposure)
- [ ] Breaking changes called out
```

## Reviewer checklist

1. **Correctness:** does it do what the issue asks? Edge cases (empty, null, large, concurrent, failure paths)?
2. **Tests:** do they fail without the change? Do they test behavior, not implementation?
3. **Security:** new inputs validated? Authorization on new endpoints? Secrets or personal data in logs?
4. **Design:** fits existing patterns? Any duplication or unneeded abstraction?
5. **Readability:** clear names; comments explain why, not what.
6. **Operability:** errors logged usefully; metrics or alerts needed?

Label comments so the author knows what blocks: `blocking:`, `suggestion:`, `nit:`, `question:`.
