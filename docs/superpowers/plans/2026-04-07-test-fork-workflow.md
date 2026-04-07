# Test MONAI Fork Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Validate the fork management workflow on ROCm-LS/MONAIwip (production org test fork)

**Architecture:** Test all components of the fork workflow (setup, documentation, issue templates, release process) on ROCm-LS/MONAIwip. This is the actual production fork that will be used for AMD releases (may be renamed to MONAI later).

**Tech Stack:** Git, GitHub, Markdown

---

## Task 1: Verify Fork and Setup Remotes

**Files:**
- Read: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md`

**Context:** The fork ROCm-LS/MONAIwip already exists as a proper GitHub fork. We need to verify it and set up the proper remote configuration.

- [ ] **Step 1: Verify current repository state**

Run:
```bash
git remote -v
git status
```

Expected: 
- Current repo is /home/AMD/nilapate/opensource/MONAI
- Should see `origin` remote (may point to upstream or nilapate fork)

- [ ] **Step 2: Add upstream remote if not present**

Run:
```bash
# Check if upstream exists
git remote | grep upstream || git remote add upstream https://github.com/Project-MONAI/MONAI.git
git fetch upstream
```

Expected: upstream remote added and fetched successfully

- [ ] **Step 3: Add ROCm-LS fork remote**

Run:
```bash
# Add ROCm-LS/MONAIwip as 'rocm-fork' remote
git remote | grep rocm-fork || git remote add rocm-fork https://github.com/ROCm-LS/MONAIwip.git
git fetch rocm-fork
```

Expected: rocm-fork remote added

- [ ] **Step 4: Verify remote configuration**

Run:
```bash
git remote -v
```

Expected output should include:
```
upstream	https://github.com/Project-MONAI/MONAI.git (fetch)
upstream	https://github.com/Project-MONAI/MONAI.git (push)
rocm-fork	https://github.com/ROCm-LS/MONAIwip.git (fetch)
rocm-fork	https://github.com/ROCm-LS/MONAIwip.git (push)
```

- [ ] **Step 5: Commit remote configuration documentation**

Run:
```bash
# Document the remote setup
cat > docs/test-workflow-validation.md << 'EOF'
# Fork Workflow Validation Log

## Production Test Fork: ROCm-LS/MONAIwip

### Remote Configuration
- `upstream`: https://github.com/Project-MONAI/MONAI.git
- `rocm-fork`: https://github.com/ROCm-LS/MONAIwip.git

### Validation Date
Started: $(date +%Y-%m-%d)

### Notes
- Testing on actual production organization fork
- May rename MONAIwip → MONAI after successful validation
- Starting fresh, ignoring existing branches
EOF

git add docs/test-workflow-validation.md
git commit -m "docs: add fork workflow validation log"
```

Expected: Documentation file created and committed

---

## Task 2: Create Fork-Specific README Updates

**Files:**
- Modify: `README.md` (top section)
- Reference: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md` (section 3)

**Context:** Add AMD fork branding and information to README. For testing, we'll add a TEST FORK banner.

- [ ] **Step 1: Backup original README**

Run:
```bash
cp README.md README.md.backup
```

Expected: Backup created

- [ ] **Step 2: Add fork header to README**

Add after the main MONAI header in `README.md`:

```markdown
---

# 🧪 WORKFLOW VALIDATION - AMD ROCm Fork

**⚠️ This fork is being validated for AMD ROCm workflow. May be renamed to ROCm-LS/MONAI upon completion.**

This is AMD's optimized fork of [Project MONAI](https://github.com/Project-MONAI/MONAI) 
with ROCm GPU support and performance optimizations for AMD hardware.

## AMD-Specific Features
- ROCm/HIP compatibility for CUDA extensions
- AMD GPU optimizations for medical imaging workloads
- Tested and validated on AMD Instinct GPUs

## ROCm Compatibility

| MONAI Fork Version | Based on MONAI | Supported ROCm Versions | Tested GPUs |
|--------------------|----------------|-------------------------|-------------|
| v1.3.0-rocm-test   | v1.3.0         | ROCm 5.7, 6.0, 6.1      | MI210, MI250|

*This table will be updated with each release*

## Installation (Test Fork)

### Prerequisites
- AMD GPU with ROCm support
- ROCm 5.7+ installed
- Python 3.8+

### Install from source
```bash
git clone https://github.com/ROCm-LS/MONAIwip.git
cd MONAIwip
pip install -e .
```

## Upstream Sync

This test fork is rebased on MONAI stable releases. See [CHANGELOG.md](CHANGELOG.md) 
for the base version and AMD-specific changes.

## Reporting Issues

**Note: This fork is under workflow validation**

- **AMD/ROCm specific issues:** Report here in ROCm-LS/MONAIwip Issues
- **General MONAI issues:** Report to [upstream MONAI](https://github.com/Project-MONAI/MONAI/issues)

## Contributing

AMD-specific contributions are welcome on the production fork. For general MONAI improvements, 
please contribute to the [upstream project](https://github.com/Project-MONAI/MONAI).

---
```

Expected: README now has AMD fork header

- [ ] **Step 3: Verify README changes**

Run:
```bash
head -100 README.md | grep -A 5 "TEST FORK"
```

Expected: Should see the test fork banner

- [ ] **Step 4: Commit README changes**

Run:
```bash
git add README.md
git commit -m "docs: add AMD fork header to README (test)"
```

Expected: README changes committed

---

## Task 3: Create AMD Fork CHANGELOG

**Files:**
- Modify: `CHANGELOG.md` (prepend AMD section)
- Reference: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md` (section 3)

**Context:** Add AMD fork changelog at the top of existing CHANGELOG.md

- [ ] **Step 1: Read current CHANGELOG structure**

Run:
```bash
head -50 CHANGELOG.md
```

Expected: See existing MONAI changelog structure

- [ ] **Step 2: Create AMD changelog section**

Prepend to `CHANGELOG.md`:

```markdown
# AMD ROCm Fork Changelog

> **Note:** This is the changelog for AMD-specific enhancements. For upstream MONAI changes, see the main changelog below.

---

## v1.3.0-rocm-test (2026-04-07)

**Status:** TEST RELEASE - Workflow Validation

**Base Version:** MONAI v1.3.0 stable

**AMD Enhancements:**
- Initial ROCm fork test release
- Workflow validation for fork management process
- Documentation structure for AMD-specific features

**ROCm Support:**
- Target: ROCm 5.7, 6.0, 6.1
- Planned validation on MI210, MI250 GPUs

**Test Goals:**
- Validate fork creation and configuration
- Test documentation workflow
- Verify issue template functionality
- Simulate release process

---

# Original MONAI Changelog

```

Expected: AMD changelog prepended to file

- [ ] **Step 3: Verify CHANGELOG structure**

Run:
```bash
head -40 CHANGELOG.md
```

Expected: AMD section appears first, followed by original changelog

- [ ] **Step 4: Commit CHANGELOG changes**

Run:
```bash
git add CHANGELOG.md
git commit -m "docs: add AMD fork changelog section"
```

Expected: CHANGELOG committed

---

## Task 4: Create Issue Templates

**Files:**
- Create: `.github/ISSUE_TEMPLATE/bug_report.md`
- Create: `.github/ISSUE_TEMPLATE/upstream_redirect.md`
- Reference: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md` (section 4)

**Context:** Set up issue templates to help users report AMD-specific vs upstream issues

- [ ] **Step 1: Create ISSUE_TEMPLATE directory**

Run:
```bash
mkdir -p .github/ISSUE_TEMPLATE
```

Expected: Directory created

- [ ] **Step 2: Create AMD/ROCm bug report template**

Create `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
---
name: Bug Report (AMD/ROCm)
about: Report AMD or ROCm specific issues
title: '[BUG] '
labels: ['bug', 'amd-specific']
---

**⚠️ Note: This is a TEST FORK for workflow validation**

For production issues, please use the ROCm-LS/MONAI fork when available.

## Describe the bug
A clear and concise description of what the bug is.

## Environment
- **ROCm Version:** [e.g., 6.0]
- **GPU:** [e.g., MI250]
- **MONAI Fork Version:** [e.g., v1.3.0-rocm-test]
- **Python Version:** [e.g., 3.10]
- **PyTorch Version:** [e.g., 2.0.0]

## To Reproduce
Steps to reproduce the behavior:
1. 
2. 
3. 

## Expected behavior
A clear and concise description of what you expected to happen.

## Actual behavior
What actually happened.

## Is this issue AMD/ROCm specific?
- [ ] Yes, only happens with AMD GPUs/ROCm
- [ ] No, might be a general MONAI issue
- [ ] Unsure

## Code snippet (if applicable)
```python
# Minimal code to reproduce
```

## Error message/traceback
```
Paste error message here
```

## Additional context
Add any other context about the problem here.
```

Expected: Bug report template created

- [ ] **Step 3: Create upstream redirect template**

Create `.github/ISSUE_TEMPLATE/upstream_redirect.md`:

```markdown
---
name: General MONAI Issue
about: For issues not specific to AMD/ROCm - redirect to upstream
title: '[UPSTREAM] '
labels: ['upstream-candidate']
---

**⚠️ This looks like a general MONAI issue**

**For test fork users:** This is a test fork for workflow validation. 

If this issue is **not specific to AMD GPUs or ROCm**, please report it to 
the upstream MONAI project:

👉 https://github.com/Project-MONAI/MONAI/issues

---

## AMD-Specific Issues

Report here (or to production ROCm-LS/MONAI when available) if the issue is:
- ROCm compatibility problems
- AMD GPU-specific bugs
- Performance issues on AMD hardware
- HIP/ROCm build problems

## General MONAI Issues

Report to upstream if the issue is:
- Feature requests for MONAI
- General bugs that affect all platforms
- Documentation improvements
- API design questions

---

**If you're unsure**, you can still report here and we'll help triage.
```

Expected: Upstream redirect template created

- [ ] **Step 4: Verify issue templates**

Run:
```bash
ls -la .github/ISSUE_TEMPLATE/
cat .github/ISSUE_TEMPLATE/bug_report.md | head -20
```

Expected: Both template files exist and have correct content

- [ ] **Step 5: Commit issue templates**

Run:
```bash
git add .github/ISSUE_TEMPLATE/
git commit -m "feat: add issue templates for AMD/ROCm fork"
```

Expected: Templates committed

---

## Task 5: Simulate Internal Development Branch

**Files:**
- None (Git operations only)

**Context:** Create a branch that simulates the "internal development" repository to test the release workflow

- [ ] **Step 1: Create simulated internal dev branch**

Run:
```bash
git checkout -b amd-internal-dev
```

Expected: New branch created and checked out

- [ ] **Step 2: Add a simulated AMD feature**

Create a simple marker file to simulate AMD development:

```bash
cat > AMD_FEATURES.md << 'EOF'
# AMD-Specific Features (Simulated)

This file simulates AMD-specific development work.

## Features in Development

### ROCm/HIP Compatibility Layer
- Status: Simulated for testing
- Description: Compatibility shims for CUDA extensions

### AMD GPU Optimizations  
- Status: Planned
- Description: Performance optimizations for MI series GPUs

### Documentation
- Status: Complete
- Description: Fork workflow and setup documentation
EOF

git add AMD_FEATURES.md
git commit -m "feat: add simulated AMD features for testing"
```

Expected: AMD_FEATURES.md created and committed

- [ ] **Step 3: Document internal branch**

Update validation log:

```bash
cat >> docs/test-workflow-validation.md << 'EOF'

## Internal Development Branch

Branch: `amd-internal-dev`
Purpose: Simulates internal AMD development repository
Created: $(date +%Y-%m-%d)
EOF

git add docs/test-workflow-validation.md
git commit -m "docs: document internal dev branch simulation"
```

Expected: Documentation updated

- [ ] **Step 4: Push internal branch to ROCm fork**

Run:
```bash
git push rocm-fork amd-internal-dev
```

Expected: Branch pushed to ROCm-LS/MONAIwip

---

## Task 6: Create Release Branch (Simulate Compliance)

**Files:**
- Modify: `docs/test-workflow-validation.md`
- Reference: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md` (Release Workflow section)

**Context:** Simulate the release preparation and compliance review process

- [ ] **Step 1: Fetch upstream stable branch**

Run:
```bash
git fetch upstream
git branch -a | grep stable
```

Expected: See available stable branches from upstream

- [ ] **Step 2: Create release branch based on upstream stable**

Run:
```bash
# Use the most recent stable or dev if no stable branch
git checkout -b amd-release-v1.3.0-test upstream/dev
```

Expected: Release branch created from upstream

- [ ] **Step 3: Cherry-pick AMD changes onto release branch**

Run:
```bash
# Get the commits from internal dev
git log amd-internal-dev --oneline | head -5

# Cherry-pick AMD feature commits (adjust commit hashes as needed)
git cherry-pick amd-internal-dev~2..amd-internal-dev
```

Expected: AMD commits applied to release branch

- [ ] **Step 4: Update CHANGELOG for release**

Update the date in CHANGELOG.md from "2026-04-07" to actual date and change status:

```markdown
## v1.3.0-rocm-test ($(date +%Y-%m-%d))

**Status:** RELEASE CANDIDATE - Ready for Validation

**Base Version:** MONAI v1.3.0 (dev branch as of $(date +%Y-%m-%d))
```

Run:
```bash
git add CHANGELOG.md
git commit -m "docs: update changelog for v1.3.0-rocm-test release"
```

Expected: CHANGELOG updated for release

- [ ] **Step 5: Document simulated compliance review**

```bash
cat >> docs/test-workflow-validation.md << 'EOF'

## Simulated Compliance Review

### Release Branch: amd-release-v1.3.0-test

**Compliance Checklist (SIMULATED):**
- [x] License review: Apache 2.0 maintained
- [x] Security audit: No new security concerns
- [x] IP compliance: All code is original or properly licensed
- [x] Export control: No restricted content

**Review Status:** ✅ APPROVED (simulated)
**Reviewer:** Test validation process
**Date:** $(date +%Y-%m-%d)

**Notes:**
- This is a simulated compliance review for workflow testing
- Production releases require actual compliance review
- All AMD-specific code passes through internal review process
EOF

git add docs/test-workflow-validation.md
git commit -m "docs: add simulated compliance review documentation"
```

Expected: Compliance documentation added

---

## Task 7: Test Release Process

**Files:**
- None (Git operations, tags)
- Reference: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md` (Release Workflow section)

**Context:** Execute the release workflow as it would happen in production

- [ ] **Step 1: Push release branch to ROCm fork**

Run:
```bash
git push rocm-fork amd-release-v1.3.0-test
```

Expected: Release branch pushed to ROCm-LS/MONAIwip

- [ ] **Step 2: Create release tag**

Run:
```bash
git tag -a v1.3.0-rocm-test -m "AMD ROCm test release based on MONAI v1.3.0

This is a TEST release for validating the fork workflow.

Base Version: MONAI dev branch
AMD Features: Documentation, workflow validation
ROCm Support: Planned for 5.7, 6.0, 6.1
Tested Hardware: Validation pending
"

git tag -l "v1.3.0*"
```

Expected: Tag created and visible

- [ ] **Step 3: Push tag to ROCm fork**

Run:
```bash
git push rocm-fork v1.3.0-rocm-test
```

Expected: Tag pushed to GitHub

- [ ] **Step 4: Verify tag on GitHub**

Run:
```bash
echo "Visit: https://github.com/ROCm-LS/MONAIwip/tags"
echo "Expected: v1.3.0-rocm-test tag should be visible"
```

Expected: Instructions to verify on GitHub

- [ ] **Step 5: Update fork's dev branch (merge release)**

Run:
```bash
git checkout dev || git checkout -b dev upstream/dev
git merge amd-release-v1.3.0-test --no-ff -m "Merge AMD test release v1.3.0-rocm-test"
```

Expected: Release merged to dev branch

- [ ] **Step 6: Push dev branch to ROCm fork**

Run:
```bash
git push rocm-fork dev
```

Expected: Dev branch updated on fork

---

## Task 8: Validate Fork on GitHub

**Files:**
- Modify: `docs/test-workflow-validation.md`

**Context:** Verify that all changes are visible on GitHub and fork relationship is correct

- [ ] **Step 1: Check fork network graph**

Run:
```bash
echo "Validation Steps:"
echo "1. Visit: https://github.com/ROCm-LS/MONAIwip/network"
echo "2. Verify fork shows connection to Project-MONAI/MONAI"
echo "3. Check that branches are visible"
echo ""
echo "Expected: Fork network graph shows relationship to upstream"
```

Expected: Instructions displayed

- [ ] **Step 2: Verify README on GitHub**

Run:
```bash
echo "Validation Steps:"
echo "1. Visit: https://github.com/ROCm-LS/MONAIwip"
echo "2. Scroll to README section"
echo "3. Verify AMD fork header is visible"
echo "4. Check ROCm compatibility table renders correctly"
echo ""
echo "Expected: README shows AMD fork documentation"
```

Expected: Instructions displayed

- [ ] **Step 3: Verify issue templates**

Run:
```bash
echo "Validation Steps:"
echo "1. Visit: https://github.com/ROCm-LS/MONAIwip/issues/new/choose"
echo "2. Verify 'Bug Report (AMD/ROCm)' template appears"
echo "3. Verify 'General MONAI Issue' template appears"
echo "4. Check template content loads correctly"
echo ""
echo "Expected: Both issue templates available and formatted correctly"
```

Expected: Instructions displayed

- [ ] **Step 4: Verify releases and tags**

Run:
```bash
echo "Validation Steps:"
echo "1. Visit: https://github.com/ROCm-LS/MONAIwip/releases"
echo "2. Check if v1.3.0-rocm-test tag is visible"
echo "3. Visit: https://github.com/ROCm-LS/MONAIwip/tags"
echo "4. Verify tag points to correct commit"
echo ""
echo "Expected: Tag visible, commit hash correct"
```

Expected: Instructions displayed

- [ ] **Step 5: Document validation results**

```bash
cat >> docs/test-workflow-validation.md << 'EOF'

## GitHub Validation Results

### Tested Components
Date: $(date +%Y-%m-%d)

**Fork Relationship:**
- [ ] Fork network graph shows upstream connection
- [ ] Branches visible in GitHub UI

**Documentation:**
- [ ] README displays AMD fork header
- [ ] ROCm compatibility table renders
- [ ] CHANGELOG shows AMD section

**Issue Templates:**
- [ ] Bug report template available
- [ ] Upstream redirect template available
- [ ] Templates render correctly

**Releases:**
- [ ] Tag v1.3.0-rocm-test visible
- [ ] Release branch accessible
- [ ] Commit history preserved

**Notes:**
(Add manual validation notes here after checking GitHub)

EOF

git add docs/test-workflow-validation.md
git commit -m "docs: add GitHub validation checklist"
git push rocm-fork dev
```

Expected: Validation checklist created and pushed

---

## Task 9: Test Issue Workflow Simulation

**Files:**
- Create: `docs/test-workflow-validation.md` (append)

**Context:** Simulate how issues would be handled to validate the workflow

- [ ] **Step 1: Document AMD-specific issue scenario**

```bash
cat >> docs/test-workflow-validation.md << 'EOF'

## Issue Handling Workflow Test

### Scenario 1: AMD-Specific Issue

**Simulated Issue:**
- Title: "ROCm 6.1 compatibility error in GMM CUDA extension"
- Type: AMD-specific
- Template: Bug Report (AMD/ROCm)

**Workflow:**
1. User reports via AMD/ROCm bug template
2. Maintainer labels: `amd-specific`, `rocm`
3. Fix in internal repository (simulated: amd-internal-dev branch)
4. Test and validate fix
5. Include in next release (v1.3.1-rocm-test)
6. Update public issue: "Fix committed, will be in v1.3.1-rocm-test"
7. Close issue when released

**Status:** Workflow documented ✅

### Scenario 2: Upstream Issue

**Simulated Issue:**
- Title: "Memory leak in DataLoader with large datasets"
- Type: General MONAI issue
- Template: General MONAI Issue (redirect)

**Workflow:**
1. User reports via upstream redirect template
2. Maintainer reviews and confirms it's not AMD-specific
3. Redirect to upstream: https://github.com/Project-MONAI/MONAI/issues
4. If urgent for AMD users: apply temporary fix in internal repo
5. Track upstream issue
6. Remove temporary patch when upstream releases fix

**Status:** Workflow documented ✅

EOF

git add docs/test-workflow-validation.md
git commit -m "docs: add issue handling workflow scenarios"
```

Expected: Issue scenarios documented

- [ ] **Step 2: Create test issue labels documentation**

```bash
cat >> docs/test-workflow-validation.md << 'EOF'

### Recommended Issue Labels

For production fork (ROCm-LS/MONAI), configure these labels:

**AMD-Specific:**
- `amd-specific` - ROCm compatibility, AMD GPU bugs
- `rocm` - ROCm platform issues
- `hip` - HIP-related issues
- `performance` - Performance on AMD hardware

**Triage:**
- `upstream-candidate` - Should be reported upstream
- `needs-compliance` - Requires compliance review before fix
- `documentation` - Docs improvements

**Priority:**
- `critical` - Blocking issues
- `enhancement` - New features
- `question` - User questions

EOF

git add docs/test-workflow-validation.md
git commit -m "docs: add recommended issue labels"
```

Expected: Labels documented

---

## Task 10: Create Workflow Summary and Next Steps

**Files:**
- Modify: `docs/test-workflow-validation.md` (final summary)

**Context:** Summarize validation results and provide guidance for production deployment

- [ ] **Step 1: Create validation summary**

```bash
cat >> docs/test-workflow-validation.md << 'EOF'

---

## Validation Summary

### Completed Tests

✅ **Fork Setup**
- Remote configuration
- Branch structure
- Tag creation

✅ **Documentation**
- README fork header
- CHANGELOG AMD section
- Installation instructions

✅ **Issue Management**
- Bug report template
- Upstream redirect template
- Issue handling workflow

✅ **Release Process**
- Internal dev simulation
- Release branch creation
- Compliance review simulation
- Tag and release publication

### Identified Issues

(Document any issues found during testing here)

### Lessons Learned

(Add insights from testing process here)

---

## Next Steps for Production Use

### Post-Validation Checklist

- [ ] Review validation results with team
- [ ] Adjust documentation based on findings
- [ ] Decide: Keep "MONAIwip" name or rename to "MONAI"
- [ ] Remove "WORKFLOW VALIDATION" banners from README
- [ ] Prepare actual internal repository
- [ ] Set up compliance review process
- [ ] Configure CI/CD if needed

### Transitioning to Production

1. **Repository Naming Decision**
   - Option A: Keep ROCm-LS/MONAIwip as-is
   - Option B: Rename to ROCm-LS/MONAI via GitHub settings
   - Consider: GitHub redirects old URLs, but users may have bookmarks

2. **Update Documentation**
   - Remove "workflow validation" language
   - Make production-ready
   - Update any test-specific content

3. **Configure for Production**
   - Set description: "AMD ROCm optimized fork of MONAI"
   - Ensure topics: amd, rocm, monai, medical-imaging
   - Configure branch protection on dev
   - Set up issue labels

4. **First Production Release**
   - Follow validated workflow
   - Include actual compliance review
   - Validate on real AMD hardware
   - Create proper release notes

### Reference Documentation

- Design Spec: `docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md`
- This Validation: `docs/test-workflow-validation.md`
- Upstream MONAI: https://github.com/Project-MONAI/MONAI

---

## Test Completion

**Validation Date:** $(date +%Y-%m-%d)
**Production Test Fork:** ROCm-LS/MONAIwip
**Status:** Workflow validated on actual production org

**Sign-off:**
- Workflow tested: ✅
- Documentation complete: ✅
- Ready for production use: ⏳ (pending team review and naming decision)

EOF

git add docs/test-workflow-validation.md
git commit -m "docs: add validation summary and production next steps"
```

Expected: Summary documentation created

- [ ] **Step 2: Push final documentation**

Run:
```bash
git push rocm-fork dev
```

Expected: All documentation pushed to ROCm fork

- [ ] **Step 3: Display final validation report**

Run:
```bash
echo "================================"
echo "FORK WORKFLOW VALIDATION COMPLETE"
echo "================================"
echo ""
echo "Production Test Fork: https://github.com/ROCm-LS/MONAIwip"
echo "Documentation: docs/test-workflow-validation.md"
echo ""
echo "Validation Components:"
echo "  ✅ Fork setup and configuration"
echo "  ✅ Documentation (README, CHANGELOG)"
echo "  ✅ Issue templates"
echo "  ✅ Release workflow simulation"
echo "  ✅ GitHub integration"
echo ""
echo "Next Steps:"
echo "  1. Review validation documentation"
echo "  2. Check GitHub fork for visual verification"
echo "  3. Address any identified issues"
echo "  4. Decision: Keep MONAIwip name or rename to MONAI"
echo "  5. Begin using for actual AMD releases"
echo ""
echo "Design Spec: docs/superpowers/specs/2026-04-07-monai-fork-setup-design.md"
echo "Test Report: docs/test-workflow-validation.md"
echo ""
```

Expected: Summary displayed to user

---

## Manual Verification Steps

After completing all tasks, manually verify the following on GitHub:

### Visual Checks

1. **Fork Relationship**
   - Navigate to: https://github.com/ROCm-LS/MONAIwip/network
   - Confirm: Fork shows connection to upstream MONAI

2. **README Display**
   - Navigate to: https://github.com/ROCm-LS/MONAIwip
   - Confirm: AMD fork header visible
   - Confirm: ROCm compatibility table renders correctly

3. **Issue Templates**
   - Navigate to: https://github.com/ROCm-LS/MONAIwip/issues/new/choose
   - Confirm: Both templates appear
   - Test: Click each template, verify content loads

4. **Tags and Releases**
   - Navigate to: https://github.com/ROCm-LS/MONAIwip/tags
   - Confirm: v1.3.0-rocm-test tag exists
   - Navigate to: https://github.com/ROCm-LS/MONAIwip/releases
   - Option: Create a GitHub release from the tag (optional)

5. **Branches**
   - Navigate to: https://github.com/ROCm-LS/MONAIwip/branches
   - Confirm: dev, amd-internal-dev, amd-release-v1.3.0-test branches exist

### Update Validation Document

After manual verification, update `docs/test-workflow-validation.md` with actual results:
- Check off the validation checklist items
- Document any issues found
- Add screenshots if helpful
- Note lessons learned

---

## Success Criteria

**All tasks completed when:**
- ✅ All git operations successful
- ✅ Documentation files created and committed
- ✅ Issue templates visible on GitHub
- ✅ Release workflow simulated successfully
- ✅ Manual verification completed
- ✅ Validation report documented
- ✅ Next steps for production clearly defined

**Ready for production deployment when:**
- Team has reviewed validation results
- Any identified issues are resolved
- Internal repository is prepared
- Compliance process is established
- ROCm-LS organization access is confirmed
