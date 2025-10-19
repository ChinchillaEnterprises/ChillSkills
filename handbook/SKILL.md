---
name: handbook
description: Complete Chill Amplify Template handbook - AWS Amplify Gen 2 patterns, rules, and best practices for building production apps (project, gitignored)
---

# Handbook Skill - Learn Before You Code

## Purpose

This skill instructs you (Claude) to **educate yourself from the project's handbook BEFORE implementing any code** in an Amplify Gen 2 project.

## Three Modes of Operation

### Mode 1: **"Read handbook"** - Learning Mode
When user says things like:
- "Read the handbook"
- "Check the handbook"
- "Consult the handbook"
- Or asks you to implement Amplify Gen 2 code

**What to do:** Read `resources/handbook/` locally to learn correct patterns before coding.

### Mode 2: **"Sync/Update handbook"** - Pull from Source
When user says things like:
- "Sync the handbook"
- "Update the handbook from the template"
- "Pull latest handbook"
- "Refresh the handbook"

**What to do:** Pull latest handbook from chill-amplify-template GitHub repo and replace local copy.

### Mode 3: **"Push handbook changes"** - Push to Source
When user says things like:
- "Push this handbook change"
- "Update the handbook with this new pattern"
- "Commit this to the template"
- "Add this to the handbook repo"

**What to do:** Push local handbook changes back to chill-amplify-template GitHub repo.

---

## Mode 1: Learning Mode (Read Handbook)

### Core Principle: No Hallucinations, Only Handbook Patterns

When a user asks you to implement anything in an Amplify Gen 2 project:

1. **STOP** - Don't immediately write code
2. **CHECK** - Does `resources/handbook/` exist in the current project?
3. **LEARN** - Read the handbook to understand the correct patterns
4. **THEN CODE** - Implement using handbook patterns, not generic approaches

### The Workflow

#### Step 1: Check for Handbook
```bash
# Is there a handbook in this project?
ls resources/handbook/README.md
```

If it exists, you have access to the complete Chill Amplify Template handbook.

#### Step 2: Start with Table of Contents
**ALWAYS begin here:**
- Read `resources/handbook/README.md`
- This contains the full table of contents
- Understand what sections exist (Auth, Functions, Data, Webhooks, etc.)
- Treat it like a textbook - respect the organization

#### Step 3: Read Quick Start
- Read `resources/handbook/QUICK_START.md`
- Understand the basics and philosophy of this template
- Get context on how everything fits together

#### Step 4: Navigate to Relevant Section
Based on what the user wants to build:

| User Wants To Build | Read This Section |
|---------------------|-------------------|
| **DynamoDB model/schema** | `resources/handbook/data/` |
| **Scheduled Lambda function** | `resources/handbook/functions/scheduledFunction/` |
| **GraphQL resolver** | `resources/handbook/functions/graphqlResolver/` |
| **Webhook handler** | `resources/handbook/webhooks/` |
| **Google OAuth authentication** | `resources/handbook/auth/GOOGLE_OAUTH_SETUP.md` |
| **Cognito triggers** | `resources/handbook/auth/TRIGGERS_GUIDE.md` |
| **Environment variables** | `resources/handbook/ENVIRONMENT_VARIABLES.md` |
| **Docker Lambda** | `resources/handbook/functions/DOCKER_LAMBDA_WITH_AMPLIFY_GEN2.md` |
| **Long-running operations (>30s)** | `resources/handbook/ASYNC_PATTERNS.md` |

#### Step 5: Learn the Pattern
- Read the guide completely
- Understand the Amplify Gen 2 way to do it
- Note any security considerations
- Check for common pitfalls
- Look at examples if available

#### Step 6: Implement the Code
NOW you write the code, following the handbook patterns you just learned.

### Critical Rules (Always Follow)

#### The "No CDK" Rule
- ❌ NEVER import from `aws-cdk-lib` for standard tasks
- ✅ ONLY use: `defineAuth`, `defineData`, `defineFunction`, `defineStorage`
- See: `resources/handbook/AI-DEVELOPMENT-GUIDELINES.md`

#### The "No API Gateway" Rule
- ❌ NEVER create API Gateway resources
- ✅ Internal operations: GraphQL custom mutations
- ✅ External webhooks: Next.js API routes
- See: `resources/handbook/NO-API-GATEWAY.md`

#### The "Learn First" Rule
- ❌ Don't write code based on generic knowledge or hallucinations
- ✅ Read the handbook section first, THEN code
- This prevents using wrong patterns or outdated approaches

---

## Mode 2: Sync Handbook (Pull from GitHub)

### When to Use
User wants to pull the latest handbook from the template repo to update their local copy.

### Configuration
- **GitHub Repo:** `ChinchillaEnterprises/chill-amplify-template`
- **GitHub Token:** Stored in AWS Secrets Manager as `github-token`
- **Source Path:** `resources/handbook/` (in template repo)
- **Destination:** `resources/handbook/` (in current project)

### Command Workflow

```bash
# Step 1: Get GitHub token from AWS Secrets Manager
GITHUB_TOKEN=$(aws secretsmanager get-secret-value --secret-id github-token --query SecretString --output text | jq -r '.token')

# Step 2: Get list of all files in handbook directory from GitHub
# Use GitHub API to recursively list all files
curl -s -H "Authorization: Bearer ${GITHUB_TOKEN}" \
  "https://api.github.com/repos/ChinchillaEnterprises/chill-amplify-template/git/trees/main?recursive=1" \
  | jq -r '.tree[] | select(.path | startswith("resources/handbook/")) | select(.type == "blob") | .path' \
  > /tmp/handbook_files.txt

# Step 3: Create temp directory for download
mkdir -p /tmp/handbook_sync

# Step 4: Download each file
while IFS= read -r file_path; do
  # Extract just the handbook-relative path (remove resources/handbook/ prefix)
  relative_path="${file_path#resources/handbook/}"

  # Create directory structure
  mkdir -p "/tmp/handbook_sync/$(dirname "$relative_path")"

  # Download file content (base64 encoded) and decode
  curl -s -H "Authorization: Bearer ${GITHUB_TOKEN}" \
    "https://api.github.com/repos/ChinchillaEnterprises/chill-amplify-template/contents/$file_path" \
    | jq -r '.content' | base64 --decode > "/tmp/handbook_sync/$relative_path"

  echo "✅ Downloaded: $relative_path"
done < /tmp/handbook_files.txt

# Step 5: Backup current handbook (optional but recommended)
if [ -d "resources/handbook" ]; then
  mv resources/handbook "resources/handbook.backup.$(date +%Y%m%d_%H%M%S)"
  echo "📦 Backed up existing handbook"
fi

# Step 6: Replace with new handbook
mkdir -p resources/handbook
cp -r /tmp/handbook_sync/* resources/handbook/

# Step 7: Cleanup
rm -rf /tmp/handbook_sync /tmp/handbook_files.txt

echo "✅ Handbook synced successfully from template repo!"
```

### Success Message
After syncing, tell the user:
```
✅ Handbook synced from chill-amplify-template!
   - Pulled latest from: https://github.com/ChinchillaEnterprises/chill-amplify-template
   - Updated: resources/handbook/
   - Old version backed up to: resources/handbook.backup.[timestamp]
```

---

## Mode 3: Push Handbook Changes (Push to GitHub)

### When to Use
User has created or updated handbook files locally and wants to push them to the template repo.

### Configuration
- **GitHub Repo:** `ChinchillaEnterprises/chill-amplify-template`
- **GitHub Token:** Stored in AWS Secrets Manager as `github-token`
- **Source:** `resources/handbook/` (in current project)
- **Destination Path:** `resources/handbook/` (in template repo)

### Command Workflow

```bash
# Step 1: Get GitHub token from AWS Secrets Manager
GITHUB_TOKEN=$(aws secretsmanager get-secret-value --secret-id github-token --query SecretString --output text | jq -r '.token')

# Step 2: Determine which files to push
# Ask user or detect changed files
# For this example, let's say user specifies: "resources/handbook/webhooks/NEW_PATTERN.md"

FILE_TO_PUSH="resources/handbook/webhooks/NEW_PATTERN.md"

# Extract the path relative to repo root
REPO_PATH="$FILE_TO_PUSH"

# Step 3: Check if file exists on GitHub (to get SHA if updating)
RESPONSE=$(curl -s -H "Authorization: Bearer ${GITHUB_TOKEN}" \
  "https://api.github.com/repos/ChinchillaEnterprises/chill-amplify-template/contents/$REPO_PATH")

# Check if file exists (has a sha)
FILE_SHA=$(echo "$RESPONSE" | jq -r '.sha // empty')

# Step 4: Base64 encode the file content
CONTENT=$(base64 -i "$FILE_TO_PUSH")

# Step 5: Prepare commit message
# Ask user for commit message or generate one
COMMIT_MESSAGE="docs: Add $(basename "$FILE_TO_PUSH") to handbook"

# Step 6: Push to GitHub
if [ -n "$FILE_SHA" ]; then
  # File exists - update it
  curl -X PUT \
    -H "Authorization: Bearer ${GITHUB_TOKEN}" \
    -H "Accept: application/vnd.github+json" \
    "https://api.github.com/repos/ChinchillaEnterprises/chill-amplify-template/contents/$REPO_PATH" \
    -d "{\"message\":\"$COMMIT_MESSAGE\",\"content\":\"$CONTENT\",\"sha\":\"$FILE_SHA\"}"
  echo "✅ Updated: $REPO_PATH"
else
  # File doesn't exist - create it
  curl -X PUT \
    -H "Authorization: Bearer ${GITHUB_TOKEN}" \
    -H "Accept: application/vnd.github+json" \
    "https://api.github.com/repos/ChinchillaEnterprises/chill-amplify-template/contents/$REPO_PATH" \
    -d "{\"message\":\"$COMMIT_MESSAGE\",\"content\":\"$CONTENT\"}"
  echo "✅ Created: $REPO_PATH"
fi
```

### Detecting Which Files to Push

**Option 1: User specifies**
```
User: "Push resources/handbook/webhooks/RATE_LIMITING.md to the template"
```

**Option 2: Auto-detect new/changed files**
```bash
# Compare with template to find differences
# (This requires more complex logic - best to ask user)
```

### Success Message
After pushing, tell the user:
```
✅ Handbook changes pushed to template repo!
   - File: resources/handbook/webhooks/NEW_PATTERN.md
   - Commit: "docs: Add NEW_PATTERN.md to handbook"
   - View: https://github.com/ChinchillaEnterprises/chill-amplify-template/blob/main/resources/handbook/webhooks/NEW_PATTERN.md
```

---

## Example Scenarios

### Scenario 1: Learning Mode
```
User: "Build a scheduled function that syncs data every hour"

You:
1. Read: resources/handbook/functions/scheduledFunction/README.md
2. Learn:
   - Use schedule expression in defineFunction
   - Use resourceGroupName: 'data' for DynamoDB access
   - How to handle environment detection
3. Implement: Following the exact pattern from the handbook
4. Result: Code that works first try ✅
```

### Scenario 2: Sync Mode
```
User: "Update the handbook from the template"

You:
1. Get GitHub token from Secrets Manager
2. Pull all files from resources/handbook/ in template repo
3. Backup current local handbook
4. Replace with latest version
5. Result: Local handbook now matches template ✅
```

### Scenario 3: Push Mode
```
User: "I just created a new rate limiting pattern. Push it to the template."

You:
1. Ask: "Which file should I push?"
2. User: "resources/handbook/webhooks/RATE_LIMITING.md"
3. Get GitHub token from Secrets Manager
4. Base64 encode the file
5. Push to template repo via GitHub API
6. Result: New pattern now in template for all projects ✅
```

---

## What This Prevents

❌ Using outdated Amplify Gen 1 patterns
❌ Importing unnecessary CDK libraries
❌ Creating API Gateway resources (not needed)
❌ Manually writing IAM policies (handled by Amplify)
❌ Making up APIs that don't exist
❌ Generic Lambda code that doesn't integrate properly
❌ Manual copy-paste of handbook files
❌ Handbook docs going out of sync across projects

✅ Using correct Amplify Gen 2 abstractions
✅ Following proven, production-tested patterns
✅ Writing code that works first try
✅ Proper integration with Amplify resources
✅ Easy handbook updates across all projects
✅ Version-controlled handbook improvements

---

## Navigation Aid

The handbook README.md has a comprehensive "I Need To..." table that maps user needs to specific documentation. Use it as your guide.

## Remember

**This skill has three purposes:**

1. **Learning** - Read local handbook before coding
2. **Syncing** - Pull latest handbook from template
3. **Contributing** - Push handbook improvements back to template

Every time you're about to write Amplify Gen 2 code:
1. Check for `resources/handbook/`
2. Read the relevant section
3. Learn the pattern
4. Then code

This ensures you implement things the correct way, following the template's established patterns and best practices.
