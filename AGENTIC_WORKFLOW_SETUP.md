# Agentic Workflow Setup Guide

Transform your repository into an AI-powered development environment where Claude autonomously implements solutions.

## Overview

The agentic workflow enables Claude to:
1. Analyze new issues automatically
2. Understand your codebase
3. Implement complete solutions
4. Create pull requests for review
5. Move issues through your Kanban board

## Prerequisites

- Claude Code v1.0.44 or later
- GitHub organization or personal repository
- Admin access to repository settings
- GitHub Projects v2 board

## Step 1: Create a GitHub Personal Access Token (PAT)

The agentic workflow needs a PAT to access organization project boards and create branches/PRs.

### Generate the PAT

1. Go to GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Click "Generate new token"
3. Configure the token:
   - **Name**: "Claude Agentic Workflow"
   - **Expiration**: Choose appropriate duration (90 days recommended)
   - **Repository access**: Select your repositories
   - **Permissions**:
     - **Repository permissions**:
       - Contents: Read and write
       - Issues: Read and write
       - Pull requests: Read and write
       - Workflows: Read and write
     - **Organization permissions** (if using org projects):
       - Projects: Read and write

4. Click "Generate token"
5. **Copy the token immediately** - you won't be able to see it again!

### Add PAT to Repository Secrets

1. Go to your repository → Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `PERSONAL_ACCESS_TOKEN`
4. Value: Paste the token you copied
5. Click "Add secret"

## Step 2: Verify OAuth Token

Ensure your Claude OAuth token is configured (from earlier setup):

```bash
# Check if secret exists
gh secret list --repo YOUR_ORG/docs | grep CLAUDE_CODE_OAUTH_TOKEN
```

If not present, add it:
```bash
claude setup-token
gh secret set CLAUDE_CODE_OAUTH_TOKEN --repo YOUR_ORG/docs --body "<your-token>"
```

## Step 3: Configure Project Board

The workflow assumes a project board with these columns:
- **Backlog** - New issues start here
- **Ready** - Issues ready for implementation
- **In Progress** - Currently being worked on
- **Review** - PR created, awaiting review
- **Done** - Merged and deployed

### Get Your Project Number

1. Navigate to your project board URL
2. The number is in the URL: `https://github.com/orgs/YOUR_ORG/projects/7`
   - In this case, the project number is `7`

3. Update the workflow file (`.github/workflows/agentic-workflow.yml`) if your project number is different:
   ```yaml
   gh project item-add <YOUR_PROJECT_NUMBER> --owner YOUR_ORG ...
   ```

## Step 4: Customize the Workflow (Optional)

Edit `.github/workflows/agentic-workflow.yml` to customize behavior:

###Branch Prefix
```yaml
branch_prefix: "claude/fix-issue-"  # Change to your preference
```

### Allowed Tools
```yaml
allowed_tools: "Bash,Read,Write,Edit,Glob,Grep,WebFetch"
# Add or remove tools as needed
```

### Trigger Conditions
```yaml
on:
  issues:
    types: [opened, reopened]  # Trigger on new or reopened issues
  issue_comment:
    types: [created]  # Also trigger on comments (for @claude mentions)
```

### Add Custom Instructions

In the `direct_prompt` section, add repository-specific guidelines:
```yaml
direct_prompt: |
  # Add after "Important Guidelines" section:

  ## Repository-Specific Rules
  - Always run `npm test` before creating PR
  - Follow the TypeScript strict mode
  - Update CHANGELOG.md with your changes
  - Add JSDoc comments to all public functions
```

## Step 5: Test the Workflow

Create a test issue to verify everything works:

```bash
gh issue create --repo YOUR_ORG/docs \
  --title "Test: Add hello world function" \
  --body "Create a simple hello() function in utils.js that returns 'Hello, World!'"
```

### What to Expect

1. **Within 30 seconds**: Workflow triggers
2. **1-2 minutes**: Claude analyzes codebase
3. **2-5 minutes**: Claude implements solution
4. **Final**: PR created and issue moved to Review

### Monitor Progress

Watch the workflow:
```bash
gh run watch --repo YOUR_ORG/docs
```

View the issue comments to see Claude's thought process and decisions.

## Step 6: Review and Merge

1. Check the PR Claude created
2. Review the code changes
3. Run tests locally if desired
4. Approve and merge (or request changes)

## Troubleshooting

### Workflow Not Triggering

**Check:**
- Workflow file is in `.github/workflows/` on main branch
- File has correct YAML syntax: `yamllint .github/workflows/agentic-workflow.yml`
- Repository Actions are enabled: Settings → Actions → Allow all actions

### Permission Errors

**Error**: "Resource not accessible by integration"

**Solution**: Verify PAT has correct permissions and is not expired

```bash
# Test PAT locally
gh auth login --with-token < <PAT>
gh project list --owner YOUR_ORG
```

### Project Board Not Updating

**Error**: "Could not resolve to a ProjectV2 with the number X"

**Solution**: Verify project number and ensure PAT has project permissions

```bash
# List projects to find correct number
gh project list --owner YOUR_ORG
```

### Claude Not Creating PR

**Check workflow logs:**
```bash
gh run list --repo YOUR_ORG/docs --limit 1
gh run view <run-id> --log
```

**Common issues:**
- Base branch doesn't exist
- Conflicts with existing branches
- No changes to commit (Claude determined issue already resolved)

## Security Considerations

### PAT Security

- **Store securely**: Never commit PATs to code
- **Rotate regularly**: Set expiration and rotate tokens
- **Minimal permissions**: Only grant what's needed
- **Monitor usage**: Check audit logs regularly

### Code Review

- **Always review**: Never merge without human review
- **Check for secrets**: Ensure no API keys or credentials added
- **Test thoroughly**: Run full test suite before merging
- **Security scans**: Use dependabot and security scanning

## Best Practices

### Issue Quality

Write clear issues for best results:
```markdown
## Problem
Describe what's wrong or what's needed

## Expected Behavior
What should happen

## Acceptance Criteria
- [ ] Specific, testable criteria
- [ ] Edge cases handled
- [ ] Tests added
```

### Workflow Optimization

1. **Start with simple issues**: Let Claude learn your codebase
2. **Provide feedback**: Comment on PRs to guide future work
3. **Update CLAUDE.md**: Add coding standards and conventions
4. **Monitor costs**: Check Claude usage to manage API costs

### Gradual Adoption

**Phase 1**: Use for documentation and simple bug fixes
**Phase 2**: Expand to features and refactoring
**Phase 3**: Full autonomous development for most issues

## Advanced Configuration

### Conditional Execution

Only run for labeled issues:
```yaml
if: contains(github.event.issue.labels.*.name, 'claude-auto')
```

### Multiple Workflows

Create separate workflows for different issue types:
- `agentic-bugfix.yml` - For bug fixes only
- `agentic-docs.yml` - For documentation
- `agentic-features.yml` - For new features

### Integration with CI/CD

Ensure Claude's PRs trigger your existing CI:
```yaml
# In your CI workflow
on:
  pull_request:
    branches: [main]
  # Will run for Claude-created PRs too
```

## Cost Management

### Estimate Costs

- **Simple issues**: ~$0.10-0.50 per issue
- **Medium issues**: ~$0.50-2.00 per issue
- **Complex issues**: ~$2.00-5.00 per issue

### Cost Controls

1. **Set issue labels**: Only auto-process labeled issues
2. **Limit workflow triggers**: Don't trigger on every comment
3. **Use max_turns parameter**: Limit autonomous turns
4. **Monitor usage**: Check Claude dashboard regularly

```yaml
# Add to workflow
with:
  max_turns: 20  # Limit to 20 back-and-forth iterations
```

## Next Steps

1. ✅ Complete setup steps above
2. 📝 Create test issue and verify workflow
3. 🔧 Customize prompt for your codebase
4. 📊 Monitor and iterate based on results
5. 🚀 Gradually expand autonomous development scope

## Support

- **GitHub Issues**: Report problems in this repository
- **Workflow Logs**: Always check logs first for debugging
- **Claude Docs**: https://docs.claude.com/claude-code
- **GitHub Actions Docs**: https://docs.github.com/actions

---

**Remember**: The agentic workflow is a powerful tool. Start small, learn what works, and gradually increase autonomy as you build confidence in the system.
