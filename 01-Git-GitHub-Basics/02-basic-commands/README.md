# 💻 Basic Git Commands

## 🎯 Essential Commands Every BA Should Know

Even if you don't code, understanding these commands helps you communicate with developers.

## 🚀 Starting a Project

### Initialize Git
```bash
git init
```
**What it does:** Tells Git to start tracking this folder  
**When used:** Starting a new project  
**BA Translation:** "Start keeping track of changes in this folder"

### Check Status
```bash
git status
```
**What it does:** Shows what files have changed  
**When used:** All the time! Like checking your to-do list  
**BA Translation:** "What work is pending?"

## 💾 Saving Your Work

### Add Files
```bash
# Add specific file
git add filename.txt

# Add all changed files
git add .
```
**What it does:** Tells Git which files to include in the next save  
**When used:** Before committing changes  
**BA Translation:** "Select these documents to save"

### Commit Changes
```bash
git commit -m "Add user registration feature"
```
**What it does:** Saves a snapshot with a description  
**When used:** After adding files  
**BA Translation:** "Save these changes with a note about what was done"

## 📋 Good Commit Messages

### ✅ Good Examples
```bash
git commit -m "Fix login bug where password was visible"
git commit -m "Add email validation to signup form"
git commit -m "Update privacy policy page"
git commit -m "Remove outdated contact information"
```

### ❌ Bad Examples
```bash
git commit -m "Fixed stuff"
git commit -m "Changes"
git commit -m "Work in progress"
git commit -m "..."
```

### 📝 Commit Message Template
```
[Action] [What] [Why if needed]

Examples:
- Add user profile page
- Fix broken contact form
- Update API documentation for clarity
- Remove deprecated payment method
```

## 🌿 Working with Branches

### Create New Branch
```bash
git branch feature-name
```
**What it does:** Creates a new timeline for your project  
**When used:** Starting work on a new feature  
**BA Translation:** "Create a separate workspace for this new feature"

### Switch Branches
```bash
git checkout feature-name

# Or modern way
git switch feature-name
```
**What it does:** Moves you to a different branch  
**When used:** When you want to work on different features  
**BA Translation:** "Switch to working on this feature"

### Create and Switch (Shortcut)
```bash
git checkout -b new-feature

# Or modern way
git switch -c new-feature
```
**What it does:** Creates and switches to new branch in one command  
**When used:** Starting new work  
**BA Translation:** "Create new workspace and start working there"

## 📖 Viewing History

### See Commit History
```bash
git log
```
**What it does:** Shows all previous saves with messages  
**When used:** When you want to see what's been done  
**BA Translation:** "Show me the history of changes"

### Pretty History
```bash
git log --oneline
```
**What it does:** Shows compact version of history  
**Example output:**
```
a1b2c3d Add user login feature
e4f5g6h Fix email validation bug
i7j8k9l Update homepage design
```

### See What Changed
```bash
git show
```
**What it does:** Shows details of the latest commit  
**When used:** Want to see exactly what was changed  

## 🔄 Working with Remote Repositories

### Connect to GitHub
```bash
git remote add origin https://github.com/username/project.git
```
**What it does:** Links your local project to GitHub  
**When used:** Setting up connection to online repository  
**BA Translation:** "Connect this project to its online backup"

### Upload Changes
```bash
git push origin main
```
**What it does:** Uploads your commits to GitHub  
**When used:** Sharing your work or backing up  
**BA Translation:** "Send my changes to the online backup"

### Download Changes
```bash
git pull origin main
```
**What it does:** Downloads latest changes from GitHub  
**When used:** Getting updates from team members  
**BA Translation:** "Get the latest version from the team"

## 🛠️ Fixing Mistakes

### Undo Last Commit (Keep Changes)
```bash
git reset --soft HEAD~1
```
**What it does:** Undoes the last commit but keeps your changes  
**When used:** You made a commit too early  
**BA Translation:** "Undo that save but keep my work"

### Undo Changes to a File
```bash
git checkout -- filename.txt
```
**What it does:** Throws away changes to a specific file  
**When used:** You messed up a file and want the last saved version  
**BA Translation:** "Restore this file to its last saved state"

### See Differences
```bash
git diff
```
**What it does:** Shows exactly what's changed since last commit  
**When used:** Before committing, to review changes  
**BA Translation:** "Show me what's different from the last save"

## 📊 Daily Workflow Example

Here's how a developer typically uses Git:

```bash
# 1. Check current status
git status

# 2. Get latest changes from team
git pull origin main

# 3. Create new branch for feature
git checkout -b add-search-feature

# 4. Make changes to files...
# (edit code files)

# 5. Check what changed
git status
git diff

# 6. Add files to commit
git add .

# 7. Commit with good message
git commit -m "Add search functionality to product page"

# 8. Push to GitHub
git push origin add-search-feature

# 9. Create Pull Request on GitHub
# (done through GitHub website)
```

## 🎯 Git Commands Cheat Sheet

| Command | What It Does | When To Use |
|---------|--------------|-------------|
| `git status` | Show current state | Always! Check this first |
| `git add .` | Stage all changes | Before committing |
| `git commit -m "message"` | Save changes | After staging |
| `git push` | Upload to GitHub | Share work |
| `git pull` | Download from GitHub | Get updates |
| `git log --oneline` | See history | Review what's been done |
| `git diff` | Show changes | Before committing |
| `git checkout -b branch` | Create new branch | Start new feature |

## 🚨 Important Notes for BAs

### What NOT to Do
- Don't commit sensitive information (passwords, API keys)
- Don't commit large files (videos, big images)
- Don't force push (`git push --force`) unless you know what you're doing

### What TO Do
- Always check `git status` first
- Write clear commit messages
- Pull before you push
- Create branches for new features

## 🎯 Next Steps

Now that you know basic commands, let's explore GitHub features:

👉 [GitHub Features](../03-github-features/README.md)