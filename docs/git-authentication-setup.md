# Git Authentication Setup for ChaseNoCapDev

## Problem
The git credential store was caching old credentials for `ChaseNoCap` account, causing 403 errors when pushing to `ChaseNoCapDev` repositories.

## Solution
Configure git to use GitHub CLI (`gh`) as the credential helper. This ensures git uses the currently authenticated `gh` account (ChaseNoCapDev) for all git operations.

## Configuration Change

### Before
```bash
git config --global credential.helper
# Output: store
```

### After
```bash
git config --global credential.helper
# Output: !gh auth git-credential
```

## Commands Applied

```bash
# Clear existing credential helper
git config --global credential.helper ""

# Add gh as the credential helper
git config --global --add credential.helper '!gh auth git-credential'
```

## How It Works

1. When git needs credentials for GitHub, it calls `gh auth git-credential`
2. GitHub CLI provides credentials for the currently authenticated account
3. No need to manually manage tokens or cached credentials
4. Credentials stay in sync with `gh auth login`

## Verification

```bash
# Check which GitHub account is active
gh auth status

# Should show:
# ✓ Logged in to github.com account ChaseNoCapDev
```

## Troubleshooting

### If pushes still fail with 403:
1. Check `gh auth status` - ensure ChaseNoCapDev is active
2. Re-authenticate: `gh auth login`
3. Clear git credential cache: `git credential reject` (then enter host)

### To switch accounts:
```bash
# Logout current account
gh auth logout

# Login with different account
gh auth login
```

## References
- [GitHub CLI Manual - git-credential](https://cli.github.com/manual/gh_auth_git-credential)
- Date Applied: 2025-11-02
- Applied By: Claude Code
