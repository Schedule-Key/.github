# Schedule-Key Organization Defaults

This repository contains organization-wide defaults for all Schedule-Key repos.

## Workflow Templates

### Gitleaks Secret Scanning

Every repo in the Schedule-Key org uses [Gitleaks](https://github.com/gitleaks/gitleaks) to scan for hardcoded secrets (API keys, passwords, tokens).

**How it works:**
- Runs on every **pull request** and **push** to main branches (main, master, beta, development, dev)
- Downloads the official gitleaks binary from [gitleaks/gitleaks releases](https://github.com/gitleaks/gitleaks/releases)
- PRs: scans only the commits in the PR
- Pushes: scans the latest commit
- If secrets are found, the check **fails** and you get an email notification

### Setting Up a New Repo

1. **Copy the workflow file** from `workflow-templates/gitleaks.yml` to your new repo at `.github/workflows/gitleaks.yml`

   Or use the GitHub UI: go to your repo > Actions > "New workflow" > find "Gitleaks Secret Scanning" in the org templates.

2. **Push to all branches** that need protection (master, beta, development, etc.)

3. **That is it.** No license key, no secrets, no configuration needed.

### Resolving Findings

If gitleaks catches something:

| Finding | Fix |
|---------|-----|
| Real secret | Rotate immediately, remove from code, use environment variables |
| False positive (one line) | Add `# gitleaks:allow` comment on the line |
| False positive (specific finding) | Add the fingerprint to `.gitleaksignore` in the repo root |
| False positive (pattern) | Add an allowlist rule to `.gitleaks.toml` in the repo root |

### Local Pre-Commit Hook

All developers should install gitleaks locally for pre-commit scanning:

```bash
brew install gitleaks
```

The global pre-commit hook at `~/.git-hooks/pre-commit` automatically runs `gitleaks protect --staged` before every commit.

### Full Documentation

Each repo has detailed docs at `docs/SECURITY_SCANNING.md`.

---

## Organization Profile

The public-facing org profile is in `profile/README.md`.
