# 🔄 Real-World Git & GitHub Workflows

## 🎯 Complete Feature Development Example

Let's follow a complete feature from idea to production.

### Scenario: Adding a "Forgot Password" Feature

#### Step 1: Create Issue (BA Creates)
```
Title: Add forgot password functionality

Labels: enhancement, user-experience, high-priority

Description:
Users need to be able to reset their password if they forget it.

Acceptance Criteria:
- [ ] "Forgot Password" link on login page
- [ ] User enters email address
- [ ] System sends reset email (if email exists)
- [ ] Email contains secure reset link
- [ ] Link expires after 24 hours
- [ ] User can set new password
- [ ] User is logged in after successful reset

Technical Notes:
- Email template needed
- Database field for reset tokens
- Security considerations for tokens

Assigned to: @developer-john
Milestone: Sprint 15
```

#### Step 2: Developer Workflow

```bash
# 1. Developer gets latest code
git checkout main
git pull origin main

# 2. Creates feature branch
git checkout -b feature/forgot-password

# 3. Works on the feature (multiple commits)
git add forgot-password.html
git commit -m "Add forgot password page HTML structure"

git add styles.css
git commit -m "Style forgot password form"

git add auth.js
git commit -m "Add password reset logic"

git add email-template.html
git commit -m "Create password reset email template"

# 4. Pushes branch to GitHub
git push origin feature/forgot-password
```

#### Step 3: Create Pull Request (Developer)
```
Title: Add forgot password functionality

Fixes #234

## Changes Made
- Added forgot password page (/forgot-password)
- Created email template for reset links
- Added password reset logic in backend
- Added database migration for reset tokens
- Added form validation and error handling

## Testing
- [x] Form validates email format
- [x] Reset email sent for valid emails
- [x] No email sent for invalid addresses (security)
- [x] Reset link works correctly
- [x] Reset link expires after 24 hours
- [x] Password successfully updated

## Screenshots
![Forgot Password Form](screenshot1.png)
![Reset Email](screenshot2.png)

## Checklist
- [x] Code follows style guidelines
- [x] Self-review completed
- [x] Tests added/updated
- [x] Documentation updated

Ready for review! @ba-sarah @developer-mike
```

#### Step 4: Review Process (BA + Team)

**BA Review (Sarah):**
```
Great work! I tested the feature and it works as expected. 
Few suggestions:

✅ All acceptance criteria met
✅ Email template looks professional
✅ Security considerations handled well

💬 Minor feedback:
- Could we add a confirmation message after email is sent?
- "Password" field label could be "New Password" for clarity

Approved pending minor changes.
```

**Developer Review (Mike):**
```
Code looks good overall! Few technical suggestions:

✅ Logic is sound
✅ Error handling is comprehensive
✅ Security best practices followed

💬 Code feedback:
- Consider adding rate limiting for reset requests
- Small typo in line 45 of auth.js
- Might want to add logging for security events

Approved after addressing the typo.
```

#### Step 5: Address Feedback
```bash
# Developer makes requested changes
git add confirmation-message.html
git commit -m "Add email sent confirmation message"

git add auth.js
git commit -m "Fix typo and improve field labels"

git push origin feature/forgot-password
```

#### Step 6: Final Approval & Merge
```bash
# After approvals, merge to main
git checkout main
git pull origin main
git merge feature/forgot-password
git push origin main

# Delete feature branch (cleanup)
git branch -d feature/forgot-password
git push origin --delete feature/forgot-password
```

#### Step 7: Deployment & Testing
```
🚀 Auto-deployment triggered
✅ Tests passed
✅ Deployed to staging
✅ BA tests on staging
✅ Deployed to production
✅ Issue closed automatically
```

## 🐛 Bug Fix Workflow

### Scenario: Login Button Not Working on Mobile

#### Step 1: Bug Report (User or QA)
```
Title: Login button unresponsive on mobile devices

Labels: bug, mobile, high-priority

Description:
Login button doesn't respond when tapped on mobile devices.

Steps to Reproduce:
1. Open website on mobile device
2. Navigate to login page
3. Tap "Login" button
4. Nothing happens

Expected: Login form should submit
Actual: Button doesn't respond to touch

Environment:
- Device: iPhone 12, Samsung Galaxy S21
- Browser: Safari Mobile, Chrome Mobile
- OS: iOS 15, Android 12

Screenshots attached.

Reported by: @qa-tester
Assigned to: @developer-frontend
Priority: High (blocking user logins)
```

#### Step 2: Quick Investigation
```bash
# Developer creates hotfix branch
git checkout main
git pull origin main
git checkout -b hotfix/mobile-login-button

# Investigates and finds the issue
# CSS hover effects preventing touch events
```

#### Step 3: Quick Fix
```bash
# Fix the CSS issue
git add mobile-styles.css
git commit -m "Fix mobile login button touch events

- Remove hover-only CSS that blocked touch
- Add proper mobile touch handling
- Test on iOS and Android devices

Fixes #456"

# Push and create PR immediately
git push origin hotfix/mobile-login-button
```

#### Step 4: Fast-Track Review
```
Title: HOTFIX: Fix mobile login button touch events

🚨 URGENT: This fixes a critical bug blocking user logins on mobile

## Issue
Login button wasn't responding to touch on mobile devices due to 
CSS hover effects interfering with touch events.

## Solution
- Removed problematic hover-only CSS
- Added proper mobile touch handling
- Tested on multiple devices

## Testing
- [x] iPhone 12 - Safari ✅
- [x] iPhone 12 - Chrome ✅  
- [x] Samsung Galaxy S21 - Chrome ✅
- [x] iPad - Safari ✅

Fixes #456

@ba-sarah - Can you quickly verify this works on your device?
```

#### Step 5: Quick Deploy
```bash
# After fast approval, merge immediately
git checkout main
git merge hotfix/mobile-login-button
git push origin main

# Hotfix deployed to production
# Bug verified fixed
# Issue closed
```

## 👥 Team Collaboration Workflow

### Scenario: Multi-Developer Feature

#### The Situation
Building a user dashboard with multiple components:
- User profile section (Developer A)
- Activity timeline (Developer B)  
- Settings panel (Developer C)

#### Coordination Strategy

```bash
# Main coordination branch
git checkout -b feature/user-dashboard

# Each developer creates sub-branches
git checkout -b feature/user-dashboard-profile    # Developer A
git checkout -b feature/user-dashboard-timeline   # Developer B  
git checkout -b feature/user-dashboard-settings   # Developer C
```

#### Integration Process

```bash
# Developer A finishes first
git checkout feature/user-dashboard
git merge feature/user-dashboard-profile
git push origin feature/user-dashboard

# Developer B integrates their work
git checkout feature/user-dashboard
git pull origin feature/user-dashboard  # Get A's changes
git merge feature/user-dashboard-timeline
# Resolve any conflicts
git push origin feature/user-dashboard

# Developer C does the same
git checkout feature/user-dashboard
git pull origin feature/user-dashboard  # Get A+B's changes
git merge feature/user-dashboard-settings
# Resolve any conflicts
git push origin feature/user-dashboard

# Final integration testing
# Create PR from feature/user-dashboard to main
```

## 📋 Release Workflow

### Scenario: Preparing Version 2.1.0

#### Step 1: Release Planning
```
Milestone: Version 2.1.0
Target Date: March 15, 2024

Features Included:
✅ Forgot password functionality (#234)
✅ User dashboard (#267)
✅ Email notifications (#289)
✅ Mobile performance improvements (#301)

Bug Fixes:
✅ Login button mobile issue (#456)
✅ Email validation bug (#467)
✅ Safari compatibility issue (#478)
```

#### Step 2: Release Branch
```bash
# Create release branch from main
git checkout main
git pull origin main
git checkout -b release/v2.1.0

# Final testing and minor fixes only
git add release-notes.md
git commit -m "Add v2.1.0 release notes"

git add version.json
git commit -m "Bump version to 2.1.0"
```

#### Step 3: Testing & QA
```
Release Testing Checklist:
- [ ] All new features working
- [ ] No regression bugs
- [ ] Performance acceptable
- [ ] Mobile testing complete
- [ ] Cross-browser testing done
- [ ] Security scan passed
- [ ] Documentation updated
```

#### Step 4: Deploy to Production
```bash
# After QA approval
git checkout main
git merge release/v2.1.0
git tag v2.1.0
git push origin main
git push origin v2.1.0

# Automatic deployment triggered
# Release announcement sent
```

## 🎯 GitHub Best Practices in Action

### Branch Naming Convention
```
feature/description     → feature/user-dashboard
bugfix/description      → bugfix/mobile-login
hotfix/description      → hotfix/security-patch
release/version         → release/v2.1.0
```

### Commit Message Standards
```
type(scope): description

Examples:
feat(auth): add forgot password functionality
fix(mobile): resolve login button touch events
docs(readme): update installation instructions
style(css): fix button spacing on mobile
refactor(api): simplify user authentication logic
test(login): add unit tests for password reset
```

### PR Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
```

## 🔍 Troubleshooting Common Issues

### Merge Conflicts
```bash
# When git says there are conflicts
git status  # Shows conflicted files

# Edit files to resolve conflicts
# Look for:
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name

# After resolving
git add resolved-file.js
git commit -m "Resolve merge conflict in user authentication"
```

### Accidentally Committed to Wrong Branch
```bash
# Move last commit to correct branch
git reset --soft HEAD~1  # Undo commit, keep changes
git stash                 # Save changes temporarily
git checkout correct-branch
git stash pop            # Restore changes
git add .
git commit -m "Add feature to correct branch"
```

## 🎯 Key Takeaways for BAs

1. **Issues are your friend** - Use them to track all requirements and bugs
2. **Pull Requests ensure quality** - Code is reviewed before going live
3. **Branches enable parallel work** - Multiple features can be developed simultaneously
4. **Tags mark important milestones** - Easy to find specific releases
5. **Good communication is crucial** - Clear descriptions and reviews save time

## 🎯 Next Steps

Ready to learn the best practices? Let's cover the dos and don'ts:

👉 [Best Practices](../05-best-practices/README.md)