# MONAI AMD Fork Setup Design

**Date:** 2026-04-07  
**Author:** AMD Team  
**Status:** Approved for Implementation

## Overview

This document outlines the strategy for creating and managing an AMD-optimized fork of Project MONAI at `ROCm-LS/MONAI`. The fork will provide visibility for AMD/ROCm users while maintaining an internal development workflow that ensures compliance and quality control before public releases.

## Goals

1. **Visibility** - Create a proper GitHub fork that appears in Project-MONAI/MONAI's fork network, making it discoverable to users
2. **Discoverability** - AMD/ROCm users can easily find the optimized fork when visiting the upstream MONAI project
3. **Controlled Release** - Maintain internal development with compliance review before public releases
4. **Sustainable Workflow** - Document repeatable processes for ongoing fork maintenance

## Non-Goals

- Contributing AMD changes back to upstream MONAI (future consideration, not part of current plan)
- Maintaining constant sync with upstream dev branch (only sync at release time with stable branches)
- Replacing the existing ROCm-LS/monai repository (it remains untouched)

## Architecture

### Repository Structure

```
Project-MONAI/MONAI (upstream)
         ↓
    (fork relationship)
         ↓
ROCm-LS/MONAI (public AMD fork)
         ↑
    (releases from)
         ↑
Internal AMD Repository (private)
         ↑
    (rebased on)
         ↑
Project-MONAI/MONAI stable branch
```

### Branch Strategy

**ROCm-LS/MONAI branches:**
- `dev` - main development branch (matches upstream convention)
- `amd-release-vX.Y.Z` - release branches for each AMD release
- Additional feature branches as needed

**Tags:**
- Format: `vX.Y.Z-rocm` where X.Y.Z is the upstream MONAI version
- Examples: `v1.3.0-rocm`, `v1.4.0-rocm`

## Components

### 1. Fork Creation

**Process:**
1. Navigate to https://github.com/Project-MONAI/MONAI
2. Click "Fork" button
3. Select `ROCm-LS` organization as owner
4. Name the repository `MONAI` (capital letters to match upstream)
5. Fork only the default branch initially (clean start)

**Initial Setup:**
```bash
# Clone the new fork
git clone https://github.com/ROCm-LS/MONAI.git
cd MONAI

# Add upstream remote
git remote add upstream https://github.com/Project-MONAI/MONAI.git

# Fetch stable branch
git fetch upstream
git checkout -b dev upstream/dev

# Push to fork
git push origin dev
```

### 2. Repository Configuration

**GitHub Settings:**
- **Description:** "AMD ROCm optimized fork of MONAI"
- **Topics/Tags:** `amd`, `rocm`, `monai`, `medical-imaging`, `deep-learning`, `pytorch`
- **Issues:** Enabled (for AMD-specific bug reports)
- **Wiki:** Disabled (unless needed later)
- **Discussions:** Optional (consider enabling for community engagement)

**Branch Protection:**
- Protect `dev` branch from direct pushes
- Require pull request reviews for merges
- Require status checks to pass before merging (if CI is set up)

### 3. Documentation Updates

**README.md additions:**
```markdown
# MONAI - AMD ROCm Fork

This is AMD's optimized fork of [Project MONAI](https://github.com/Project-MONAI/MONAI) 
with ROCm GPU support and performance optimizations for AMD hardware.

## AMD-Specific Features
- ROCm/HIP compatibility for CUDA extensions
- AMD GPU optimizations for medical imaging workloads
- Tested and validated on AMD Instinct GPUs

## ROCm Compatibility

| MONAI Fork Version | Based on MONAI | Supported ROCm Versions | Tested GPUs |
|--------------------|----------------|-------------------------|-------------|
| v1.3.0-rocm        | v1.3.0         | ROCm 5.7, 6.0, 6.1      | MI210, MI250|

*Table will be updated with each release*

## Installation

### Prerequisites
- AMD GPU with ROCm support
- ROCm 5.7+ installed
- Python 3.8+

### Install from source
```bash
git clone https://github.com/ROCm-LS/MONAI.git
cd MONAI
pip install -e .
```

## Upstream Sync

This fork is rebased on MONAI stable releases. See [CHANGELOG.md](CHANGELOG.md) 
for the base version and AMD-specific changes.

## Reporting Issues

- **AMD/ROCm specific issues:** Report here in ROCm-LS/MONAI Issues
- **General MONAI issues:** Report to [upstream MONAI](https://github.com/Project-MONAI/MONAI/issues)

## Contributing

AMD-specific contributions are welcome. For general MONAI improvements, 
please contribute to the [upstream project](https://github.com/Project-MONAI/MONAI).
```

**CHANGELOG.md additions:**
```markdown
# AMD ROCm Fork Changelog

## v1.3.0-rocm (YYYY-MM-DD)

**Base Version:** MONAI v1.3.0 stable

**AMD Enhancements:**
- Initial ROCm fork release
- Added ROCm/HIP compatibility layer
- [List specific AMD optimizations]

**ROCm Support:**
- Tested with ROCm 5.7, 6.0, 6.1
- Validated on MI210, MI250 GPUs
```

### 4. Issue Management Workflow

**When users report issues on ROCm-LS/MONAI:**

**Step 1: Triage & Classification**
- Issue submitted on ROCm-LS/MONAI GitHub Issues
- Maintainer reviews and labels:
  - `amd-specific` - ROCm compatibility, AMD GPU bugs
  - `rocm` - ROCm platform issues
  - `upstream-candidate` - General MONAI bugs discovered via fork
  - `documentation` - Docs improvements
  - `question` - User questions

**Step 2: Fix Location Decision**

**AMD-Specific Issues:**
1. Fix in internal repository first
2. Follow internal dev → testing → compliance review cycle
3. Include fix in next scheduled release
4. Update public issue: "Fix committed to internal dev, will be in vX.Y.Z-rocm release (estimated QX 20XX)"
5. Close issue when released publicly

**Upstream-Relevant Issues:**
1. Report to upstream Project-MONAI/MONAI
2. If urgent for AMD users: apply temporary fix in internal repo
3. Track upstream progress
4. Remove temporary patch when upstream fix is released
5. Document upstream issue link in fork's issue

**Step 3: Communication**
- Keep GitHub issue open and updated with progress
- Reference internal tracking ID if appropriate (without exposing internal details)
- Provide transparency on timeline
- Close when fix is publicly available

**Issue Templates:**

Create `.github/ISSUE_TEMPLATE/bug_report.md`:
```markdown
---
name: Bug Report (AMD/ROCm)
about: Report AMD or ROCm specific issues
---

**Describe the bug**
A clear description of what the bug is.

**Environment:**
- ROCm Version: [e.g., 6.0]
- GPU: [e.g., MI250]
- MONAI Fork Version: [e.g., v1.3.0-rocm]
- Python Version: [e.g., 3.10]

**To Reproduce**
Steps to reproduce the behavior...

**Is this issue AMD/ROCm specific?**
- [ ] Yes, only happens with AMD GPUs/ROCm
- [ ] No, might be a general MONAI issue
- [ ] Unsure

**Additional context**
Add any other context about the problem.
```

Create `.github/ISSUE_TEMPLATE/upstream_redirect.md`:
```markdown
---
name: General MONAI Issue
about: For issues not specific to AMD/ROCm
---

**This looks like a general MONAI issue**

If this issue is not specific to AMD GPUs or ROCm, please report it to 
the upstream MONAI project:

https://github.com/Project-MONAI/MONAI/issues

AMD-specific issues (ROCm compatibility, AMD GPU bugs, etc.) should 
be reported here.
```

## Release Workflow

### Pre-Release Phase (Internal Development)

**Step 1: Sync with Upstream Stable**
```bash
# In internal repository
git fetch upstream
git checkout -b sync-stable-v1.4.0 upstream/stable-v1.4.0

# Rebase AMD development branch onto new stable
git checkout amd-dev
git rebase sync-stable-v1.4.0
# Resolve conflicts if any

# Test thoroughly
```

**Step 2: Prepare Release Branch**
```bash
# Create release branch in internal repo
git checkout -b amd-release-v1.4.0

# Cherry-pick or merge AMD-specific commits ready for public release
# Exclude any internal-only code, experimental features, etc.

# Update CHANGELOG.md with AMD changes
# Update README.md ROCm compatibility table
git commit -m "Prepare v1.4.0-rocm release"
```

**Step 3: Internal Validation**
- Run full test suite on AMD hardware
- Performance benchmarks
- Integration testing with ROCm stack
- Documentation review

**Step 4: Compliance Review** ⚠️
- Submit release branch for licensing review
- Security audit
- IP compliance checks
- Export control review if applicable
- Address any findings
- Obtain formal approval to release publicly

### Public Release Phase

**Step 5: Push to Public Fork**
```bash
# Add public fork as remote (one-time setup)
git remote add public-fork https://github.com/ROCm-LS/MONAI.git

# Push release branch
git push public-fork amd-release-v1.4.0

# Create release tag
git tag -a v1.4.0-rocm -m "AMD ROCm optimized release based on MONAI v1.4.0"
git push public-fork v1.4.0-rocm
```

**Step 6: Update Main Branch**
```bash
# Merge to dev branch
git checkout dev
git merge amd-release-v1.4.0
git push public-fork dev
```

**Step 7: Create GitHub Release**
- Go to https://github.com/ROCm-LS/MONAI/releases
- Click "Draft a new release"
- Select tag `v1.4.0-rocm`
- Title: "MONAI v1.4.0 - AMD ROCm Optimized"
- Release notes:
  - Base MONAI version
  - AMD-specific features/optimizations
  - ROCm versions supported
  - Tested hardware
  - Known issues
  - Installation instructions

## Ongoing Maintenance

### Upstream Sync Cadence

- **When:** Only when preparing a new AMD release
- **What:** Rebase on latest upstream stable branch (not dev)
- **Frequency:** Determined by AMD internal milestones (quarterly, feature-driven, etc.)

**Between releases:**
- Fork does not need constant syncing with upstream
- Monitor upstream for critical security patches
- Assess security patches case-by-case for off-cycle updates

### Documentation Maintenance

**Update with each release:**
- README.md ROCm compatibility table
- CHANGELOG.md with new AMD features
- Any new installation requirements

**Keep current:**
- Links to upstream MONAI documentation
- AMD GPU hardware compatibility list
- Known issues and workarounds

### Issue Tracking

- Review new issues weekly
- Respond to AMD-specific issues within 2-3 business days
- Close resolved issues when fix is released
- Keep upstream-related issues linked to upstream tracker

## Testing Strategy

### Before Each Public Release

**Functional Testing:**
- Run MONAI test suite on AMD GPUs
- Validate all CUDA extensions work with ROCm/HIP
- Test key medical imaging workflows

**Performance Testing:**
- Benchmark against previous release
- Compare with NVIDIA baseline where applicable
- Document any performance regressions

**Compatibility Testing:**
- Test with supported ROCm versions
- Validate on target AMD GPU hardware (MI210, MI250, etc.)
- Test with common PyTorch versions

**Documentation Testing:**
- Verify installation instructions work
- Test code examples in README
- Validate links are not broken

## Success Metrics

- **Visibility:** Fork appears in MONAI network graph within 24 hours of creation
- **Discoverability:** Users can find fork from upstream MONAI repo
- **Adoption:** Track GitHub stars, forks, and issue activity
- **Stability:** Clean release process with no rollbacks
- **Compliance:** 100% of releases pass compliance review before public push

## Rollback Plan

If issues are discovered after public release:

1. **Minor issues:** Fix in next release
2. **Critical bugs:** 
   - Create hotfix branch from release tag
   - Fix in internal repo
   - Fast-track compliance review
   - Release patch version (e.g., v1.4.0-rocm → v1.4.1-rocm)
3. **Security vulnerabilities:**
   - Follow responsible disclosure practices
   - Coordinate with upstream MONAI if applicable
   - Release security patch immediately after compliance

## Future Considerations

- **Upstream Contribution:** Consider contributing AMD optimizations back to Project-MONAI/MONAI
- **CI/CD:** Set up automated testing on AMD hardware
- **Docker Images:** Provide ROCm-based Docker images
- **PyPI Package:** Publish AMD-optimized builds to PyPI as `monai-rocm` or similar
- **ROCm Compatibility:** Expand testing matrix as new ROCm versions release

## Appendix: Key Commands Reference

### Fork Setup
```bash
# Clone fork
git clone https://github.com/ROCm-LS/MONAI.git
cd MONAI

# Add upstream
git remote add upstream https://github.com/Project-MONAI/MONAI.git
git fetch upstream
```

### Release Preparation
```bash
# Sync with upstream stable
git fetch upstream
git checkout -b amd-release-vX.Y.Z upstream/stable-vX.Y.Z

# Add public fork remote (one-time)
git remote add public-fork https://github.com/ROCm-LS/MONAI.git

# Push release
git push public-fork amd-release-vX.Y.Z
git tag -a vX.Y.Z-rocm -m "Release message"
git push public-fork vX.Y.Z-rocm
```

### Maintenance
```bash
# Check fork status
git fetch upstream
git fetch public-fork
git status

# Update fork's dev branch
git checkout dev
git pull public-fork dev
git push public-fork dev
```
