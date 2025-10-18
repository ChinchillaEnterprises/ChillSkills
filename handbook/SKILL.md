---
name: Handbook
description: Complete Chill Amplify Template handbook - AWS Amplify Gen 2 patterns, rules, and best practices for building production apps
trigger:
  - read the handbook
  - consult handbook
  - check handbook
  - handbook reference
---

# Chill Amplify Template Handbook

This skill provides comprehensive knowledge of the Chill Amplify Template development patterns, rules, and best practices.

## What This Skill Contains

All handbook documentation files are included as supporting resources in this skill directory. When activated, you have access to the complete handbook knowledge base without needing to read files from the project.

## How to Use This Skill

### 1. When to Activate

Activate this skill when:
- Starting any backend feature implementation
- Setting up authentication
- Implementing webhooks from external services
- Creating Lambda functions
- Designing database schemas
- Making architectural decisions
- Debugging deployment or configuration issues
- Unsure whether to use CDK or which pattern to follow

### 2. Core Principles (Always Follow)

**The "No CDK" Rule:**
- ❌ NEVER import from `aws-cdk-lib` for standard tasks
- ✅ ONLY use: `defineAuth`, `defineData`, `defineFunction`, `defineStorage`
- See: `AI-DEVELOPMENT-GUIDELINES.md`

**The "No API Gateway" Rule:**
- ❌ NEVER create API Gateway resources
- ✅ Internal operations: GraphQL custom mutations (see `NO-API-GATEWAY.md`)
- ✅ External webhooks: Next.js API routes (see `WEBHOOKS-GUIDE.md`)

**The "No Root Docs" Rule:**
- ❌ NO markdown files at project root except README.md
- ✅ ALL documentation belongs in `resources/handbook/`

### 3. Quick Reference - Finding Solutions

| User Needs | Start Here | Supporting Files |
|------------|------------|------------------|
| **Authentication setup** | `auth/README.md` → `auth/AUTH_PATTERNS.md` | CLIENT_SIDE_SETUP, SERVER_SIDE_SETUP, GOOGLE_OAUTH_SETUP |
| **Database/schema design** | `data/` folder | simple-custom-operations.ts, blog-schema.ts, ecommerce-schema.ts, TTL_PATTERN.md |
| **Backend API endpoint** | `NO-API-GATEWAY.md` | functions/graphqlResolver/ |
| **External webhooks** | `WEBHOOKS-GUIDE.md` → `webhooks/NEXTJS_API_ROUTES_PATTERN.md` | webhooks/examples/, API_KEY_AUTH_EXPLAINED.md |
| **Scheduled tasks** | `functions/scheduledFunction/` | BACKEND_ENV_CONFIG.md |
| **Lambda permissions** | `functions/LAMBDA_DYNAMODB_ACCESS.md` | AI-DEVELOPMENT-GUIDELINES.md |
| **Long operations (>30s)** | `ASYNC_PATTERNS.md` | webhooks/NEXTJS_API_ROUTES_PATTERN.md (buffer pattern) |
| **Secrets/API keys** | `functions/SECRETS_MANAGER_IAM.md` | ENVIRONMENT_VARIABLES.md |
| **ML models (heavy deps)** | `functions/DOCKER_LAMBDA_WITH_AMPLIFY_GEN2.md` | - |
| **Environment detection** | `BACKEND_ENV_CONFIG.md` | ENVIRONMENT_VARIABLES.md |
| **Frontend/design** | `frontend/README.md` | frontend/design/LIQUID_GLASS_DESIGN.md |
| **Troubleshooting** | `troubleshooting/README.md` | troubleshooting/deployment/, auth/troubleshooting/ |

### 4. File Organization

```
handbook/
├── SKILL.md                          ← YOU ARE HERE (instruction manual)
├── README.md                         ← Navigation hub with "I Need To..." table
│
├── AI-DEVELOPMENT-GUIDELINES.md      ← ⭐ MUST READ: Core rules
├── ASYNC_PATTERNS.md                 ← Bypassing 30-second timeout
├── BACKEND_ENV_CONFIG.md             ← Environment-aware backend code
├── ENVIRONMENT_VARIABLES.md          ← Passing env vars to Lambda
├── NO-API-GATEWAY.md                 ← Internal backend operations
├── WEBHOOKS-GUIDE.md                 ← External webhooks quick ref
├── QUICK_START.md                    ← Getting started guide
│
├── auth/                             ← Authentication
│   ├── README.md                     ├─ Auth navigation hub
│   ├── CLIENT_SIDE_SETUP.md          ├─ Client setup (enable-oauth-listener!)
│   ├── SERVER_SIDE_SETUP.md          ├─ Server-side auth
│   ├── HOW_AUTH_WORKS.md             ├─ Conceptual guide
│   ├── AUTH_PATTERNS.md              ├─ 5 production patterns
│   ├── TRIGGERS_GUIDE.md             ├─ Lambda triggers
│   ├── GOOGLE_OAUTH_SETUP.md         ├─ Google OAuth
│   ├── SECURITY_CHECKLIST.md         └─ Common mistakes
│   └── troubleshooting/
│
├── data/                             ← Database schemas
│   ├── README.md
│   ├── TTL_PATTERN.md                ├─ Auto-delete temp data
│   ├── simple-custom-operations.ts   ├─ Basic examples
│   ├── blog-schema.ts                ├─ Blog platform
│   └── ecommerce-schema.ts           └─ E-commerce
│
├── webhooks/                         ← Webhook handling
│   ├── README.md
│   ├── NEXTJS_API_ROUTES_PATTERN.md  ├─ ⭐ Main guide
│   ├── API_KEY_AUTH_EXPLAINED.md     ├─ Deep dive
│   ├── authorization-modes.md
│   ├── troubleshooting.md
│   └── examples/                     └─ Production code
│       ├── slack-events/
│       └── shared/
│
├── functions/                        ← Lambda functions
│   ├── DOCKER_LAMBDA_WITH_AMPLIFY_GEN2.md  ← ML workloads
│   ├── LAMBDA_DYNAMODB_ACCESS.md     ← DynamoDB access (NEVER CDK!)
│   ├── LAMBDA_FUNCTION_URLS.md       ← Public endpoints
│   ├── SECRETS_MANAGER_IAM.md        ← Secure credentials
│   ├── graphqlResolver/
│   ├── scheduledFunction/
│   ├── cognitoTrigger/
│   └── userTriggered/
│
├── troubleshooting/                  ← Debugging
│   ├── README.md
│   └── deployment/
│       └── PARCEL_WATCHER_ERROR.md
│
└── frontend/                         ← Client-side
    ├── README.md
    └── design/
        └── LIQUID_GLASS_DESIGN.md    └─ Design system
```

### 5. Common Workflows

**"I need to add authentication"**
1. Read: `auth/AUTH_PATTERNS.md`
2. Choose pattern based on app type (B2B SaaS → Pattern 3)
3. Check: `auth/SECURITY_CHECKLIST.md`
4. Implement: `auth/CLIENT_SIDE_SETUP.md` + `auth/SERVER_SIDE_SETUP.md`

**"External service needs to send webhooks"**
1. Read: `webhooks/NEXTJS_API_ROUTES_PATTERN.md`
2. Create: Next.js API route in project's `app/api/`
3. Use: `dataClient` with `authMode: 'apiKey'`
4. Schema: Add `allow.publicApiKey()`
5. Example: See `webhooks/examples/slack-events/`

**"I need a backend API endpoint"**
1. Read: `NO-API-GATEWAY.md`
2. Define: Custom mutation in `amplify/data/resource.ts`
3. Handler: Create Lambda in `amplify/functions/`
4. Call: `client.mutations.operationName()` from frontend

**"Lambda needs DynamoDB access"**
1. Read: `functions/LAMBDA_DYNAMODB_ACCESS.md`
2. Function: Add `resourceGroupName: 'data'`
3. Schema: Add `allow.resource(functionName)`
4. Done: No CDK, no IAM code

**"Operation takes longer than 30 seconds"**
1. Read: `ASYNC_PATTERNS.md`
2. Choose:
   - Pattern 1 (DynamoDB Stream) for frontend with real-time updates
   - Pattern 2 (Lambda Self-Invoke) for webhooks
3. Implement based on guide examples

### 6. Critical Discoveries (Prevent Hours of Debugging)

**OAuth UI Not Updating:**
```typescript
// THIS LINE IS REQUIRED
import "aws-amplify/auth/enable-oauth-listener";
```
See: `auth/CLIENT_SIDE_SETUP.md`

**Lambda "Malformed Environment Variables" Error:**
- You're probably trying to pass DynamoDB table name as env var
- Solution: Use `resourceGroupName: 'data'` instead
- See: `functions/LAMBDA_DYNAMODB_ACCESS.md`

**Webhook Returns 401 Unauthorized:**
- Missing `allow.publicApiKey()` in schema
- Missing `apiKeyAuthorizationMode` in defineData
- See: `webhooks/NEXTJS_API_ROUTES_PATTERN.md`

**Glass Effect Looks Flat:**
- Use WHITE borders (not black!) at low opacity
- Must have gradient or image behind glass
- See: `frontend/design/LIQUID_GLASS_DESIGN.md`

**Docker Lambda Build Fails:**
- Need custom CodeBuild image
- Must start Docker daemon in amplify.yml
- See: `functions/DOCKER_LAMBDA_WITH_AMPLIFY_GEN2.md`

### 7. Decision Trees

**Need HTTP endpoint?**
```
Is it for internal operations (user-triggered, authenticated)?
  → YES: GraphQL custom mutation (NO-API-GATEWAY.md)
  → NO: Is it for external webhooks (Slack, Stripe)?
      → YES: Next.js API route (WEBHOOKS-GUIDE.md)
      → NO: Consider Lambda Function URL (functions/LAMBDA_FUNCTION_URLS.md)
```

**Need Lambda to access AWS resource?**
```
Is it DynamoDB from Amplify Data?
  → YES: resourceGroupName: 'data' (LAMBDA_DYNAMODB_ACCESS.md)
  → NO: Is it S3, Cognito, or other Amplify resource?
      → YES: Use appropriate resourceGroupName
      → NO: Need CDK escape hatch? Ask first, usually unnecessary
```

**Need to store secrets?**
```
Is it for local sandbox testing?
  → YES: npx ampx sandbox secret set (QUICK_START.md)
  → NO: Is it for production deployment?
      → YES: AWS Secrets Manager (SECRETS_MANAGER_IAM.md)
```

**Operation takes > 30 seconds?**
```
Is it user-facing with progress updates?
  → YES: DynamoDB Stream pattern (ASYNC_PATTERNS.md Pattern 1)
  → NO: Is it a webhook that needs fast acknowledgment?
      → YES: Lambda Self-Invoke pattern (ASYNC_PATTERNS.md Pattern 2)
```

### 8. How to Navigate

**Start Point:**
- Always begin with `README.md` - it has the comprehensive "I Need To..." table

**Finding Specific Info:**
- Use the file structure above to locate the right guide
- Most folders have their own README.md as a navigation hub

**Reading Order for New Features:**
1. Find feature in "I Need To..." table
2. Read the primary guide listed
3. Check related/supporting files as needed
4. Verify against security/troubleshooting guides

**For AI Assistants:**
- Reference specific handbook files when explaining solutions
- Copy proven patterns from examples
- Always check AI-DEVELOPMENT-GUIDELINES.md before suggesting CDK
- Use decision trees to guide implementation choices

### 9. Progressive Disclosure

This skill uses progressive disclosure - you don't need to read every file upfront. Instead:

1. Start with this SKILL.md (overview + navigation)
2. Read the specific guide for the task at hand
3. Dive into supporting files only when needed
4. Reference examples when implementing

The handbook is designed for quick lookups, not sequential reading.

### 10. Remember

- **No CDK for standard tasks** - Use Amplify abstractions
- **No API Gateway ever** - GraphQL or Next.js API routes
- **Copy patterns, don't reinvent** - Examples are production-tested
- **Check security checklists** - Cognito custom attributes are permanent!
- **Default to sandbox-safe** - Prevent duplicate operations

---

When this skill is active, treat the handbook files in this directory as your primary source of truth for the Chill Amplify Template's patterns and best practices.
