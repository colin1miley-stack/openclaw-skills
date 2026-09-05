---
name: logto-icm
description: Open-source auth infrastructure (OIDC/OAuth/SAML), ICM-converted from logto-io/logto. Use when adding authentication, multi-tenancy, SSO, or RBAC to Signal, Revenue Systems, or client portals.
---

# Logto — ICM Converted

**Source:** `logto-io/logto` (AGPL-3.0 License)  
**Converted:** 2026-09-05  
**Purpose:** Auth infrastructure for Signal multi-user, Revenue Systems client portal, affiliate brand separation  

---

## ICM Layer Mapping

| Layer | File | Usage |
|-------|------|-------|
| **Layer 0** | `SOUL.md`, `AGENTS.md` | Security boundaries |
| **Layer 1** | Root `CONTEXT.md` | Route to auth stages |
| **Layer 2** | This SKILL.md | Task contract for auth setup |
| **Layer 3** | `skills/logto-icm/`, `references/` | Logto config, SDK patterns |
| **Layer 4** | `stages/06-auth-integration/output/` | Deployment configs, learnings |

---

## What Logto Provides

| Feature | Use Case |
|---------|----------|
| **OIDC/OAuth 2.1** | Standard auth flows for web apps |
| **SAML** | Enterprise SSO for client portals |
| **Social Login** | Google, LinkedIn, Twitter/X OAuth |
| **MFA** | SMS, TOTP, backup codes |
| **RBAC** | Role-based access (admin, creator, viewer) |
| **Multi-tenancy** | Separate organizations per brand |
| **MCP Ready** | Works with Model Context Protocol |

---

## Deployment Options

### Option 1: Logto Cloud (Fastest)
```
https://cloud.logto.io/ → Sign up → Create tenant → Done
Cost: Free tier available, then pay-per-MAU
```

### Option 2: Self-Hosted (Private)
```bash
# Docker Compose
curl -fsSL https://raw.githubusercontent.com/logto-io/logto/HEAD/docker-compose.yml | \
docker compose -p logto -f - up
```

### Option 3: Railway/Render (Managed)
```
Render: https://render.com/deploy?repo=https://github.com/logto-io/logto
Railway: Deploy from GitHub
```

---

## Brand Separation Architecture

```
Logto Tenant: "colinmiley"
├── Application: "signal-app"
│   └── Roles: admin, content_creator, viewer
├── Application: "revenue-systems-portal"
│   └── Roles: client, auditor, admin
├── Application: "affiliate-dashboard"
│   └── Roles: publisher, admin
└── Organizations:
    ├── "Colin Miley" (main)
    ├── "Revenue Ritual" (product)
    └── "AI Revenue Systems" (consulting)
```

---

## Integration with Signal

### Next.js + Logto SDK

```typescript
// app/api/auth/[...nextauth]/route.ts
import { handleAuth } from '@logto/next/server-actions';

export const GET = handleAuth({
  redirectUri: 'http://localhost:3000/callback',
  postSignOutRedirectUri: 'http://localhost:3000/',
});
```

### Signal Worker + Logto API

```typescript
// Verify JWT from Logto
import { jwtVerify } from 'jose';

async function verifyAuth(token: string) {
  const { payload } = await jwtVerify(
    token,
    await fetchLogtoJwks(),
    { issuer: 'https://your-logto.app/oidc' }
  );
  return payload;
}
```

---

## ICM Stage Contract

```json
{
  "stageName": "auth-integration",
  "branchName": "feat/logto-auth",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "au-001",
      "title": "Deploy Logto instance",
      "agent": "builder",
      "acceptance": ["Logto running", "Admin console accessible"],
      "estimatedTokens": 5000
    },
    {
      "id": "au-002",
      "title": "Create applications (Signal, Portal, Affiliate)",
      "agent": "builder",
      "acceptance": ["3 apps registered", "Redirect URIs configured"],
      "estimatedTokens": 5000
    },
    {
      "id": "au-003",
      "title": "Set up roles and organizations",
      "agent": "builder",
      "acceptance": ["Roles defined", "Orgs created", "User assigned"],
      "estimatedTokens": 5000
    },
    {
      "id": "au-004",
      "title": "Integrate into Signal Worker",
      "agent": "builder",
      "acceptance": ["Auth middleware", "JWT verification", "Route protection"],
      "estimatedTokens": 8000
    }
  ]
}
```

---

## Quality Gate

- [ ] Logto accessible at configured URL
- [ ] Sign-up flow works
- [ ] Sign-in flow works
- [ ] Social login (Google, LinkedIn) works
- [ ] Role-based route protection works
- [ ] JWT verification in Signal Worker
- [ ] Token refresh works
- [ ] Logout works

---

## Cost Estimate

| Option | Cost/Month |
|--------|-----------|
| Logto Cloud (free tier) | $0 (up to 5K MAU) |
| Logto Cloud (paid) | ~$0.01/MAU |
| Self-hosted (VPS) | ~$5-20/month |
| Railway/Render | ~$5-25/month |

---

*ICM-converted from logto-io/logto. See original repo for full documentation.*
