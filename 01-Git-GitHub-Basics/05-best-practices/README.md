# ✅ Git & GitHub Best Practices

## 🎯 Essential Rules for Everyone

These practices will save you time, prevent problems, and make collaboration smoother.

## 📝 Commit Best Practices

### Write Clear Commit Messages

#### ✅ Good Examples

```bash
git commit -m "Add email validation to signup form"
git commit -m "Fix mobile navigation menu not closing"
git commit -m "Update user profile API documentation"
git commit -m "Remove deprecated payment gateway integration"
```

#### ❌ Bad Examples

```bash
git commit -m "fix"
git commit -m "changes"
git commit -m "work stuff"
git commit -m "asdf"
git commit -m "final version"
```

### Commit Message Template

```
[Type]: [Brief description]

Examples:
Add: New feature or functionality
Fix: Bug fixes
Update: Modifications to existing features
Remove: Deleted code or features
Refactor: Code improvements without changing functionality
Docs: Documentation changes
Style: Formatting, no code changes
Test: Adding or updating tests
```

### Commit Frequency

```
✅ DO: Commit small, logical changes frequently
❌ DON'T: Save up a day's work in one massive commit

Good example:
- Commit 1: "Add user registration form HTML"
- Commit 2: "Add form validation logic"
- Commit 3: "Style registration form"
- Commit 4: "Add success/error messages"

Bad example:
- Commit 1: "Complete entire user registration system"
```

## 🌿 Branch Strategy

### Branch Naming Convention

```
feature/feature-name     → feature/user-profile
bugfix/issue-description → bugfix/login-validation
hotfix/critical-fix      → hotfix/security-patch
release/version-number   → release/v1.2.0
docs/documentation-type  → docs/api-guide
```

### Branch Lifecycle

```
1. Create branch from main
2. Work on feature/fix
3. Push branch to GitHub
4. Create Pull Request
5. Get review and approval
6. Merge to main
7. Delete branch (cleanup)
```

### Keep Branches Focused

```
✅ One feature per branch
- feature/user-login
- feature/password-reset
- feature/email-verification

❌ Multiple unrelated changes
- feature/login-and-dashboard-and-settings
```

## 🔄 Pull Request Guidelines

### PR Title and Description

```markdown
Title: Add user authentication system

## Summary

Implements complete user authentication including login, registration,
and password reset functionality.

## Changes Made

- Added login/registration forms
- Implemented JWT token authentication
- Created password reset workflow
- Added email verification
- Updated API documentation

## Testing

- [x] Unit tests for auth functions
- [x] Integration tests for login flow
- [x] Manual testing on dev environment
- [x] Cross-browser testing completed

## Screenshots

[Include relevant UI screenshots]

## Checklist

- [x] Code follows style guidelines
- [x] Self-review completed
- [x] Tests added/updated
- [x] Documentation updated

Fixes #123, #124
Related to #125
```

### PR Size Guidelines

```
✅ Small PRs (under 300 lines)
- Easier to review
- Faster to merge
- Lower chance of bugs
- Less likely to have conflicts

❌ Large PRs (over 500 lines)
- Hard to review thoroughly
- Takes longer to merge
- More likely to have issues
- Difficult to troubleshoot
```

## 🔒 Security Best Practices

### Never Commit Sensitive Information

```bash
❌ NEVER commit:
- Passwords
- API keys
- Database credentials
- Private certificates
- Personal information
- Internal URLs

✅ Use instead:
- Environment variables (.env files)
- Secret management services
- Configuration files (in .gitignore)
```

### .gitignore File

```gitignore
# Environment variables
.env
.env.local
.env.production

# Dependencies
node_modules/
vendor/

# Build outputs
dist/
build/
*.log

# IDE files
.vscode/
.idea/
*.swp

# OS files
.DS_Store
Thumbs.db

# Sensitive files
config/secrets.json
private-keys/
```

### API Key Management

```javascript
// ❌ BAD: Hard-coded in code
const apiKey = "sk_live_abc123xyz789";

// ✅ GOOD: Environment variable
const apiKey = process.env.STRIPE_API_KEY;

// ❌ BAD: Committed in file
// config.json
{
  "database": "mongodb://user:password@server:27017/db"
}

// ✅ GOOD: Environment variable
// config.js
const config = {
  database: process.env.DATABASE_URL
};
```

## 📊 Repository Organization

### File Structure

```
project-name/
├── README.md              ← Project overview
├── .gitignore            ← Files to ignore
├── package.json          ← Dependencies
├── LICENSE               ← Legal information
├── docs/                 ← Documentation
│   ├── api.md
│   ├── setup.md
│   └── contributing.md
├── src/                  ← Source code
│   ├── components/
│   ├── utils/
│   └── tests/
└── scripts/              ← Build/deploy scripts
```

### README.md Template

````markdown
# Project Name

Brief description of what this project does.

## Features

- Feature 1
- Feature 2
- Feature 3

## Installation

```bash
npm install
```
````

## Usage

```bash
npm start
```

## Contributing

1. Fork the repository
2. Create feature branch
3. Make changes
4. Submit pull request

## License

MIT License

```

## 👥 Collaboration Best Practices

### Code Review Guidelines

#### For Authors (Who Submit PRs)
```

✅ DO:

- Test your changes thoroughly
- Write clear description
- Keep changes focused
- Respond to feedback promptly
- Update based on suggestions

❌ DON'T:

- Submit untested code
- Ignore review comments
- Mix unrelated changes
- Take feedback personally

```

#### For Reviewers
```

✅ DO:

- Review promptly (within 24-48 hours)
- Be constructive and specific
- Test functionality if possible
- Explain the "why" behind suggestions
- Approve when ready

❌ DON'T:

- Just say "looks good" without reviewing
- Be overly critical or rude
- Request changes without explanation
- Let PRs sit for days

```

### Communication Strategies
```

GitHub Features for Communication:

- @mentions for specific people
- Issues for tracking problems/features
- Discussions for general questions
- Project boards for progress tracking
- Milestones for release planning

````

## 🚨 Emergency Procedures

### Hotfix Process
```bash
# For critical production bugs
1. git checkout main
2. git pull origin main
3. git checkout -b hotfix/critical-issue
4. Make minimal fix
5. Test thoroughly
6. Create PR with "HOTFIX" label
7. Get expedited review
8. Merge and deploy immediately
9. Monitor production closely
````

### Rollback Process

```bash
# If deployment causes issues
1. git checkout main
2. git revert HEAD  # Undo last commit
3. git push origin main
4. Or use deployment platform rollback feature
5. Investigate issue in separate branch
6. Fix and redeploy properly
```

### Accidental Main Branch Push

```bash
# If you accidentally pushed bad code to main
1. git revert <commit-hash>  # Undo the bad commit
2. git push origin main      # Push the revert
3. Create new branch to fix properly
4. Submit PR for review
```

## 📈 Workflow Optimization

### Daily Git Habits

```bash
# Start of day
git checkout main
git pull origin main

# Before starting new work
git checkout -b feature/my-task

# Throughout the day
git add .
git commit -m "Clear commit message"
git push origin feature/my-task

# End of day
git push origin feature/my-task  # Backup your work
```

### Weekly Maintenance

```bash
# Clean up merged branches
git branch --merged | grep -v main | xargs git branch -d

# Update main branch
git checkout main
git pull origin main

# Update dependencies (if applicable)
npm update
```

## 🎯 Team Standards

### Establish Team Rules

```markdown
Team Git Guidelines:

1. Always work on feature branches
2. Require PR reviews before merging
3. Use descriptive commit messages
4. Keep PRs small and focused
5. Test before submitting PR
6. Delete branches after merging
7. Use issue templates
8. Tag releases properly
```

### Issue Templates

```markdown
<!-- Bug Report Template -->

**Bug Description**
Clear description of the bug

**Steps to Reproduce**

1. Step one
2. Step two
3. Step three

**Expected Behavior**
What should happen

**Actual Behavior**
What actually happens

**Environment**

- Browser:
- OS:
- Device:

**Screenshots**
Add screenshots if applicable
```

## 📊 Measuring Success

### Key Metrics to Track

```
Code Quality:
- Pull request review time
- Number of bugs found in review
- Time from PR to deployment

Collaboration:
- Number of contributors
- Frequency of commits
- Issue resolution time

Process:
- Branch naming compliance
- Commit message quality
- Documentation completeness
```

## 🎯 Tools and Integrations

### Useful GitHub Apps

```
Code Quality:
- CodeClimate (code analysis)
- SonarCloud (code quality)
- Snyk (security scanning)

Project Management:
- ZenHub (project boards)
- Linear (issue tracking)
- Slack (notifications)

CI/CD:
- GitHub Actions (automation)
- Netlify (deployment)
- Vercel (hosting)
```

### VS Code Extensions

```
- GitLens (enhanced Git features)
- GitHub Pull Requests (review PRs in editor)
- Git Graph (visualize branch history)
- GitIgnore (generate .gitignore files)
```

## 🎯 Key Takeaways

### For Business Analysts

1. **Good practices reduce bugs** - Proper workflow catches issues early
2. **Clear communication saves time** - Detailed issues and PRs prevent confusion
3. **Small changes are safer** - Incremental updates are easier to review and fix
4. **Documentation matters** - Good docs help everyone understand the project
5. **Security is everyone's responsibility** - Never commit sensitive information

### For Everyone

1. **Consistency is key** - Follow established patterns and conventions
2. **Communication is crucial** - Over-communicate rather than under-communicate
3. **Quality over speed** - Take time to do things right the first time
4. **Learn from mistakes** - Use post-mortems to improve processes
5. **Stay updated** - Keep learning new Git features and best practices

## 🎯 Next Module

Great job! You now understand Git and GitHub fundamentals. Ready to learn about the tools developers use every day?

👉 [Development Tools - VS Code, ESLint, and More](../../02-Development-Tools/README.md)
