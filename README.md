# Chill Skills

**Claude Code skills for Amplify Gen 2 development**

This repository contains custom Claude Code skills used across Chinchilla Enterprises projects.

---

## Skills Included

### 1. `handbook` - Amplify Gen 2 Developer Handbook

Complete documentation for building production-ready apps with AWS Amplify Gen 2 and Next.js.

**What's inside:**
- Authentication patterns (Google OAuth, auto-confirm, security)
- Data modeling (DynamoDB, GraphQL schemas, TTL patterns)
- Lambda functions (DynamoDB access, scheduled functions, triggers)
- Webhooks (Slack, Stripe, GitHub integrations)
- Frontend patterns (MUI components, design systems)
- Troubleshooting guides
- Production patterns and best practices

**How to use:**
```
Call the "handbook" skill in Claude Code to access documentation
```

### 2. `handbook-updater` - Handbook Maintenance

Expert knowledge on how to properly maintain and update the handbook.

**What it does:**
- Guides Claude on where to create new documentation
- Enforces naming conventions and formatting
- Updates navigation tables automatically
- Handles git workflow (commit and push changes)

**How to use:**
```
Activate when adding new patterns or updating existing docs
```

### 3. `webapp-testing` - Web Application Testing

Automated testing toolkit for web applications using Playwright and Python.

**What it does:**
- Test local web applications with UI verification
- Automate browser interactions (clicks, form fills, navigation)
- Debug UI behavior and capture screenshots
- Inspect page elements and identify selectors

**How to use:**
```
Use the webapp-testing skill to test your web app, find selectors, or automate interactions
```

---

## Installation

### First-Time Setup

1. **Backup existing skills (if any):**
   ```bash
   mv ~/.claude/skills ~/.claude/skills-backup
   ```

2. **Clone this repo as your skills directory:**
   ```bash
   git clone https://github.com/ChinchillaEnterprises/ChillSkills.git ~/.claude/skills
   ```

3. **Verify installation:**
   ```bash
   ls -la ~/.claude/skills/
   # Should see: handbook/, handbook-updater/, and webapp-testing/
   ```

4. **Test in Claude Code:**
   - Open Claude Code
   - Type "use the handbook skill to show me auth patterns"
   - Should work!

---

## Getting Updates

When teammates add new documentation or patterns:

```bash
cd ~/.claude/skills/
git pull
```

That's it! Updates are instant since this directory IS the git repo.

---

## Contributing Updates

### When You Discover a New Pattern

1. **Use the handbook-updater skill:**
   ```
   "Add this to the handbook" or "update the handbook with this pattern"
   ```

2. **Claude will:**
   - Create/update documentation files
   - Update navigation tables
   - Commit changes
   - Push to GitHub automatically

3. **Tell your teammates:**
   - Let them know to `git pull` for the latest docs

### Manual Updates (if needed)

If you need to manually update:

```bash
cd ~/.claude/skills/

# Make your changes to handbook files

# Commit and push
git add .
git commit -m "docs: [describe what you added]"
git push
```

---

## How This Works

### Source of Truth Model

```
~/.claude/skills/ (your local)
        ↕
GitHub (ChinchillaEnterprises/ChillSkills)
        ↕
~/.claude/skills/ (teammates' local)
```

- Everyone's `~/.claude/skills/` directory IS a git clone
- Updates flow through GitHub
- No copying files - direct git workflow
- All projects can READ from skills via Claude Code
- All team members can CONTRIBUTE new patterns

### Why This is Powerful

**Before:**
- Each project has its own outdated `resources/handbook/` folder
- Documentation gets stale
- Hard to share knowledge

**Now:**
- One canonical handbook in `~/.claude/skills/handbook/`
- All projects access the same docs via the skill
- Team members contribute patterns from any project
- Knowledge compounds over time

---

## File Structure

```
~/.claude/skills/
├── README.md                           # This file
├── handbook/                           # Main documentation skill
│   ├── SKILL.md                        # Skill configuration
│   ├── README.md                       # Handbook navigation hub
│   ├── AI-DEVELOPMENT-GUIDELINES.md    # Core rules
│   ├── ASYNC_PATTERNS.md               # Timeout solutions
│   ├── auth/                           # Authentication guides
│   │   ├── README.md
│   │   ├── AUTO_CONFIRM_SETUP.md
│   │   ├── AUTH_PATTERNS.md
│   │   ├── CLIENT_SIDE_SETUP.md
│   │   └── ...
│   ├── data/                           # Database patterns
│   ├── functions/                      # Lambda patterns
│   ├── webhooks/                       # Webhook guides
│   ├── frontend/                       # Client-side patterns
│   └── troubleshooting/                # Debug guides
├── handbook-updater/                   # Maintenance skill
│   └── SKILL.md                        # Update workflow
└── webapp-testing/                     # Web app testing skill
    ├── SKILL.md                        # Skill configuration
    ├── with_server.py                  # Server lifecycle management
    ├── example_script.py                # Example Playwright script
    └── ...                             # Additional testing resources
```

---

## Troubleshooting

### "Permission denied" when pushing

You need write access to the GitHub repo. Contact the repo admin.

### "Divergent branches" error

Someone else pushed changes. Pull first:
```bash
cd ~/.claude/skills/
git pull
git push
```

### Merge conflicts

If you and a teammate edited the same file:
```bash
cd ~/.claude/skills/
git pull
# Resolve conflicts in the files
git add .
git commit -m "docs: Resolve merge conflicts"
git push
```

### Skills not showing up in Claude Code

1. Verify installation:
   ```bash
   ls -la ~/.claude/skills/handbook/SKILL.md
   ```

2. Restart Claude Code

3. Check skill syntax in SKILL.md files

---

## Best Practices

1. **Pull before making changes:**
   ```bash
   cd ~/.claude/skills/
   git pull
   ```

2. **Use descriptive commit messages:**
   ```bash
   git commit -m "docs: Add webhook signature verification guide"
   ```

3. **Update navigation when adding files:**
   - Update folder README.md
   - Update main handbook README.md
   - Add cross-references

4. **Test changes locally before pushing:**
   - Use the handbook skill to verify docs are readable
   - Check that links work

5. **Communicate with team:**
   - Post in Slack when adding major patterns
   - Let teammates know to pull updates

---

## Questions?

Ask in the #mcp-tools Slack channel or tag the team.

---

**Happy documenting!** 🚀
