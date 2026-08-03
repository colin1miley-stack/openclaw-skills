# Content Authenticity Enforcer

**Version:** 1.0  
**Date:** 2026-08-03  
**Purpose:** Prevent credibility-damaging errors in public-facing content by enforcing that every biographical claim, credential, number, and story is verifiable against the author's fact base.

---

## When to Use This Skill

Use this skill BEFORE publishing any content that:
- Mentions personal history, career, or credentials
- Includes numbers (revenue, years of experience, deal sizes)
- Tells stories about past clients or employers
- Makes claims about results or outcomes

**Trigger phrases:** "fact check this", "is this authentic", "verify my story", "check against fact base", "pre-publish credibility check"

---

## The Authenticity Protocol

### Step 1: Load the Fact Base

**Required:** `COLIN-FACT-BASE.md` must exist and be current.

If it doesn't exist:
1. Create it by interviewing the subject
2. Record: dates, numbers, names, outcomes
3. Update before every major content push

### Step 2: Extract Claims from Content

Run this check on any draft:

```
For each sentence in the content:
  1. Does it contain a NUMBER? (years, €, %, dates)
  2. Does it mention a PERSON? (names, titles, roles)
  3. Does it mention an ORGANIZATION? (companies, schools)
  4. Does it tell a STORY? ("I once...", "A client...")
  5. Does it make a CLAIM? ("I helped...", "We achieved...")
```

### Step 3: Verify Each Claim

| Claim Type | Verification Method | Source |
|-----------|---------------------|--------|
| **Dates** | Check against timeline | CV, contracts, emails |
| **Numbers** | Check against records | Invoices, CRM, bank statements |
| **Names** | Check spelling, titles | LinkedIn, company directories |
| **Stories** | Confirm with subject | Voice memo, interview |
| **Results** | Check for evidence | Case studies, testimonials |

### Step 4: Flag Issues

| Severity | Action |
|----------|--------|
| 🔴 **CRITICAL** | Factually incorrect — remove or correct immediately |
| 🟡 **WARNING** | Unverifiable — add "to my knowledge" or remove |
| 🟢 **NOTE** | Minor discrepancy — flag for subject review |

---

## Example: Content Audit

**Draft:** "I managed a €20M portfolio at Vodafone for 12 years."

**Fact Base Check:**
- €20M portfolio? ❌ INCORRECT — was €10M
- 12 years at Vodafone? ❌ INCORRECT — was 10 years (Nov 2014–Nov 2024)

**Corrected:** "I managed a €10M portfolio at Vodafone for 10 years."

---

## Anti-Patterns

### Anti-Pattern 1: "Aspirational Truth"

**Symptom:** Writing what you HOPE to achieve as if you've already done it.
**Example:** "I help sales teams save 10 hours/week" (you've only done it for one team)
**Fix:** "I helped one sales team save 10 hours/week. Now I'm scaling that system."

### Anti-Pattern 2: "Rounded Up"

**Symptom:** "15 years" when it's 14. "€10M" when it's €9.2M.
**Fix:** Use exact numbers or ranges. "14 years" or "€9-10M portfolio."

### Anti-Pattern 3: "Implied Endorsement"

**Symptom:** Mentioning former employers as if they endorse your current work.
**Example:** "Former Vodafone executive launches AI consultancy"
**Fix:** "15 years in enterprise B2B sales" — no employer name needed.

### Anti-Pattern 4: "Vague Superlatives"

**Symptom:** "Best in class", "industry leading", "top performer" without evidence.
**Fix:** Quantify. "Top 10% of account managers by revenue growth" (if verifiable).

---

## Integration with Content Workflow

```
┌─────────────────────────────────────────┐
│  1. DRAFT CONTENT                       │
│     (Claude/Kimi writes first pass)     │
├─────────────────────────────────────────┤
│  2. CONTENT-AUTHENTICITY-ENFORCER       │
│     Verify every claim against fact base│
├─────────────────────────────────────────┤
│  3. BRAND-GUARDIAN                      │
│     Check tone, currency, descriptors   │
├─────────────────────────────────────────┤
│  4. COPY-EDITOR                         │
│     Fix grammar, flow, clarity          │
├─────────────────────────────────────────┤
│  5. COLIN (HUMAN)                       │
│     Final approval                      │
├─────────────────────────────────────────┤
│  6. PUBLISH                             │
│     LinkedIn, newsletter, site          │
└─────────────────────────────────────────┘
```

---

## Session Output

After running Content Authenticity Enforcer, create:

```
content-audit/[date]-[content-title]/
├── draft.md
├── fact-base-check.md
├── flagged-issues.md
└── corrected-draft.md
```

---

*Skill created: 2026-08-03*  
*Source: Post-incident review of June 30, 2026 LinkedIn post with fabricated details*
