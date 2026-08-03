---
name: web-compliance-gate
version: "1.0"
description: "Legal compliance checkpoint for websites, landing pages, and web applications. Covers GDPR (EU/UK), ePrivacy Directive (cookies), accessibility (WCAG), and consumer protection law. Activates before any feature that collects data, sets cookies, or processes personal information."
argument-hint: 'Run before deploying any site with: forms, analytics, cookies, email capture, payment processing, or user accounts'
allowed-tools: Read, Write, Edit
homepage: https://github.com/colin1miley-stack/openclaw-skills
author: AI Revenue Systems
license: MIT
user-invocable: true
metadata:
  openclaw:
    emoji: "⚖️"
    tags:
      - compliance
      - gdpr
      - legal
      - web
      - privacy
---

# Web Compliance Gate

**Purpose:** Prevent legal liability by checking every web feature against EU/UK data protection and consumer law before deployment.

**When to activate:**
- Adding ANY form (contact, email capture, quiz, survey)
- Adding ANY tracking (Google Analytics, Meta Pixel, Hotjar)
- Adding ANY cookie (session, preference, marketing)
- Adding ANY payment processing
- Adding ANY user account system
- Launching ANY new page or site

**When to skip:**
- Purely informational static page with no forms, cookies, or tracking
- Internal tool not accessible to public

---

## The Gate: Check Before You Ship

### Step 1: What Data Does This Feature Collect?

List EVERY piece of data:

| Data Type | Example | GDPR Classification | Required Action |
|-----------|---------|---------------------|-----------------|
| **Email address** | `colin@vitae10.com` | Personal data (Article 4) | Consent + privacy notice |
| **Name** | "Colin Miley" | Personal data | Consent + privacy notice |
| **IP address** | Logged by server | Personal data | Legitimate interest or consent |
| **Cookie ID** | `_ga` (Google Analytics) | Personal data | Consent (ePrivacy) |
| **Health data** | Quiz answers about sleep | Special category (Article 9) | **Explicit consent** |
| **Location** | GeoIP | Personal data | Consent or legitimate interest |
| **Payment info** | Credit card | Personal data + PCI-DSS | Consent + encryption + PCI compliance |
| **Behavioural data** | Page views, clicks | Personal data | Consent or legitimate interest |

**If you collect ANY data → Proceed to Step 2**

**If you collect NO data → Compliance complete. Ship.**

---

### Step 2: What Legal Basis Applies?

GDPR requires a legal basis for processing. Choose ONE:

| Basis | When to Use | Example |
|-------|-------------|---------|
| **Consent** | User actively agrees | Email newsletter signup, marketing cookies |
| **Contract** | Necessary for service | Processing payment for a purchase |
| **Legal obligation** | Required by law | Tax records, court order |
| **Vital interests** | Life or death | Emergency contact |
| **Public task** | Public authority function | Government service |
| **Legitimate interest** | Your business need, balanced against user rights | Fraud prevention, security, analytics (with opt-out) |

**For email capture → Consent required (Article 6(1)(a))**
**For analytics cookies → Consent required (ePrivacy Directive)**
**For essential cookies (session, security) → Legitimate interest or contract**

---

### Step 3: Consent Requirements (GDPR Article 7)

**Consent MUST be:**

| Requirement | What This Means | What NOT To Do |
|-------------|-----------------|----------------|
| **Freely given** | User has real choice | No pre-ticked boxes. No bundled consent. |
| **Specific** | One consent per purpose | Don't bundle "email me" with "share my data with partners" |
| **Informed** | User knows what they're consenting to | Don't hide details in 50-page T&Cs |
| **Unambiguous** | Clear affirmative action | No implied consent ("by using this site...") |
| **Withdrawable** | User can withdraw as easily as they gave | One-click unsubscribe. No hoops. |
| **Documented** | You can prove consent was given | Timestamp, what they consented to, how |

**For email capture, consent MUST include:**
- [ ] Unticked checkbox (not pre-ticked)
- [ ] Clear statement of what they get ("5-Minute Sales Audit PDF")
- [ ] Clear statement of what else they might get ("occasional emails about AI for sales")
- [ ] Link to privacy policy
- [ ] Statement that they can unsubscribe anytime
- [ ] No bundling with other consents (marketing ≠ service provision)

---

### Step 4: Cookie Compliance (ePrivacy Directive)

**Cookie categories:**

| Category | Examples | Consent Required? |
|----------|----------|-------------------|
| **Strictly necessary** | Session cookies, security, load balancing | ❌ No |
| **Preferences** | Language, theme, font size | ✅ Yes |
| **Analytics** | Google Analytics, Plausible, Hotjar | ✅ Yes |
| **Marketing** | Meta Pixel, Google Ads, retargeting | ✅ Yes |

**Cookie banner requirements:**
- [ ] Banner before any non-essential cookies are set
- [ ] Clear categories (necessary / preferences / analytics / marketing)
- [ ] Granular choice (user can accept analytics but reject marketing)
- [ ] "Reject all" option (not just "accept all" or "manage preferences")
- [ ] Link to cookie policy explaining each cookie
- [ ] Easy way to change preferences later

**What happens if you set Google Analytics without consent:**
- 🔴 **Illegal in EU/UK** (ePrivacy Directive)
- 🔴 **€20M or 4% revenue fine risk**
- 🟠 Data collected may be inadmissible in analytics
- 🟡 Competitor or privacy advocate could report you

---

### Step 5: Privacy Notice Requirements (GDPR Article 13-14)

**Every site that collects data MUST have a privacy notice with:**

| Required Element | What to Include |
|-----------------|-----------------|
| **Identity** | Who you are: AI Revenue Systems, Colin Miley, contact details |
| **DPO** | Data Protection Officer contact (if required) |
| **Purpose** | Why you collect data: "To send you the 5-Minute Sales Audit PDF" |
| **Legal basis** | Article 6 basis: "Consent" |
| **Recipients** | Who sees the data: "Formspree (our form processor), ConvertKit (our email platform)" |
| **International transfers** | If data leaves EU/UK: "Formspree servers are in the US. We use Standard Contractual Clauses." |
| **Retention** | How long you keep it: "24 months after last interaction" |
| **Rights** | GDPR rights: access, rectification, erasure, restriction, portability, objection |
| **Withdrawal** | How to withdraw consent: "Click unsubscribe in any email or email privacy@..." |
| **Complaint** | How to complain to ICO: "ico.org.uk/make-a-complaint" |
| **Automated decisions** | If applicable: "We do not use automated decision-making" |

**Privacy notice must be:**
- [ ] Written in plain English (not legalese)
- [ ] Accessible from every page (footer link)
- [ ] Linked at point of consent (near the checkbox)
- [ ] Updated when data practices change

---

### Step 6: Accessibility (WCAG 2.1 AA)

**Required by law in many jurisdictions for public sector. Strongly recommended for B2B.**

| Requirement | Check |
|-------------|-------|
| **Color contrast** | Text ≥ 4.5:1 against background |
| **Keyboard navigation** | All interactive elements reachable via Tab |
| **Focus indicators** | Visible focus ring on all links/buttons |
| **Alt text** | All images have descriptive alt text |
| **Form labels** | All inputs have associated labels |
| **Error identification** | Form errors clearly identified and described |
| **Resizable text** | Page usable at 200% zoom |
| **No auto-play** | No audio/video auto-plays without user control |

---

### Step 7: Consumer Protection (UK/EU)

| Requirement | What This Means |
|-------------|-----------------|
| **Clear pricing** | All prices include VAT, no hidden fees |
| **Cooling-off period** | 14-day right to cancel for digital services (if not started) |
| **Terms & Conditions** | Clear contract terms, accessible before purchase |
| **Refund policy** | Clearly stated, legally compliant |
| **Contact details** | Business name, address, email, phone |
| **Company number** | If limited company, display on site |

---

## Feature-Specific Compliance Checklists

### Email Capture (Lead Magnet)

```
□ Consent checkbox: unticked, clearly worded
□ Link to privacy policy (near checkbox)
□ Clear value proposition ("Get the 5-Minute Sales Audit PDF")
□ Clear frequency ("Occasional emails, not daily")
□ Unsubscribe mechanism (one-click in every email)
□ Privacy policy page exists and is accessible
□ No pre-ticked boxes
□ No bundling with unrelated consents
□ Double opt-in (recommended but not strictly required)
```

### Contact Form

```
□ Privacy notice linked near form
□ Only collect necessary fields (name, email, message)
□ No auto-checkboxes for marketing
□ Secure transmission (HTTPS)
□ Data retention limit stated
□ Spam protection (honeypot or CAPTCHA)
```

### Google Analytics / Tracking

```
□ Cookie banner BEFORE tracking starts
□ "Reject all" option available
□ Analytics category clearly explained
□ IP anonymisation enabled (recommended)
□ Data processing agreement with Google
□ User can withdraw consent and stop tracking
```

### E-commerce / Payments

```
□ Terms & Conditions accessible before checkout
□ Refund policy clearly stated
□ PCI-DSS compliant payment processor (Stripe, etc.)
□ VAT shown at checkout
□ 14-day cooling-off period for digital products
□ Secure checkout (HTTPS, padlock)
```

### Newsletter Subscription

```
□ Double opt-in (email confirmation required)
□ Unsubscribe link in every email
□ Unsubscribe works within 24 hours
□ List of sender name and physical address in email
□ No purchased lists
□ No spam (only people who opted in)
```

---

## Violation Severity

| Level | Example | Fine Risk | Action |
|-------|---------|-----------|--------|
| 🔴 **Critical** | Collecting health data without explicit consent | €20M or 4% revenue | **DO NOT SHIP** |
| 🔴 **Critical** | Setting marketing cookies without consent | €20M or 4% revenue | **DO NOT SHIP** |
| 🟠 **High** | No privacy policy | Up to €10M or 2% revenue | Fix before ship |
| 🟠 **High** | Pre-ticked consent boxes | Up to €10M or 2% revenue | Fix before ship |
| 🟡 **Medium** | Missing cookie categories in banner | Warning → fine | Fix soon |
| 🟡 **Medium** | Privacy policy hard to find | Warning → fine | Fix soon |
| 🟢 **Low** | Missing alt text on decorative image | No fine, accessibility issue | Fix when convenient |

---

## Quick Compliance Template

### Privacy Policy (Minimum Viable)

```markdown
# Privacy Policy — AI Revenue Systems

**Last updated:** [Date]

## Who We Are
AI Revenue Systems is operated by Colin Miley.
Contact: colin@vitae10.com

## What Data We Collect
- **Email address** — when you download the free audit or contact us
- **Name** — if you provide it
- **Usage data** — pages visited, time on site (via analytics, with consent)

## Why We Collect It
- To send you the 5-Minute Sales Audit PDF
- To respond to your enquiries
- To improve our website (analytics)

## Legal Basis
- **Consent** — for email capture and analytics cookies
- **Legitimate interest** — for security and fraud prevention

## Who We Share With
- **Formspree** — processes our contact forms (US-based, SCCs in place)
- **Vercel** — hosts our website (US-based, SCCs in place)
- **Google Analytics** — if you consent to analytics cookies

## How Long We Keep It
- Email addresses: 24 months after last interaction
- Analytics data: 26 months (Google Analytics default)

## Your Rights
- **Access** — request a copy of your data
- **Rectification** — correct inaccurate data
- **Erasure** — request deletion ("right to be forgotten")
- **Restriction** — limit how we use your data
- **Portability** — receive data in a portable format
- **Objection** — object to processing

To exercise any right, email colin@vitae10.com.

## Cookies
We use:
- **Essential cookies** — required for site function (no consent needed)
- **Analytics cookies** — to understand site usage (consent required)

See our Cookie Policy for details.

## Complaints
If you're unhappy with how we handle your data, you can complain to the 
Information Commissioner's Office (ICO): ico.org.uk/make-a-complaint

## Changes
We may update this policy. Check this page for the latest version.
```

### Cookie Policy (Minimum Viable)

```markdown
# Cookie Policy — AI Revenue Systems

## What Are Cookies?
Small text files stored on your device when you visit websites.

## Cookies We Use

| Cookie | Purpose | Duration | Category |
|--------|---------|----------|----------|
| `__vercel` | Site hosting | Session | Essential |
| `_ga` | Google Analytics (if consented) | 2 years | Analytics |
| `_gid` | Google Analytics (if consented) | 24 hours | Analytics |

## How to Manage Cookies
You can:
- Accept or reject cookies via our cookie banner
- Clear cookies in your browser settings
- Use browser extensions to block cookies

## Changes
We may update this policy. Check this page for the latest version.
```

---

## Pre-Deployment Checklist

Run this for EVERY feature before shipping:

```
□ What data does this collect? (List every field)
□ What is the legal basis? (Consent / contract / legitimate interest)
□ Is consent freely given, specific, informed, unambiguous?
□ Are consent checkboxes unticked by default?
□ Is there a "Reject all" option for cookies?
□ Is the privacy policy accessible from this page?
□ Is the privacy policy written in plain English?
□ Are user rights explained?
□ Is there an unsubscribe mechanism?
□ Is data encrypted in transit (HTTPS)?
□ Is there a data retention limit?
□ Are third-party processors named?
□ Are international transfers explained?
□ Is the cookie banner shown before non-essential cookies?
□ Are all images accessible (alt text)?
□ Is color contrast ≥ 4.5:1?
□ Is keyboard navigation possible?
```

**If ANY critical or high item is unchecked → DO NOT SHIP.**

---

*Skill created: 2026-08-02 | Based on GDPR, ePrivacy Directive, WCAG 2.1 AA*
