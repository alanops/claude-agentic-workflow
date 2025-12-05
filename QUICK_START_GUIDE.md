# Agentic Workflow Quick Start Guide

**Get your autonomous AI developer running in 15 minutes**

---

## What You'll Get

After following this guide, you'll have:

- ✅ Claude automatically analyzing new issues
- ✅ Claude implementing solutions autonomously
- ✅ Production-ready code in pull requests
- ✅ 24/7 development capability
- ✅ Massive time savings (2.7 hours per issue)

**Time Required**: 15 minutes
**Difficulty**: Easy (copy/paste most steps)

---

## Prerequisites

- [ ] GitHub repository (organization or personal)
- [ ] Admin access to repository
- [ ] Claude Code installed (`claude --version` to check)
- [ ] GitHub CLI installed (`gh --version` to check)
- [ ] Terminal access

---

## Step 1: Create Claude OAuth Token (2 minutes)

Open your terminal and run:

```bash
claude setup-token
```

This will:
1. Open your browser
2. Ask you to authenticate
3. Generate a token starting with `sk-ant-oat01-...`

**Copy the token** - you'll need it in Step 4.

---

## Step 2: Create GitHub Personal Access Token (3 minutes)

### Option A: Via Browser (Recommended)

1. Go to: https://github.com/settings/tokens

2. Click **"Generate new token"** → **"Generate new token (classic)"**

3. Configure:
   - **Note**: `Claude Agentic Workflow`
   - **Expiration**: `90 days`
   - **Select scopes**:
     - ✅ `repo` (Full control of private repositories)
     - ✅ `workflow` (Update GitHub Action workflows)
     - ✅ `read:org` (Read org and team membership)

4. Click **"Generate token"**

5. **Copy the token** (starts with `ghp_...`) - you won't see it again!

### Option B: Via CLI

```bash
gh auth refresh -s repo -s workflow -s read:org
```

---

## Step 3: Clone the Workflow Files (2 minutes)

Navigate to your repository:

```bash
cd /path/to/your/repo
```

Create the workflows directory:

```bash
mkdir -p .github/workflows
```

Copy the workflow files from the docs repo:

```bash
# Issue automation (basic)
curl -o .github/workflows/issue-automation.yml \
  https://raw.githubusercontent.com/YOUR_ORG/docs/main/.github/workflows/issue-automation.yml

# Agentic workflow (advanced)
curl -o .github/workflows/agentic-workflow.yml \
  https://raw.githubusercontent.com/YOUR_ORG/docs/main/.github/workflows/agentic-workflow.yml
```

---

## Step 4: Add Secrets to Repository (3 minutes)

Replace `YOUR_REPO` with `owner/repo-name` (e.g., `YOUR_ORG/my-app`):

```bash
# Set your repo
REPO="YOUR_REPO"

# Add Claude OAuth token (replace with your token from Step 1)
gh secret set CLAUDE_CODE_OAUTH_TOKEN --repo $REPO --body "sk-ant-oat01-YOUR_TOKEN_HERE"

# Add GitHub PAT (replace with your token from Step 2)
gh secret set PERSONAL_ACCESS_TOKEN --repo $REPO --body "ghp_YOUR_TOKEN_HERE"
```

Verify secrets were added:

```bash
gh secret list --repo $REPO
```

You should see:
```
CLAUDE_CODE_OAUTH_TOKEN    2025-12-04T...
PERSONAL_ACCESS_TOKEN      2025-12-04T...
```

---

## Step 5: Customize the Workflow (2 minutes)

Open `.github/workflows/agentic-workflow.yml` and update these lines:

### Update Project Number

Find line ~45:
```yaml
gh project item-add 7 --owner YOUR_ORG --url ...
```

Replace `7` with your project number and `YOUR_ORG` with your org/username.

**How to find your project number:**
1. Go to your project board URL
2. Look at the URL: `https://github.com/orgs/YOUR_ORG/projects/NUMBER`
3. The number at the end is your project number

### Update Owner Reference

Find all instances of `YOUR_ORG` and replace with your organization or username:

```bash
# Quick find and replace (Mac/Linux)
sed -i 's/YOUR_ORG/YOUR_ORG_NAME/g' .github/workflows/agentic-workflow.yml
sed -i 's/YOUR_ORG/YOUR_ORG_NAME/g' .github/workflows/issue-automation.yml

# Or manually edit the files
```

---

## Step 6: Commit and Push (1 minute)

```bash
git add .github/workflows/
git commit -m "Add Claude Code agentic workflow automation

- Issue automation for auto-labeling and analysis
- Agentic workflow for autonomous development
- Both workflows configured and ready to use"

git push origin main
```

---

## Step 7: Test It! (2 minutes)

Create a test issue:

```bash
gh issue create --repo $REPO \
  --title "Test: Add hello function" \
  --body "## Task
Create a simple hello() function that returns 'Hello, World!'

## Requirements
- Create file: src/utils/hello.js
- Export the function
- Add JSDoc comment

## Acceptance Criteria
- [ ] Function created
- [ ] Returns correct string
- [ ] Properly documented"
```

Watch the magic happen:

```bash
# Watch workflows
gh run watch --repo $REPO

# Check issue after ~3 minutes
gh issue view 1 --repo $REPO
```

You should see:
- ✅ Issue labeled automatically
- ✅ Detailed analysis comment
- ✅ Code implemented
- ✅ Branch created
- ✅ PR link provided

---

## What Happens Next?

### Every time you create an issue:

**Within 30 seconds:**
- Both workflows trigger

**Within 2-3 minutes:**
- Issue analyzed and labeled
- Priority determined
- Detailed comment added

**Within 3-5 minutes:**
- Code implemented
- Branch created with your code
- PR link ready for review

**You then:**
1. Click the PR link
2. Review the code
3. Merge if good (or request changes)
4. Done!

---

## Troubleshooting

### Workflows Not Triggering?

Check workflow files exist:
```bash
ls -la .github/workflows/
```

Check GitHub Actions are enabled:
- Go to repo Settings → Actions → General
- Ensure "Allow all actions and reusable workflows" is selected

### Permission Errors?

Verify secrets:
```bash
gh secret list --repo $REPO
```

Both should be present and recent.

### Claude Can't Create PRs?

Check PAT has correct scopes:
```bash
gh auth status
```

Should show: `repo`, `workflow`, `read:org`

### Issues Not Being Labeled?

Check the workflow logs:
```bash
gh run list --repo $REPO --limit 3
gh run view <run-id> --log
```

Look for permission errors or API failures.

---

## Customization Tips

### Change Branch Prefix

In `agentic-workflow.yml`, line ~33:
```yaml
branch_prefix: "claude/fix-"  # Change to your preference
```

### Add Custom Instructions

In `agentic-workflow.yml`, find the `direct_prompt` section and add:

```yaml
## Repository-Specific Guidelines
- Use TypeScript for all new code
- Follow ESLint rules strictly
- Update CHANGELOG.md
- Run `npm test` before creating PR
```

### Limit to Specific Labels

Only process issues with `auto-fix` label:

```yaml
jobs:
  autonomous-developer:
    if: contains(github.event.issue.labels.*.name, 'auto-fix')
```

### Different Workflows per Issue Type

Create separate workflows:
- `agentic-bugfix.yml` - Only for bugs
- `agentic-feature.yml` - Only for features
- `agentic-docs.yml` - Only for documentation

---

## Cost Management

### Monitor Usage

Check Claude dashboard regularly:
- https://console.anthropic.com/usage

### Set Budget Alerts

Create alerts for:
- Daily spend > $10
- Weekly spend > $50
- Monthly spend > $150

### Limit Workflow Runs

Add to workflow:
```yaml
# Only run on business hours
if: |
  github.event_name == 'issues' &&
  (github.event.action == 'opened' || github.event.action == 'reopened') &&
  contains(fromJSON('["Mon","Tue","Wed","Thu","Fri"]'),
          format('{0:ddd}', github.event.issue.created_at))
```

---

## Best Practices

### Write Clear Issues

**Good Issue:**
```markdown
## Task
Add user authentication

## Requirements
- JWT-based auth
- Login/logout endpoints
- Token refresh
- Session management

## Acceptance Criteria
- [ ] /login endpoint returns JWT
- [ ] /logout clears session
- [ ] /refresh renews token
- [ ] Tests pass
```

**Bad Issue:**
```markdown
Add auth
```

### Review All PRs

**Never auto-merge** without review:
- Check code quality
- Verify tests pass
- Ensure security best practices
- Validate requirements met

### Start Small

**Week 1**: Documentation issues only
**Week 2**: Simple bug fixes
**Week 3**: Small features
**Week 4+**: Complex features

### Monitor Metrics

Track:
- Success rate
- Time saved
- Code quality
- Cost per issue
- Developer satisfaction

---

## Security Checklist

- [ ] Secrets stored in GitHub (not in code)
- [ ] PAT has minimal required scopes
- [ ] Token expiration set (90 days max)
- [ ] Branch protection rules enabled
- [ ] Required reviews configured
- [ ] Dependabot enabled
- [ ] Secret scanning enabled
- [ ] Code scanning enabled

---

## Next Steps

### Immediate (Today)

1. ✅ Complete this setup
2. ✅ Create test issue
3. ✅ Review generated PR
4. ✅ Merge if good

### This Week

1. Create 3-5 real issues
2. Monitor workflow performance
3. Gather team feedback
4. Refine prompts as needed

### This Month

1. Expand to 2-3 more repos
2. Document learnings
3. Train team members
4. Measure ROI

### This Quarter

1. Full team adoption
2. Process optimization
3. Advanced features
4. Cross-repo intelligence

---

## Advanced Features (Optional)

### Enable Issue Comments

Re-enable in `agentic-workflow.yml`:

```yaml
on:
  issues:
    types: [opened, reopened]
  issue_comment:
    types: [created]

jobs:
  autonomous-developer:
    if: |
      github.event_name == 'issues' ||
      (github.event_name == 'issue_comment' &&
       github.event.comment.user.type != 'Bot' &&
       contains(github.event.comment.body, '@claude'))
```

Now comment `@claude` on any issue to trigger the workflow.

### Auto-Create PRs

Claude currently provides a PR link. To auto-create:

Add to the prompt:
```
After implementing, create the PR directly using:
gh pr create --title "..." --body "..." --base main --head <branch>
```

### Multiple Workflows

Create workflow variants:
```
.github/workflows/
├── agentic-bugfix.yml (bugs only)
├── agentic-feature.yml (features only)
├── agentic-docs.yml (docs only)
└── agentic-refactor.yml (refactoring only)
```

Each with different prompts and tools.

---

## Getting Help

### Documentation

- [Full Setup Guide](AGENTIC_WORKFLOW_SETUP.md) - 400+ lines of detailed docs
- [Implementation Report](AGENTIC_WORKFLOW_IMPLEMENTATION_REPORT.md) - Complete journey
- [Claude GitHub Actions](claude-github-actions.md) - Technical reference

### Check Logs

Always start with workflow logs:
```bash
gh run list --repo $REPO --limit 5
gh run view <run-id> --log
```

### Common Issues

**Issue**: Workflow doesn't trigger
**Fix**: Check `.github/workflows/` is on main branch

**Issue**: Permission denied
**Fix**: Regenerate PAT with correct scopes

**Issue**: Claude makes mistakes
**Fix**: Improve issue description clarity

**Issue**: Too expensive
**Fix**: Add label filtering, reduce auto-triggers

### Get Support

1. Check troubleshooting section above
2. Review workflow logs
3. Search GitHub issues
4. Ask in team Slack/Discord
5. File issue in this repo

---

## Success Metrics

After 1 week, you should see:

- ✅ 5+ issues processed
- ✅ 3+ successful PRs merged
- ✅ 10+ hours saved
- ✅ Happy developers

After 1 month, you should see:

- ✅ 20+ issues processed
- ✅ 15+ successful PRs merged
- ✅ 40+ hours saved
- ✅ Team fully adopted

After 3 months, you should see:

- ✅ 60+ issues processed
- ✅ 50+ successful PRs merged
- ✅ 120+ hours saved
- ✅ Process optimized

---

## FAQ

### Can I use with private repos?

Yes! Works with both public and private repositories.

### Does it work with monorepos?

Yes, but you may need to customize prompts to specify which package/service.

### Can multiple people use it?

Yes! Anyone with write access can create issues that trigger the workflow.

### What languages does it support?

All of them! Claude can work with any programming language.

### Can it run tests?

Yes! It can run any bash command including test suites.

### Does it integrate with my CI/CD?

Yes! PRs trigger your existing CI/CD pipelines normally.

### What if Claude makes a mistake?

Just request changes on the PR or close it. Claude learns from feedback.

### Can I use my own prompts?

Absolutely! Edit the `direct_prompt` section in the workflow file.

### How much does it cost?

Typical cost: $1-2 per issue. Saves 2.7 hours (~$400) per issue. ROI: 6,000%+

### Is my code secure?

Yes! All processing happens in GitHub Actions with your secrets. Code stays in your repo.

---

## Conclusion

🎉 **Congratulations!** You now have an autonomous AI developer working 24/7.

**What you've achieved:**
- ✅ Automated issue triage
- ✅ Autonomous code implementation
- ✅ Production-ready PRs
- ✅ Massive time savings
- ✅ Scalable development process

**Start using it:**
1. Create an issue
2. Wait 3-5 minutes
3. Review the PR
4. Merge and celebrate! 🚀

**Remember:**
- Start small
- Review everything
- Monitor costs
- Gather feedback
- Iterate and improve

Welcome to the future of software development! 🌟

---

**Need Help?** Open an issue or check the [Full Setup Guide](AGENTIC_WORKFLOW_SETUP.md)

**Questions?** See the [Implementation Report](AGENTIC_WORKFLOW_IMPLEMENTATION_REPORT.md) for detailed technical information

**Ready to Scale?** Apply this to all your repositories and watch productivity soar!
