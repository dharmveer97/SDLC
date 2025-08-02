# 🌐 GitHub Features

## 🎯 What Is GitHub?

GitHub = Social network for developers + Cloud storage for code

Think of it as:

- **Facebook** for programmers
- **Google Drive** for code projects
- **Project management** tool
- **Collaboration** platform

## 📁 Repositories

### What Is a Repository?

A repository (repo) = A project folder stored online

```
Repository: "MyWebsite"
├── README.md         ← Project description
├── index.html        ← Website files
├── styles.css
├── script.js
└── images/
    ├── logo.png
    └── background.jpg
```

### Types of Repositories

#### Public Repository

- Anyone can see your code
- Free for everyone
- Great for open source projects
- Good for portfolios

#### Private Repository

- Only you and invited people can see
- Free for small teams
- Good for company projects
- Sensitive code

## 🔀 Pull Requests (PRs)

### What Is a Pull Request?

Pull Request = "Hey team, I made some changes. Can you review them before we add them to the main project?"

### PR Workflow

```
1. Developer creates branch: "add-contact-form"
2. Makes changes and commits
3. Creates Pull Request
4. Team reviews the changes
5. If approved → Merge to main branch
6. If not approved → Request changes
```

### PR Example

```
Title: Add contact form to homepage

Description:
- Added contact form with email validation
- Includes name, email, and message fields
- Form submits to /api/contact endpoint
- Added basic styling to match site design

Changes:
- index.html: Added form HTML
- styles.css: Added form styling
- contact.js: Added validation logic
```

### Why Pull Requests Matter for BAs

- **Quality Control**: Code is reviewed before going live
- **Documentation**: Changes are explained and discussed
- **Collaboration**: Team members can suggest improvements
- **History**: You can see why changes were made

## 🐛 Issues

### What Are Issues?

Issues = Digital to-do list for the project

Think of Issues like:

- Bug reports
- Feature requests
- Tasks to complete
- Questions and discussions

### Issue Example

```
Title: Login button doesn't work on mobile

Labels: bug, mobile, high-priority

Description:
The login button on the homepage doesn't respond when
tapped on mobile devices (tested on iPhone and Android).

Steps to reproduce:
1. Visit homepage on mobile
2. Tap the "Login" button
3. Nothing happens

Expected: Login modal should open
Actual: Button doesn't respond

Browser: Mobile Safari, Chrome Mobile
Device: iPhone 12, Samsung Galaxy S21
```

### Issue Labels

```
🐛 bug          → Something is broken
✨ enhancement  → New feature request
📚 documentation → Docs need updating
❓ question     → Need help or clarification
🚀 priority-high → Urgent issue
🔒 security     → Security-related
```

## 📊 Projects (Kanban Boards)

### GitHub Projects = Digital Kanban Board

```
📋 To Do        📝 In Progress     ✅ Done
├── Add FAQ     ├── Fix login bug  ├── Update logo
├── Fix mobile  ├── User testing   ├── Add footer
└── SEO update  └── Code review    └── Deploy v1.0
```

### Project Workflow

1. **Backlog**: All the things we want to do
2. **To Do**: What we're planning to work on next
3. **In Progress**: Currently being worked on
4. **Review**: Waiting for approval
5. **Done**: Completed and deployed

### Connecting Issues to Projects

- Drag issues from different repositories
- Track progress across multiple projects
- Assign team members
- Set deadlines and milestones

## 👥 Collaboration Features

### User Roles

```
Owner    → Full control, can delete repository
Admin    → Manage settings, add/remove people
Write    → Push code, merge PRs, create branches
Triage   → Manage issues and PRs, no code access
Read     → View and clone repository, no changes
```

### Teams

Organize people into groups:

- **Frontend Team**: Works on user interface
- **Backend Team**: Works on server and database
- **Design Team**: Creates mockups and designs
- **QA Team**: Tests and finds bugs

### Mentions and Notifications

```
@username      → Notify specific person
@team-name     → Notify entire team
#123           → Reference issue #123
PR #456        → Reference pull request #456
```

## 📈 Insights and Analytics

### Repository Insights

- **Contributors**: Who's working on the project
- **Commits**: How much activity over time
- **Code Frequency**: Lines added/removed
- **Traffic**: How many people visit the repo

### Useful for BAs

- Track project progress
- See team productivity
- Identify bottlenecks
- Plan releases

## 🔒 Security Features

### Security Alerts

GitHub automatically scans for:

- Vulnerable dependencies
- Security issues in code
- Exposed secrets (passwords, API keys)

### Branch Protection Rules

```
Main branch protection:
✅ Require pull request reviews
✅ Require status checks (tests must pass)
✅ Require up-to-date branches
❌ Allow force pushes
❌ Allow deletions
```

## 📚 Documentation

### README.md

The front page of your repository:

```markdown
# Project Name

Description of what this project does

## Installation

How to set up the project locally

## Usage

How to use the project

## Contributing

How others can help

## License

Legal information
```

### Wiki

More detailed documentation:

- User guides
- Technical specifications
- Architecture decisions
- API documentation

### GitHub Pages

Host websites directly from your repository:

- Portfolio sites
- Documentation sites
- Project demos
- Company pages

## ⚡ GitHub Actions (Automation)

### What Are GitHub Actions?

Automated workflows that run when something happens:

- Run tests when code is pushed
- Deploy website when changes are merged
- Send notifications to Slack
- Check code quality

### Example Workflow

```
When: Someone pushes code to main branch
Then:
1. Run all tests
2. If tests pass → Deploy to production
3. If tests fail → Send email to developer
4. Update project status
```

## 🌟 Advanced Features

### GitHub Codespaces

- VS Code in your browser
- Full development environment
- No setup required
- Code from anywhere

### GitHub Copilot

- AI coding assistant
- Suggests code as you type
- Helps write functions
- Explains code

### Discussions

- Community forum for your project
- Q&A section
- Announcements
- General chat

## 🎯 GitHub Workflow for BAs

### How BAs Can Use GitHub

#### 1. Track Requirements

- Create issues for each requirement
- Use labels to categorize
- Assign to developers
- Track progress

#### 2. Review Changes

- Check pull requests
- Ensure requirements are met
- Test new features
- Approve or request changes

#### 3. Manage Projects

- Create project boards
- Move issues through workflow
- Set milestones and deadlines
- Report on progress

#### 4. Documentation

- Maintain README files
- Update user guides
- Keep requirements current
- Write acceptance criteria

## 🔍 GitHub Desktop

### GUI Alternative

If command line seems scary:

- GitHub Desktop app
- Visual interface
- Point and click
- Same functionality as command line

### Basic Operations

- Clone repositories
- Create branches
- Commit changes
- Push/pull
- Create pull requests

## 📱 GitHub Mobile

### Stay Connected

- Review pull requests
- Respond to issues
- Get notifications
- Browse code
- Merge approved PRs

## 🎯 Next Steps

Ready to see GitHub in action? Let's look at real workflow examples:

👉 [Workflow Examples](../04-workflow-examples/README.md)
