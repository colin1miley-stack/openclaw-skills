---
name: build-orchestrator
version: "1.0"
description: "Meta-skill for building any deliverable (website, content, audit, tool) with data-driven decisions, skill selection, multi-agent orchestration, and zero-tolerance quality gates. Never builds from assumptions. Always researches first, then designs, then builds, then verifies."
argument-hint: 'build-orchestrator --deliverable website --domain ai-revenue-systems --type landing-page'
allowed-tools: Read, Write, Edit, Exec, kimi_search, sessions_spawn
homepage: https://github.com/colin1miley-stack/openclaw-skills
author: AI Revenue Systems
license: MIT
user-invocable: true
metadata:
  openclaw:
    emoji: "🏗️"
    tags:
      - orchestration
      - meta-skill
      - multi-agent
      - build
      - quality
---

# Build Orchestrator

**Purpose:** Eliminate blind building. Every deliverable is researched, designed, orchestrated, and verified before shipping.

**Core principle:** If you don't have the data, you don't build. You research first.

---

## Phase 0: CONTEXT ACQUISITION (Never Skipped)

### Step 0.1: Check What You Already Know

Read these files IN ORDER:
1. `USER.md` — Who is this for? What's their business?
2. `MEMORY.md` — What decisions were made? What lessons learned?
3. `SOUL.md` — Who am I? What's my voice?
4. `AGENTS.md` — Security rules, safety constraints
5. `TOOLS.md` — What's in my toolkit?
6. `SKILL.md` files — **What skills are available? READ ALL RELEVANT ONES BEFORE PROCEEDING.**

**If ANY of these are missing → Ask the user OR infer from available data.**

### Step 0.2: MANDATORY SKILL READING (NEW — Non-Negotiable)

**Before any build, you MUST read all relevant skills.**

**Why this matters:** Every error from the 2026-08-02 session traced back to not reading `design-taste-frontend` before building. Skills contain rules that prevent slop.

**How to identify relevant skills:**

| Task Type | Required Skills | Also Check |
|-----------|----------------|------------|
| **Landing page / website** | `design-taste-frontend`, `frontend-design-anthropic` | `emil-design-eng`, `copywriting`, `cta-component` |
| **Copywriting** | `ai-voice-humanizer`, `copywriting` | `content-authenticity-enforcer` |
| **Any data collection** | `web-compliance-gate` | `regulatory-compliance-scanner` |
| **Analytics setup** | `analytics-tracking` | `ai-traffic-tracking` |
| **Video content** | `video-use`, `OpenMontage` | `emil-design-eng` |
| **Claude mode copy** | `claude-mode-orchestrator` | `copywriting`, `ai-voice-humanizer` |
| **SEO** | `seo-core-web-vitals`, `seo-meta-description` | `seo-open-graph`, `seo-sitemap` |
| **Email capture** | `web-compliance-gate` | `email-marketing`, `newsletter-signup` |

**Skill Reading Log (Create This File):**

Before building, create `skill-reading-log.md`:

```markdown
# Skill Reading Log — [Project Name]

## Skills Read
| # | Skill | Key Takeaways | Applied? |
|---|-------|--------------|----------|
| 1 | design-taste-frontend | No 3 equal cards, max 1 eyebrow/3 sections, no em-dashes | ✅ |
| 2 | emil-design-eng | Unseen details compound, beauty is leverage | ✅ |
| 3 | [skill] | [takeaway] | [status] |

## Design Read
"Reading this as: [page kind] for [audience], with a [vibe] language, leaning toward [system]."

## Dials
- DESIGN_VARIANCE: [1-10]
- MOTION_INTENSITY: [1-10]
- VISUAL_DENSITY: [1-10]

## Violations to Avoid
- [ ] Rule 1
- [ ] Rule 2
```

**Failure to create this log = build does not start.**

### Step 0.2: Identify Information Gaps

For every build task, ask:

| Gap Category | Question | If Unknown |
|-------------|----------|-----------|
| **Brand** | What's the brand voice? What's the visual identity? | Read brand docs OR ask user |
| **Audience** | Who is the customer? What do they care about? | Read customer profiles OR research market |
| **Market** | What do competitors do? What's working? | Research top 5 competitors |
| **Data** | What claims need evidence? | Source or flag as opinion |
| **Constraints** | Budget? Timeline? Technical limits? | Ask user |
| **Existing** | Is this a rebuild or greenfield? | Check file system |
| **Legal** | What compliance requirements? | Read compliance skills |

**Rule: If >3 gaps are unresolvable without user input → STOP. Ask the user.**

### Step 0.3: Research What You Don't Know

**For market/competitor data:**
```
kimi_search: "[industry] landing page design trends 2026"
kimi_search: "[competitor name] website design conversion"
kimi_search: "B2B [industry] best performing landing pages"
```

**For design/data-driven decisions:**
```
last30days: "[topic]" (if social pulse needed)
web_fetch: "https://example.com" (if competitor site analysis needed)
```

**What to extract from competitor research:**
- Headline patterns (question vs statement vs promise)
- CTA placement and wording
- Trust signals used (logos, stats, testimonials)
- Pricing presentation (anchoring, tiers, guarantees)
- Color psychology (what the industry uses)
- Content depth (short vs long form)

---

## Phase 1: SKILL SELECTION + COST ESTIMATION

### Step 1.1: Match Task to Skills

| Task Type | Primary Skills | Secondary Skills |
|-----------|---------------|-----------------|
| **Website (landing page)** | `design-taste-frontend`, `frontend-design-anthropic` | `copywriting`, `conversion-optimization`, `web-compliance-gate` |
| **Website (redesign)** | `design-taste-frontend` (Section 11), `frontend-design-anthropic` | `verification-before-completion` |
| **Content (blog/article)** | `article-content`, `ai-voice-humanizer` | `content-optimization`, `eeat-signals` |
| **Email sequence** | `email-sequence`, `copywriting` | `email-marketing` |
| **Consulting deliverable** | `audit-pipeline` | `rate-limit-guard`, `budget-gate` |
| **Social media** | `content-repurposing-engine`, `ai-voice-humanizer` | `content-scheduler` |
| **Brand identity** | `brand-identity-research`, `brandkit` | `about-page-generator` |
| **Video content** | `video-use`, `OpenMontage` | `emil-design-eng` |
| **Analytics setup** | `analytics-tracking` | `ai-traffic-tracking` |
| **Content with Claude modes** | `claude-mode-orchestrator` | `copywriting`, `ai-voice-humanizer` |
| **Any data collection** | `web-compliance-gate` | `regulatory-compliance-scanner` (if health) |

### Step 1.2: Cost Estimation + Quality Selection

**CRITICAL: Present options to user. Never assume budget constraints.**

For every build, estimate three tiers:

```markdown
## Build Cost Options

| Tier | Models | Est. Cost | Output Quality | Best For |
|------|--------|-----------|---------------|----------|
| **Essential** | Kimi K2p6 only | €0.50-1.00 | Good, competent, "professional" | Internal tools, MVPs, quick tests |
| **Professional** | Kimi + Claude (design/copy/review) | €1.00-1.50 | Very good, strategic, polished | Most client work, standard deliverables |
| **Portfolio** | Claude Sonnet 5 for all creative + review | €3.00-5.00 | Exceptional, creative, memorable | Portfolio pieces, high-stakes, "best first time" |

**User must explicitly select tier before proceeding.**
**Default: STOP and ask. Never assume.**
```

### Step 1.3: Read Selected Skills

**Before ANY work, read the full SKILL.md for every selected skill.**

**This is non-negotiable.** Every error from 2026-08-02 session traced back to:
- Not reading `design-taste-frontend` before redesigning
- Not reading `verification-before-completion` before claiming done
- Not reading `frontend-design-anthropic` before design decisions

---

## Phase 2: DESIGN INTELLIGENCE (Data-Driven, Not Preference-Driven)

### Step 2.1: Extract Design Signals from Research

**From competitor research, extract:**

| Signal | What to Look For | Why It Matters |
|--------|-----------------|---------------|
| **Color dominance** | What colors do top 5 competitors use? | Differentiation or category conformity |
| **Layout patterns** | Single column vs multi-column? | Mobile-first vs desktop-first audience |
| **Content length** | Short (above fold) vs long (scroll)? | Complexity of decision |
| **Social proof type** | Logos, testimonials, stats, case studies? | What converts in this industry |
| **Pricing presentation** | Upfront vs gated? Anchored vs standalone? | Price sensitivity |
| **CTA style** | Button text, color, placement | Conversion optimization |
| **Tone** | Formal, casual, technical, aspirational? | Audience expectations |

### Step 2.2: Make Design Decisions with Evidence

**BAD:** "I think dark mode looks premium."

**GOOD:** "Top 3 competitors in B2B sales consulting use dark backgrounds (Lavender.ai, Copy.ai, Jasper). Conversion studies show dark mode reduces eye strain for professional audiences. The user's brand already uses dark theme (established in previous build). Decision: preserve dark theme, evolve typography."

**Every design decision must cite:**
- Market evidence (what competitors do)
- Conversion evidence (what studies show)
- Brand constraint (what the user already established)
- OR explicit user preference (documented in USER.md or stated)

### Step 2.3: Create Design Specification Document

Before writing code, produce:

```markdown
# Design Spec: [Project Name]

## Research Summary
- Competitors analyzed: [list]
- Key findings: [3-5 bullets]
- Differentiation strategy: [1 sentence]

## Design Decisions
| Element | Decision | Evidence |
|---------|----------|----------|
| Color scheme | [Dark + amber accent] | User brand established; competitors use similar |
| Typography | [Inter + JetBrains Mono] | Design taste skill recommends; legibility studies |
| Layout | [Single column, scroll] | B2B consulting standard; mobile-first |
| Content depth | [8 sections, ~3000 words] | Complex service needs explanation |
| CTA strategy | [Lead magnet + direct booking] | Industry standard for €2K+ services |

## Content Map
| Section | Purpose | Key Message |
|---------|---------|-------------|
| Hero | Hook + CTA | "10 hours back every week" |
| ... | ... | ... |

## Technical Stack
- Framework: Next.js 14
- Styling: Tailwind CSS
- Animation: Framer Motion + GSAP
- Icons: Lucide React

## Compliance Requirements
- GDPR: Email capture needs consent checkbox
- Accessibility: WCAG 2.1 AA
- ...

## Quality Gates
- [ ] Currency consistency (€)
- [ ] No placeholders
- [ ] All links verified
- [ ] Build passes
- [ ] Accessibility check
```

---

## Phase 3: MULTI-AGENT ORCHESTRATION

### Step 3.1: Define the Graph

Every build is a graph. Define it before executing.

```
[Design Spec Complete]
   ↓
[Router: What needs building?]
   ├──(if: new component)──▶ [Component Agent]
   ├──(if: copy/content)──▶ [Copy Agent]
   ├──(if: visual asset)──▶ [Asset Agent]
   └──(if: integration)───▶ [Integration Agent]
   ↓
[All parallel work complete]
   ↓
[Integration Agent] → Assemble
   ↓
[Quality Gate] → Verify
   ↓
[Compliance Gate] → Legal check
   ↓
[Deploy Gate] → Deploy
```

### Step 3.2: Agent Definitions

| Agent | Responsibility | Input | Output | Tools |
|-------|---------------|-------|--------|-------|
| **Research Agent** | Gather market data, competitor analysis, trends | Topic/domain | Research report | kimi_search, last30days, web_fetch |
| **Design Agent** | Create design spec, wireframes, visual system | Research + brand constraints | Design spec document | design-taste-frontend, frontend-design-anthropic |
| **Copy Agent** | Write all text content | Design spec + brand voice | Copy for each section | copywriting, ai-voice-humanizer |
| **Component Agent** | Build React components | Design spec + copy | Component files | Next.js, Tailwind, Framer Motion |
| **Asset Agent** | Create/procure images, icons, graphics | Design spec | Image files | Nano Banana, Canva, public domain |
| **Integration Agent** | Assemble components, wire routes, configure | All components | Working application | Next.js, build tools |
| **Quality Agent** | Verify correctness, consistency, functionality | Built application | Pass/fail report | Verification checklist |
| **Compliance Agent** | Check legal requirements | Built application | Compliance report | web-compliance-gate, regulatory skills |

### Step 3.3: Execution Order

**For a website rebuild:**

```
Parallel Phase 1 (can run simultaneously):
  - Research Agent → Competitor analysis
  - Design Agent → Extract brand tokens from original

Sequential Phase 2 (depends on Phase 1):
  - Design Agent → Create design spec
  - Copy Agent → Write all copy

Parallel Phase 3 (can run simultaneously):
  - Component Agent → Build each section
  - Asset Agent → Prepare images

Sequential Phase 4:
  - Integration Agent → Assemble page.tsx
  - Quality Agent → Run verification checklist
  - Compliance Agent → Run compliance audit

Gate:
  - If Quality FAIL → Return to Component Agent
  - If Compliance FAIL → Return to Component Agent
  - If both PASS → Deploy Gate
```

### Step 3.4: Communication Between Agents

**State format (shared):**

```json
{
  "project": "ai-revenue-site-rebuild",
  "phase": "component_build",
  "agents_status": {
    "research": "completed",
    "design": "completed",
    "copy": "completed",
    "component_hero": "in_progress",
    "component_audit": "pending",
    "asset_headshot": "completed"
  },
  "design_spec": "path/to/spec.md",
  "brand_tokens": {
    "primary_color": "#D4A574",
    "background": "#0a0a0f",
    "currency": "€",
    "price": "2150"
  },
  "quality_checks": {
    "currency": "pass",
    "placeholders": "pending",
    "links": "pending"
  }
}
```

---

## Phase 4: QUALITY GATES (Zero Tolerance)

### Gate 1: Build Verification

```bash
npm run build
# MUST exit 0
```

### Gate 2: Consistency Verification

| Check | Command | Must Be |
|-------|---------|---------|
| Currency | `grep -r "£" app/` | EMPTY |
| Currency | `grep -r "€" app/` | Present where expected |
| Placeholders | `grep -ri "TODO\|FIXME\|placeholder\|add your\|replace me" app/` | EMPTY |
| Numbers | `grep -r "10 hours\|12 hours" app/` | Consistent (all 10 or all 12) |
| Links | `grep -roh 'https://[^"'\'' ]*' app/ | sort -u` | All valid, no generics |

### Gate 3: Brand Verification

| Check | Evidence |
|-------|----------|
| Brand voice | "Skills, Not Tools" present? Tone consistent? |
| Identity | Colin Miley named? €10M portfolio mentioned? |
| Positioning | "AI systems for sales teams" clear? |
| Contact | Email correct? LinkedIn correct? |

### Gate 4: Compliance Verification

| Check | Skill | Status |
|-------|-------|--------|
| GDPR (if data collection) | web-compliance-gate | Must pass |
| Accessibility | WCAG 2.1 AA check | Must pass |
| Cookie compliance (if cookies) | web-compliance-gate | Must pass |
| Consumer protection | N/A | Price clear, T&C accessible |

### Gate 5: Pre-Flight Design Check (From `design-taste-frontend`)

**CRITICAL: This gate prevents AI-tells and slop.**

#### Tier 1: Critical (Build fails if any unchecked)
- [ ] **Skills read** — ALL relevant skills read and documented in `skill-reading-log.md`
- [ ] **Zero em-dashes (`—`)** anywhere on page (headlines, body, quotes, buttons)
- [ ] **Eyebrow count** — Max 1 per 3 sections. No section numbering ("01 — ", "02 — ")
- [ ] **No 3 equal feature cards** — Use asymmetric grids, zig-zag, bento
- [ ] **Real images** — No div placeholders, no fake screenshots built from divs
- [ ] **Font** — Not Inter (use Geist, Outfit, Satoshi, Cabinet Grotesk, or brand-appropriate)
- [ ] **Icons** — Phosphor/HugeIcons/Radix/Tabler, not Lucide (unless explicitly requested)
- [ ] **CTA contrast** — WCAG AA (4.5:1), no wrapping, no duplicate intent on page
- [ ] **Hero fits viewport** — Headline ≤2 lines, subtext ≤20 words AND ≤4 lines, CTA visible
- [ ] **Hero top padding** — Max `pt-24` at desktop
- [ ] **Theme lock** — One theme (light/dark/auto) for whole page, no mid-page flips
- [ ] **Color lock** — One accent color used consistently across all sections
- [ ] **Shape lock** — One corner-radius system (all-sharp, all-soft, or all-pill)
- [ ] **No duplicate CTA intent** — "Book Call" + "Contact us" + "Let's talk" = same intent, pick one

#### Tier 2: Standard (Fix before shipping)
- [ ] Currency consistency — all €, no £ or $
- [ ] No placeholders — no "TODO", "FIXME", "coming soon"
- [ ] All links verified — Calendly, LinkedIn, email
- [ ] GDPR compliance — consent checkbox, privacy link, data retention
- [ ] Accessibility — alt text, keyboard nav, focus states, reduced motion
- [ ] Build passes — `npm run build` exits 0
- [ ] Copy self-audit — every visible string re-read, no AI-hallucinated phrases
- [ ] Navigation on one line at desktop, height ≤80px
- [ ] Section layout diversity — at least 4 different layout families across 8 sections
- [ ] No locale/time/weather strips unless brief requires it
- [ ] No scroll cues ("Scroll", "↓ scroll")
- [ ] No version labels in hero (V0.6, BETA)
- [ ] Quotes ≤3 lines, attribution clean (no em-dash)
- [ ] Motion motivated — every animation has a one-sentence justification

**If ANY Tier 1 check fails → HALT. Fix before proceeding.**

---

## Phase 5: DEPLOYMENT

### Step 5.1: EXPLICIT BUILD AUTHORIZATION (Non-Negotiable)

**NO BUILD STARTS WITHOUT EXPLICIT USER COMMAND.**

The user must say one of:
- "Build it"
- "Proceed with build"
- "Approved, build now"
- "BUILD" (explicit)

**Vague approvals are NOT sufficient:**
- ❌ "Looks good" → Ask: "Do you want me to start building now?"
- ❌ "I like it" → Ask: "Shall I proceed to the build phase?"
- ❌ "Go ahead" → Ask: "Confirm: start building the [deliverable]?"

**Only proceed to Step 5.2 after explicit BUILD authorization.**

### Step 5.2: Staging Deploy

Deploy to staging URL first. Never deploy to production without review.

### Step 5.3: Screenshot Verification

Capture screenshots of:
- Hero (desktop + mobile)
- Key section (desktop + mobile)
- Footer (desktop + mobile)

### Step 5.4: User Review

Present to user with:
- What's changed
- What research informed decisions
- What's still placeholder
- Known issues (if any)

### Step 5.5: Production Deploy

Only after explicit user approval.

---

## Future-Feature Considerations

When planning any build, document these for future versions:

### Analytics
| Feature | Skill | When to Add |
|---------|-------|-------------|
| Google Analytics 4 | `analytics-tracking` | Post-launch, with cookie banner |
| Plausible (privacy-friendly) | `analytics-tracking` | Post-launch, GDPR-compliant |
| AI traffic tracking | `ai-traffic-tracking` | When content strategy starts |
| Event tracking | `analytics-tracking` | When optimizing conversion |

### Geo-Pricing
| Feature | Consideration | Implementation |
|---------|--------------|----------------|
| IP-based detection | Detect user location | MaxMind GeoIP or Cloudflare |
| Currency conversion | € → £ → $ based on region | Real-time API (ExchangeRate-API) |
| Localized content | Pricing, case studies, testimonials by region | CMS with locale support |
| Compliance per region | GDPR (EU), CCPA (US), PIPEDA (CA) | `web-compliance-gate` per region |

### Multi-Language
| Feature | Trigger | Implementation |
|---------|---------|----------------|
| i18n framework | Traffic from non-EN countries | Next.js i18n + Fish Audio for voice |
| Localized SEO | Market expansion | hreflang tags, localized content |

---

## Anti-Patterns (What NOT To Do)

### Anti-Pattern 1: Building Without Research

**Symptom:** "I think dark mode looks good for B2B."
**Fix:** Research what top 5 competitors use. Cite evidence.

### Anti-Pattern 2: Skipping Skill Reading

**Symptom:** "I'll just build it, I know how to make a landing page."
**Fix:** Read the SKILL.md. Every time. Even if you've read it before.

### Anti-Pattern 3: One-Person Show

**Symptom:** One agent does research + design + code + review.
**Fix:** Split into specialist agents. Review agent is independent.

### Anti-Pattern 4: Deploying Without Gates

**Symptom:** "Looks good, let's ship it."
**Fix:** Run ALL gates. If any fail, halt deployment.

### Anti-Pattern 5: Assuming Instead of Verifying

**Symptom:** "The currency is probably € since he's in Ireland."
**Fix:** Check the original site. Check USER.md. Verify before deciding.

### Anti-Pattern 6: Assuming Budget Constraints

**Symptom:** "They probably want the cheaper option."
**Fix:** Present all tiers with cost + quality. Let user choose. Default: STOP and ask.

### Anti-Pattern 7: Starting Build Without Explicit Authorization

**Symptom:** User says "looks good" → immediately start coding.
**Fix:** Ask for explicit "BUILD" command. Vague approval is not authorization.

---

## Example: Rebuilding the AI Revenue Site (Correct Process)

### What Should Have Happened (2026-08-02)

```
[User: "Rebuild the site"]
   ↓
[Context Agent] → Read USER.md, MEMORY.md, original site files
   ↓
[Gap Analysis] → "I know brand, pricing, voice. Missing: headshot, Calendly URL."
   ↓
[User Input] → "Here are photos, here's my LinkedIn, Calendly is..."
   ↓
[Research Agent] → "Top 3 B2B sales consultants use: dark theme, 
                     case studies, two-tier CTAs, clear pricing"
   ↓
[Design Agent] → Read design-taste-frontend + frontend-design-anthropic
                 → Create design spec
                 → "Preserve existing tokens. Enhance with: aurora bg, 
                     case study cards, two-tier offer, email capture"
   ↓
[Copy Agent] → Write all copy
   ↓
[Component Agent] → Build each section
   ↓
[Integration Agent] → Assemble
   ↓
[Quality Gate] → Currency check: € found ✓
                → Placeholder check: 0 found ✓
                → Build check: passes ✓
   ↓
[Compliance Gate] → GDPR: Email capture needs consent checkbox ✗
                   → Accessibility: Missing alt text ✗
   ↓
[HALT] → Fix compliance issues before deploy
   ↓
[Re-run Gates] → All pass ✓
   ↓
[Deploy to Staging] → Screenshot, present to user
   ↓
[User Approval] → "Looks good"
   ↓
[Deploy to Production]
```

### What Actually Happened (Mistakes)

```
[User: "Rebuild the site"]
   ↓
[Me] → Didn't read skills first
   ↓
[Me] → Didn't verify original site currency
   ↓
[Me] → Built everything in one pass
   ↓
[Me] → Self-reviewed (contaminated)
   ↓
[Me] → Deployed
   ↓
[User] → "That's £ not €"
   ↓
[Me] → Oh. Fix. Redeploy.
   ↓
[User] → "What about GDPR?"
   ↓
[Me] → Oh. Fix. Redeploy.
   ↓
[User] → "The image still has the woman"
   ↓
[Me] → Oh. Fix. Redeploy.
```

**Cost of mistakes:** 4 deploy cycles, user frustration, credibility damage.
**Cost of process:** 1 research phase, 1 design spec, 1 build, 1 deploy.

---

## Usage

```bash
# Invoke the orchestrator
/build-orchestrator --deliverable website --domain ai-revenue-systems --type landing-page

# Or user says: "Build me a landing page for my consulting business"
# → Orchestrator auto-detects deliverable, reads context, asks gaps, selects skills, executes graph
```

---

*Skill created: 2026-08-02*
*Based on: All mistakes made on 2026-08-02 and the framework to prevent them*
