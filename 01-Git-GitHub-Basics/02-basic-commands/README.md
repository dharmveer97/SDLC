# ⚡ Basic Git Commands (What BAs Need to Know)

## Why BAs Should Understand Git Commands

You don't need to use Git commands daily, but understanding them helps you:

- **Communicate better** with developers
- **Understand status updates** from your team
- **Ask informed questions** about development progress
- **Participate in planning** discussions more effectively

## Essential Concepts (No Code Required!)

### 1. Repository Setup

**What developers say:** "I initialized a new repo"
**What it means:** Created a new project folder that Git will track
**BA impact:** The project is now ready for organized development

### 2. Making Commits

**What developers say:** "I committed the login feature"
**What it means:** Saved a checkpoint with all changes for the login feature
**BA impact:** Progress is tracked and can be reviewed

**Example workflow you might hear:**

1. "Working on user authentication"
2. "Staging the auth changes"
3. "Committing with message: 'Add user login and password validation'"
4. "Login feature is now committed and ready for review"

### 3. Working with Branches

**What developers say:** "I created a feature branch for the payment system"
**What it means:** Made a separate workspace to build payment features without affecting main code
**BA impact:** Multiple features can be developed simultaneously without interference

**Common branch names you'll hear:**

- `main` or `master` - The stable, production-ready code
- `develop` - Integration branch for testing features together
- `feature/user-profile` - Working on user profile functionality
- `bugfix/login-error` - Fixing a specific login issue
- `hotfix/payment-urgent` - Emergency fix for payment problems

### 4. Merging Changes

**What developers say:** "I merged the shopping cart branch"
**What it means:** Combined the shopping cart feature with the main codebase
**BA impact:** Feature is now part of the main product and ready for testing

## Git Status Updates (Decoding Developer Language)

### When developers say

- **"I'm working on a branch"** → They're developing a feature in isolation
- **"Ready for code review"** → Feature is complete and needs peer review
- **"Merge conflict"** → Two developers changed the same code, needs manual resolution
- **"Pushed to GitHub"** → Code is uploaded and available for others to see
- **"Pull request opened"** → Formal request to include changes in main codebase

## Common Git Workflows (What You'll Observe)

### Feature Development Workflow

1. **Branch creation** - Developer starts new feature
2. **Daily commits** - Progress checkpoints with descriptions
3. **Testing** - Feature tested in isolation
4. **Code review** - Team reviews changes
5. **Merge** - Feature added to main codebase
6. **Deployment** - Feature goes live

### Bug Fix Workflow

1. **Issue reported** - Bug identified and documented
2. **Hotfix branch** - Emergency branch created
3. **Quick fix** - Minimal changes to resolve issue
4. **Immediate testing** - Verify fix works
5. **Fast-track merge** - Skip some review steps for urgency
6. **Hotfix deployment** - Fix goes live quickly

## Questions BAs Can Ask Developers

### About Progress

- "Which features are currently on branches?"
- "What's the status of the user-profile branch?"
- "When will the payment feature be ready for merge?"

### About Planning

- "How many commits do you estimate for this feature?"
- "Should we create separate branches for each user story?"
- "What's our branching strategy for this release?"

### About Issues

- "Is there a merge conflict blocking the release?"
- "Which branch should we test for the demo?"
- "Are all features merged into the develop branch?"

## How This Helps Your BA Work

### Better Requirements Writing

- Understand that complex features may need multiple commits
- Know that changes can be reviewed and refined
- Realize that requirements changes can be tracked over time

### Improved Communication

- Speak the same language as developers
- Understand why some changes take longer (merge conflicts, code reviews)
- Know when to expect features to be ready

### Enhanced Project Management

- Track actual progress through commit history
- Understand dependencies between features
- Plan releases based on branch readiness

## What You Don't Need to Do

As a BA, you typically **don't need to:**

- Run Git commands yourself
- Resolve merge conflicts
- Create branches or commits
- Set up repositories

**You might occasionally:**

- Review pull requests for business logic
- Update documentation in Git repositories
- Track issues using GitHub's issue system

## Key Takeaways

- Git commands are tools developers use to organize work
- Understanding the workflow helps you communicate better
- You can track project progress through Git activity
- Features develop on branches and merge when ready
- Every change is tracked and can be reviewed or undone

## Next Steps

Now that you understand how developers use Git day-to-day, let's explore GitHub's collaboration features:

Continue to [GitHub Features](../03-github-features/README.md) to learn about the tools that make team collaboration possible.
