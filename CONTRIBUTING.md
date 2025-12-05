# Contributing to Claude Agentic Workflow

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## How to Contribute

1. **Fork the repository**
2. **Create a feature branch** from `main`
3. **Make your changes** following our conventions
4. **Test your changes** thoroughly
5. **Submit a pull request**

## Commit Message Convention

We use [Conventional Commits](https://www.conventionalcommits.org/) for automatic versioning and changelog generation.

### Format

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types

- **feat**: A new feature (triggers MINOR version bump)
- **fix**: A bug fix (triggers PATCH version bump)
- **docs**: Documentation only changes (triggers PATCH version bump)
- **style**: Code style changes (formatting, missing semicolons, etc.)
- **refactor**: Code refactoring without changing functionality
- **perf**: Performance improvements
- **test**: Adding or updating tests
- **chore**: Maintenance tasks, dependency updates, etc.

### Breaking Changes

To trigger a MAJOR version bump, use one of these:

```bash
# Option 1: Add ! after type
feat!: completely redesign workflow architecture

# Option 2: Add BREAKING CHANGE in footer
feat: update to new action version

BREAKING CHANGE: This requires updating your workflow files to use the new parameter names.
```

### Examples

#### Minor Version Bump (New Feature)
```bash
git commit -m "feat: add support for custom branch naming patterns"
```

#### Patch Version Bump (Bug Fix)
```bash
git commit -m "fix: correct issue with project board integration"
```

#### Major Version Bump (Breaking Change)
```bash
git commit -m "feat!: migrate to new claude-code-action v2 API"
```

#### Documentation Update
```bash
git commit -m "docs: update quick start guide with new screenshots"
```

## Automatic Versioning

### How It Works

When you push to the `main` branch:

1. **Workflow analyzes** commit messages since last tag
2. **Determines version bump** based on commit types:
   - BREAKING CHANGE or `!` → Major bump (v1.0.0 → v2.0.0)
   - `feat:` → Minor bump (v1.0.0 → v1.1.0)
   - `fix:`, `docs:`, etc. → Patch bump (v1.0.0 → v1.0.1)
3. **Creates new tag** automatically
4. **Generates changelog** from commit messages
5. **Creates GitHub release** with changelog

### Manual Release

You can also trigger a release manually:

1. Go to **Actions** tab in GitHub
2. Select **Release and Tag** workflow
3. Click **Run workflow**
4. Choose version bump type (major/minor/patch)
5. Click **Run workflow** button

## Pull Request Guidelines

### Before Submitting

- [ ] Test your changes locally
- [ ] Update documentation if needed
- [ ] Follow commit message conventions
- [ ] Ensure workflows pass (if modifying .yml files)
- [ ] Add examples if adding new features

### PR Title Format

Use the same conventional commit format for PR titles:

```
feat: add workflow customization examples
fix: resolve permission error in issue automation
docs: improve troubleshooting section
```

### PR Description Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix (patch)
- [ ] New feature (minor)
- [ ] Breaking change (major)
- [ ] Documentation update

## Testing
How you tested these changes

## Checklist
- [ ] Commit messages follow conventional commits
- [ ] Documentation updated
- [ ] Examples added (if applicable)
- [ ] Workflows tested
```

## Development Workflow

### Setting Up Development Environment

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/claude-agentic-workflow.git
cd claude-agentic-workflow

# Add upstream remote
git remote add upstream https://github.com/alanops/claude-agentic-workflow.git

# Create feature branch
git checkout -b feat/my-new-feature
```

### Making Changes

```bash
# Make your changes
vim README.md

# Commit using conventional commits
git add .
git commit -m "docs: improve installation instructions"

# Push to your fork
git push origin feat/my-new-feature
```

### Syncing with Upstream

```bash
# Fetch upstream changes
git fetch upstream

# Merge upstream main into your branch
git checkout main
git merge upstream/main

# Rebase your feature branch
git checkout feat/my-new-feature
git rebase main
```

## Code Style

### Workflow Files (.yml)

- Use 2-space indentation
- Add comments for complex logic
- Follow existing structure
- Test workflow syntax: `yamllint .github/workflows/`

### Documentation (.md)

- Use clear, concise language
- Add code examples where helpful
- Include screenshots for UI steps
- Keep line length reasonable (80-100 chars)
- Use proper markdown formatting

### Shell Scripts

- Use `#!/bin/bash` shebang
- Add comments for complex operations
- Use meaningful variable names
- Handle errors appropriately

## Testing

### Testing Workflow Changes

1. **Fork the repository** to your account
2. **Make changes** in your fork
3. **Create a test issue** in your fork
4. **Verify workflow runs** successfully
5. **Check output** matches expectations

### Testing Documentation

1. Follow your own instructions step-by-step
2. Verify all commands work as documented
3. Check links are valid
4. Ensure code examples are accurate

## Reporting Issues

### Bug Reports

Include:
- Clear description of the problem
- Steps to reproduce
- Expected vs actual behavior
- Workflow logs (if applicable)
- Environment details (OS, GitHub setup, etc.)

### Feature Requests

Include:
- Clear description of the feature
- Use case / why it's needed
- Proposed implementation (optional)
- Examples of similar features elsewhere

## Questions?

- **Issues**: Open an issue for bugs or feature requests
- **Discussions**: Use GitHub Discussions for questions
- **Security**: Email security issues privately (see SECURITY.md)

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Recognition

Contributors will be:
- Listed in release notes
- Added to contributors section (if significant contribution)
- Credited in commit history

Thank you for contributing to Claude Agentic Workflow! 🎉
