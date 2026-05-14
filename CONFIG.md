# Torti Skills Development - Configuration & Deployment

## ClawSweeper Integration

### Repository Profiles
```typescript
// src/repository-profiles.ts config for torti-skills
{
  repoSlug: "torti/torti-skills",
  label: "Torti Skills Repository",
  
  // Issue close criteria
  closeIf: {
    alreadyImplemented: true,
    notReproducible: true,
    bestForHub: true,
    duplicate: true,
    notActionable: true,
    incoherent: true,
    staleOlderThan: 60
  },
  
  // Protect maintainer items & linked PRs
  skipCloseIf: {
    authorIsMaintainer: true,
    hasOpenPR: true,
    hasProtectedLabel: true
  }
}
```

### Scheduled Jobs

**Daily Issue Review** (06:00 UTC)
```bash
openclaw cron add --name "torti:daily-review" \
  --schedule "0 6 * * *" \
  --command "clawsweeper review --since 24h --repo torti/torti-skills"
```

**Weekly Planning** (Monday 09:00 UTC)
```bash
openclaw cron add --name "torti:weekly-planning" \
  --schedule "0 9 * * 1" \
  --command "clawsweeper planning --repo torti/torti-skills"
```

**Commit Review** (On-push)
```bash
openclaw cron add --name "torti:commit-review" \
  --schedule "on-push" \
  --command "clawsweeper commit-reports --since 24h"
```

## Skill Development Checklist

- [ ] Issue created with clear requirements
- [ ] SKILL.md template created
- [ ] Implementation started
- [ ] Tests written & passing
- [ ] Code reviewed by ClawSweeper
- [ ] Merged to main
- [ ] Documented in README

## Token Limits & Monitoring

- Track Nemotron-120B usage (free tier limit unknown)
- Track Gemini-Flash usage (free tier limit unknown)
- Local Gemma-4 unlimited
- ClawSweeper: 10-min timeout per item (watch for API errors)

## GitHub Actions Workflows

- `.github/workflows/review.yml` - Scheduled ClawSweeper reviews
- `.github/workflows/commit-review.yml` - Commit validation
- `.github/workflows/skill-validate.yml` - Skill tests & lint
