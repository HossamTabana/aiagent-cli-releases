# Release Instructions for AI Agent CLI

This document provides step-by-step instructions for releasing a new version of AI Agent CLI to GitHub and PyPI.

## Prerequisites

Before starting, ensure you have:
- Git installed and configured
- GitHub CLI (`gh`) installed (optional, for automated release creation)
- Access to push to the GitHub repository
- PyPI account credentials configured (for PyPI releases)

## Release Checklist

### Step 1: Determine the New Version Number

Follow [Semantic Versioning](https://semver.org/):
- **PATCH version** (0.1.X) - Bug fixes and minor changes
- **MINOR version** (0.X.0) - New features, backward compatible
- **MAJOR version** (X.0.0) - Breaking changes

Example: If current version is `0.1.2`, next version could be:
- `0.1.3` for a patch/fix
- `0.2.0` for new features
- `1.0.0` for major changes

**New Version:** `_____________` (Fill this in)

---

### Step 2: Update CHANGELOG.md

Edit `CHANGELOG.md` and add a new section at the top (after the header):

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- New feature 1
- New feature 2

### Changed
- Modified behavior 1
- Updated dependency 2

### Fixed
- Bug fix 1
- Bug fix 2

### Installation

Install directly from PyPI:

```bash
pip install aiagent-2025
```

Or install via uvx from GitHub releases:

```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/vX.Y.Z/aiagent_cli-X.Y.Z-py3-none-any.whl
```

---
```

**Also update the Release Assets section at the bottom:**

```markdown
## Release Assets

### vX.Y.Z

**PyPI Package:** [aiagent-2025](https://pypi.org/project/aiagent-2025/)

**Installation:**
```bash
pip install aiagent-2025
```

**Wheel file:** `aiagent_cli-X.Y.Z-py3-none-any.whl`

**GitHub Installation:**
```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/vX.Y.Z/aiagent_cli-X.Y.Z-py3-none-any.whl
```

---

### v0.1.2
[Keep previous versions below...]
```

**Command:**
```bash
# Open in your editor
nano CHANGELOG.md
# or
code CHANGELOG.md
```

---

### Step 3: Update README.md

Update version references in `README.md`:

**1. Update "Install Latest Version" section:**
```markdown
**Option 2: Install from GitHub Releases**

```bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/vX.Y.Z/aiagent_cli-X.Y.Z-py3-none-any.whl
```
```

**2. Update "Verify Installation" section:**
```markdown
You should see: `aiagent, version X.Y.Z`
```

**3. Update "Troubleshooting" section:**
```markdown
# Reinstall via uvx
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/vX.Y.Z/aiagent_cli-X.Y.Z-py3-none-any.whl
```

**Command:**
```bash
# Use find and replace in your editor
# Find: v0.1.2
# Replace: vX.Y.Z
# Find: 0.1.2
# Replace: X.Y.Z
```

---

### Step 4: Update version in pyproject.toml (if applicable)

If you have a `pyproject.toml` file, update the version there:

```toml
[project]
name = "aiagent-cli"
version = "X.Y.Z"
```

**Command:**
```bash
nano pyproject.toml
```

---

### Step 5: Build the Package

Build the wheel file for distribution:

```bash
# Using uv
uv build

# Or using standard Python tools
python -m build
```

This creates:
- `dist/aiagent_cli-X.Y.Z-py3-none-any.whl`
- `dist/aiagent_cli-X.Y.Z.tar.gz`

---

### Step 6: Test the Package Locally

Before releasing, test the package:

```bash
# Install in a virtual environment
uvx install dist/aiagent_cli-X.Y.Z-py3-none-any.whl

# Verify version
aiagent --version

# Test basic commands
aiagent --help
```

---

### Step 7: Commit Changes

Commit all the updated files:

```bash
# Check what files changed
git status

# Add all modified files
git add CHANGELOG.md README.md pyproject.toml

# Commit with descriptive message
git commit -m "Release vX.Y.Z - [Brief description of changes]"
```

Example commit message:
```bash
git commit -m "Release v0.1.3 - Bug fixes and performance improvements"
```

---

### Step 8: Push Changes to GitHub

Push your commit to the main branch:

```bash
git push origin main
```

**Verify:** Check GitHub to ensure your changes are visible.

---

### Step 9: Create and Push Git Tag

Create an annotated tag for the release:

```bash
# Create annotated tag
git tag -a vX.Y.Z -m "Release vX.Y.Z - [Brief description]"

# Push the tag to GitHub
git push origin vX.Y.Z
```

Example:
```bash
git tag -a v0.1.3 -m "Release v0.1.3 - Bug fixes and performance improvements"
git push origin v0.1.3
```

**Verify:** Check GitHub tags page to ensure the tag exists.

---

### Step 10: Create GitHub Release

You have two options to create the GitHub release:

#### Option A: Using GitHub CLI (Recommended)

```bash
gh release create vX.Y.Z \
  --title "vX.Y.Z - [Release Title]" \
  --notes "## What's New

- Feature 1
- Feature 2
- Bug fix 1

## Installation

Install directly from PyPI:
\`\`\`bash
pip install aiagent-2025
\`\`\`

Or install from GitHub:
\`\`\`bash
uvx install https://github.com/HossamTabana/aiagent-cli-releases/releases/download/vX.Y.Z/aiagent_cli-X.Y.Z-py3-none-any.whl
\`\`\`

See [CHANGELOG.md](CHANGELOG.md) for full details." \
  dist/aiagent_cli-X.Y.Z-py3-none-any.whl
```

Example:
```bash
gh release create v0.1.3 \
  --title "v0.1.3 - Bug Fixes and Performance" \
  --notes "## What's New

- Fixed authentication timeout issue
- Improved caching performance
- Updated dependencies

See [CHANGELOG.md](CHANGELOG.md) for full details." \
  dist/aiagent_cli-0.1.3-py3-none-any.whl
```

#### Option B: Using GitHub Web Interface

1. Go to: `https://github.com/HossamTabana/aiagent-cli-releases/releases`
2. Click **"Draft a new release"**
3. Click **"Choose a tag"** and select `vX.Y.Z`
4. Set **Release title:** `vX.Y.Z - [Release Title]`
5. In the description box, add:
   ```markdown
   ## What's New

   - Feature 1
   - Feature 2
   - Bug fix 1

   ## Installation

   Install directly from PyPI:
   ```bash
   pip install aiagent-2025
   ```

   See [CHANGELOG.md](CHANGELOG.md) for full details.
   ```
6. Click **"Attach binaries"** and upload `dist/aiagent_cli-X.Y.Z-py3-none-any.whl`
7. Ensure **"Set as the latest release"** is checked
8. Click **"Publish release"**

**Verify:** Check the releases page to ensure the release is published and marked as "Latest"

---

### Step 11: Publish to PyPI

Upload the package to PyPI:

```bash
# Using twine (install if needed: pip install twine)
twine upload dist/aiagent_cli-X.Y.Z-py3-none-any.whl dist/aiagent_cli-X.Y.Z.tar.gz

# Or using uv
uv publish
```

You'll be prompted for your PyPI credentials:
- Username: `__token__`
- Password: `[Your PyPI API token]`

**Verify:** Check https://pypi.org/project/aiagent-2025/ to ensure the new version is live.

---

### Step 12: Test PyPI Installation

Test that users can install from PyPI:

```bash
# In a fresh environment
pip install --upgrade aiagent-2025

# Verify version
aiagent --version
# Should show: aiagent, version X.Y.Z
```

---

### Step 13: Announce the Release (Optional)

Consider announcing the release:
- Update project documentation
- Notify users via email/social media
- Post in relevant communities
- Update any integration guides

---

## Quick Reference Commands

Here's a quick summary of all commands for copy-paste:

```bash
# 1. Update files (manually edit CHANGELOG.md, README.md, pyproject.toml)

# 2. Build package
uv build

# 3. Test locally
uvx install dist/aiagent_cli-X.Y.Z-py3-none-any.whl
aiagent --version

# 4. Commit changes
git add CHANGELOG.md README.md pyproject.toml
git commit -m "Release vX.Y.Z - [description]"

# 5. Push to GitHub
git push origin main

# 6. Create and push tag
git tag -a vX.Y.Z -m "Release vX.Y.Z - [description]"
git push origin vX.Y.Z

# 7. Create GitHub release
gh release create vX.Y.Z \
  --title "vX.Y.Z - [Title]" \
  --notes "[Release notes]" \
  dist/aiagent_cli-X.Y.Z-py3-none-any.whl

# 8. Publish to PyPI
uv publish
# or
twine upload dist/*

# 9. Verify installation
pip install --upgrade aiagent-2025
aiagent --version
```

---

## Troubleshooting

### Tag Already Exists

If you need to update a tag:
```bash
# Delete local tag
git tag -d vX.Y.Z

# Delete remote tag
git push origin :refs/tags/vX.Y.Z

# Recreate and push
git tag -a vX.Y.Z -m "Release vX.Y.Z - [description]"
git push origin vX.Y.Z
```

### Release Already Exists on GitHub

Delete the release from GitHub web interface, then recreate it.

### PyPI Upload Fails

- Ensure version number is higher than current version
- Check PyPI token is valid
- Verify package builds correctly: `twine check dist/*`

### Version Mismatch

Always ensure:
- `pyproject.toml` version matches
- `CHANGELOG.md` version matches
- `README.md` references are updated
- Git tag matches
- GitHub release matches

---

## Post-Release Checklist

- [ ] GitHub tag created and pushed
- [ ] GitHub release published and marked as "Latest"
- [ ] Wheel file attached to GitHub release
- [ ] PyPI package published
- [ ] PyPI shows correct version
- [ ] Installation from PyPI works (`pip install aiagent-2025`)
- [ ] Installation from GitHub works (uvx)
- [ ] `aiagent --version` shows correct version
- [ ] CHANGELOG.md updated
- [ ] README.md updated with new version

---

**Developed by Hossam Tabana**

Last updated: 2025-11-16
