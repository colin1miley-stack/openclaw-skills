# Contributing to OpenClaw Skills Collection

Thank you for your interest in contributing! This collection is only as good as the community behind it. Whether you're fixing a typo, suggesting a new skill, or improving an existing one, your help is genuinely appreciated.

## Table of Contents

- [How to Contribute](#how-to-contribute)
- [Skill Quality Standards](#skill-quality-standards)
- [Review Process](#review-process)
- [Code of Conduct](#code-of-conduct)
- [Development Setup](#development-setup)

---

## How to Contribute

### Reporting Bugs or Issues

1. Check if the issue is already reported in [Issues](../../issues).
2. If not, open a new issue and include:
   - What you expected to happen
   - What actually happened
   - Steps to reproduce (with example prompts if possible)
   - Your environment (OpenClaw version, OS, skill version)
   - Any error messages or output

### Suggesting New Skills

Have an idea for a skill? Open an issue with the `enhancement` label and describe:
- The problem it solves
- When the agent should activate it (trigger conditions)
- What tools or APIs it needs
- A rough example of how it would work

Before suggesting, check if a similar skill already exists. If it does, consider improving the existing one instead.

### Improving Existing Skills

Found a gap in documentation? A trigger that misfires? A missing example? We welcome improvements of all sizes.

1. Fork the repository.
2. Create a branch: `git checkout -b fix/skill-name-description` or `git checkout -b feat/skill-name-description`
3. Make your changes.
4. Test your changes locally with OpenClaw.
5. Update `SKILL.md`, `README.md`, and `CHANGELOG.md` if needed.
6. Submit a pull request with a clear description of what changed and why.

### Submitting a New Skill

If you'd like to add a brand-new skill to the collection, follow the [SKILL_TEMPLATE.md](SKILL_TEMPLATE.md) as your starting point. Then:

1. Place your skill in `skills/<your-skill-name>/`
2. Include a `SKILL.md` (required) and `README.md` (recommended)
3. Add it to the main `README.md` skill index
4. Update the `CHANGELOG.md` under the "Unreleased" section

---

## Skill Quality Standards

Every skill in this collection must meet the following bar. This is what separates a useful skill from a broken one.

### 1. Clear, Specific Trigger

The `description` in `SKILL.md` frontmatter must tell the agent **exactly** when to activate this skill. Not what it does — **when to use it**.

**Good:**
```yaml
description: When the user wants to run a technical SEO audit, check site health, analyze page speed, or identify crawl issues. Use when keywords like "SEO audit", "site health", or "crawl errors" appear.
```

**Bad:**
```yaml
description: A tool for SEO stuff.
```

### 2. Actionable, Not Reference

The body of `SKILL.md` should read like a **playbook** the agent can follow step-by-step. Not a Wikipedia article.

- Numbered steps where possible
- Specific commands or actions
- Expected output format
- What to do when things fail

### 3. At Least One Concrete Example

Every skill must include a real input → output example. Agents (and humans) learn from patterns, not abstractions.

```markdown
## Example

User: "Audit example.com for SEO issues"
Agent: Runs Lighthouse, checks robots.txt, validates structured data...
Returns: Prioritized list of fixes with severity ratings
```

### 4. Documented Requirements

List all required environment variables, binaries, and dependencies in the frontmatter:

```yaml
metadata:
  openclaw:
    requires:
      env:
        - API_KEY_NAME
      bins:
        - curl
        - jq
```

### 5. Tested Locally

Before submitting, test your skill with a real OpenClaw session. Does the trigger fire correctly? Do the steps produce the right output? Does it handle edge cases gracefully?

### 6. One Job, Done Well

A skill should solve **one** problem clearly. If your idea spans multiple domains, consider splitting it into separate, focused skills.

---

## Review Process

All contributions go through the following review:

1. **Automated checks** — GitHub Actions verify formatting and basic structure.
2. **Maintainer review** — Within 48 hours, a maintainer will review for:
   - Quality standards (see above)
   - Consistency with existing skills
   - Clarity and usefulness
3. **Feedback loop** — If changes are needed, we'll comment on the PR. Please respond within 7 days.
4. **Merge** — Once approved, contributions are merged with a squash commit.

### Response Time Targets

| Type | Target |
|------|--------|
| First response on new issues | 48 hours |
| First response on PRs | 48 hours |
| Bug fixes | 7 days to merge |
| New features | 14 days to merge |
| Documentation tweaks | 3 days to merge |

---

## Code of Conduct

This is a professional space. We expect everyone to act accordingly.

- **Be respectful.** Disagree with ideas, not people.
- **Assume good intent.** Most people are here to help.
- **Be constructive.** Criticism is welcome when it comes with a suggestion for improvement.
- **No harassment.** Ever. This includes discrimination, intimidation, or sustained hostility.
- **Stay on topic.** Issues and PRs should focus on the skill at hand.

Violations may result in temporary or permanent removal from the project.

---

## Development Setup

```bash
# Clone the repo
git clone https://github.com/colin1miley-stack/openclaw-skills.git
cd openclaw-skills

# Copy a skill to test locally
cp -r skills/seo-audit ~/.openclaw/skills/seo-audit

# Test the skill
openclaw skills list
openclaw -p "Run an SEO audit on example.com"
```

For more on the skill structure, see [SKILL_TEMPLATE.md](SKILL_TEMPLATE.md).

---

Thank you for contributing. Every skill you improve makes OpenClaw more useful for everyone.
