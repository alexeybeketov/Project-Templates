# Release

Full release: commit, push, version, tag, build, deploy, verify.

## When to use
After `/review` passes. Invoke with `/release`.

## Steps

### 1. Commit + push
```bash
git add [files] && git commit -m "message" && git push origin main
```

### 2. Version bump
```bash
# Patch (X.Y.Z) — bug fixes
# Minor (X.Y.0) — new features
# Major (X.0.0) — breaking changes
echo "X.Y.Z" > VERSION
git add VERSION && git commit -m "Bump version to X.Y.Z" && git push origin main
```

### 3. Tag + release
<!-- Customize for your CI/CD: GitHub Actions, GitLab CI, etc. -->
```bash
git tag -a vX.Y.Z -m "vX.Y.Z — description" && git push origin vX.Y.Z
```

### 4. Wait for build
<!-- Check your CI/CD pipeline -->

### 5. Deploy + verify
<!-- Run your deploy command, then /verify -->

### If build fails
```bash
git tag -d vX.Y.Z && git push origin :refs/tags/vX.Y.Z
# Fix, re-commit, re-tag
```

## Output format
```
## Release vX.Y.Z
- Commit: [hash] [message]
- Build: success/failed
- Deploy: success/failed
- Version: X.Y.Z confirmed
```
