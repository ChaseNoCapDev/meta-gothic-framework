# NPM Authentication Setup for GitHub Packages

## Overview

The meta-gothic-framework packages are published to GitHub Packages under the `@chasenocap` scope. To install dependencies from GitHub Packages, you need to authenticate npm with a GitHub Personal Access Token (PAT).

## Quick Setup

### For All Packages at Once

From the framework root directory:

```bash
# Option 1: Using environment variable (recommended)
export GITHUB_TOKEN="your_github_pat_here"
for pkg in packages/*; do
  if [ -f "$pkg/.npmrc.template" ]; then
    envsubst < "$pkg/.npmrc.template" > "$pkg/.npmrc"
  fi
done

# Option 2: Using sed (if envsubst not available)
TOKEN="your_github_pat_here"
for pkg in packages/*; do
  if [ -f "$pkg/.npmrc.template" ]; then
    sed "s/\${GITHUB_TOKEN}/$TOKEN/" "$pkg/.npmrc.template" > "$pkg/.npmrc"
  fi
done
```

### For Individual Packages

```bash
cd packages/package-name

# Copy template
cp .npmrc.template .npmrc

# Replace token placeholder with your actual token
# On macOS:
sed -i '' 's/${GITHUB_TOKEN}/your_token_here/' .npmrc

# On Linux:
sed -i 's/${GITHUB_TOKEN}/your_token_here/' .npmrc
```

## Creating a GitHub Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a descriptive name (e.g., "npm-github-packages")
4. Select the following scopes:
   - ✅ `read:packages` - Required for installing packages
   - ✅ `write:packages` - Required if publishing packages
   - ✅ `repo` - Required for private repositories
5. Click "Generate token"
6. **Copy the token immediately** - you won't be able to see it again!

## .npmrc File Format

The `.npmrc` file should look like this:

```
@chasenocap:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=ghp_YourActualTokenHere
```

## Security Best Practices

### ✅ DO:
- Keep your `.npmrc` files local only (they're gitignored)
- Use `.npmrc.template` for version control
- Rotate your tokens periodically
- Use different tokens for different environments
- Set token expiration dates

### ❌ DON'T:
- Never commit `.npmrc` files with actual tokens
- Don't share tokens between team members
- Don't use tokens with more permissions than needed
- Don't commit tokens to git (GitHub will block pushes)

## Troubleshooting

### Error: 401 Unauthorized

Your token is missing or invalid. Check:
1. Token exists in `.npmrc`
2. Token has `read:packages` scope
3. Token hasn't expired
4. Token belongs to account with package access

### Error: 404 Not Found

The package doesn't exist or you don't have access. Check:
1. Package name is correct (@chasenocap/package-name)
2. Your GitHub account has access to ChaseNoCapDev org
3. Package has been published

### Error: ENOENT .npmrc not found

You haven't created `.npmrc` from template yet:
```bash
cp .npmrc.template .npmrc
# Edit .npmrc and add your token
```

## CI/CD Setup

For automated builds:

### GitHub Actions
```yaml
steps:
  - name: Setup npm authentication
    run: |
      echo "@chasenocap:registry=https://npm.pkg.github.com" > .npmrc
      echo "//npm.pkg.github.com/:_authToken=${{ secrets.GITHUB_TOKEN }}" >> .npmrc

  - name: Install dependencies
    run: npm install
```

### Other CI Systems
Set `GITHUB_TOKEN` as environment variable or secret, then:
```bash
envsubst < .npmrc.template > .npmrc
npm install
```

## Alternative: Global .npmrc

You can configure authentication globally instead of per-package:

```bash
# Edit global config
npm config set @chasenocap:registry https://npm.pkg.github.com
npm config set //npm.pkg.github.com/:_authToken your_token_here

# Or manually edit ~/.npmrc
echo "@chasenocap:registry=https://npm.pkg.github.com" >> ~/.npmrc
echo "//npm.pkg.github.com/:_authToken=your_token_here" >> ~/.npmrc
```

**Note:** Global configuration affects all npm projects on your machine.

## Verification

Test your setup:

```bash
# Should succeed if authentication is correct
npm install @chasenocap/logger

# Should show package info
npm view @chasenocap/logger
```

## Related Documentation

- [GitHub Packages Documentation](https://docs.github.com/en/packages)
- [npm Configuration](https://docs.npmjs.com/cli/v9/configuring-npm/npmrc)
- [Git Authentication Setup](./git-authentication-setup.md)
