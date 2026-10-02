# Release Checklist

## Before
- [ ] `main` is green; all PRs for the release merged.
- [ ] Version bumped per SemVer; `CHANGELOG.md` updated.
- [ ] Database migrations are backward compatible (expand, then contract in a later release).
- [ ] Dependency and security scans show no unaddressed High/Critical.
- [ ] Release notes written for users, including breaking changes and upgrade steps.
- [ ] Rollback plan confirmed (previous artifact available; migrations reversible or safe).

## During
- [ ] Tag the release (`vX.Y.Z`) from the exact commit CI built.
- [ ] Deploy to staging; run smoke tests.
- [ ] Deploy to production progressively (canary or percentage rollout).
- [ ] Watch error rate, latency, and key business metrics against the baseline.

## After
- [ ] Publish release notes.
- [ ] Remove feature flags that are fully rolled out.
- [ ] Close the milestone; move unfinished items to the next one.
- [ ] If anything went wrong, schedule a postmortem.
