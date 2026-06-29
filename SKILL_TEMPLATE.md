# Skill Template

Use this template as the starting point for every new skill in this collection. It follows the best practices from our [GitHub Optimization Guide](GITHUB_OPTIMIZATION.md) and ensures consistency across the repo.

---

## File Structure

```
skills/<your-skill-name>/
├── SKILL.md              # Required. The skill manifest + instructions for the agent.
├── README.md             # Recommended. Human-facing landing page for the skill.
├── scripts/              # Optional. Helper scripts the skill may invoke.
│   └── setup.sh
├── examples/             # Optional. Example prompts and expected outputs.
│   └── sample-output.md
└── .env.example          # Optional. Template for required environment variables.
```

---

## SKILL.md Template

Copy this exactly and fill in the blanks. The frontmatter is parsed by OpenClaw and ClawHub — getting it right is critical.

```markdown
---
name: <your-skill-name>
description: When the user wants to [do something specific], [achieve a specific outcome], or [solve a specific problem]. Use when keywords like "keyword-one", "keyword-two", or "keyword-three" appear in the user's request. Also triggers on phrases like "how do I...", "help me...", or "I need to..." [related to your domain].
metadata:
  openclaw:
    requires:
      env:
        - <ENV_VAR_NAME>          # Required environment variable
        - <OPTIONAL_VAR_NAME>     # Optional but recommended
      bins:
        - <binary-name>             # Required CLI tool (e.g., curl, jq)
    primaryEnv: <ENV_VAR_NAME>     # The most important env var to prompt for first
    emoji: 🔧                      # A single emoji representing the skill
---

# <Skill Name>

## When to Use

Repeat the trigger conditions here, but in natural language. Give the agent (and humans) a clear picture of when this skill should activate.

**Triggers on:**
- "User asks to [do X]"
- "User mentions [keyword Y]"
- "User needs help with [problem Z]"

**Does NOT trigger on:**
- [Vague or unrelated request]
- [Request that belongs to a different skill]

## Prerequisites

What must be true before this skill runs?

- [Required env var] is set
- [Required binary] is installed
- [Required service] is accessible
- [Optional but recommended setup step]

## Step-by-Step

1. **Step one** — Specific action the agent should take. Include the exact command or tool call if applicable.
2. **Step two** — Next action. Be precise. If there are multiple paths, cover the primary one first, then mention alternatives.
3. **Step three** — Validation or output format. What should the agent return? How should it format the result?

### Alternative Path (if applicable)

If the user requests [variant], follow this path instead:
1. ...
2. ...

## Example

**User:** "[A realistic example prompt]"

**Agent:** [What the agent does, step by step, with the actual output format]

```
[Example of the actual output the agent should produce]
```

## Error Handling

What should the agent do when things fail?

| Error | Cause | Action |
|-------|-------|--------|
| [Error message] | [What caused it] | [What the agent should do] |
| [Missing env var] | Not configured | Prompt the user to set it, with a link to docs |
| [API timeout] | Network or service issue | Retry once, then explain the issue |

## Output Format

When returning results, use this format:

```markdown
## Summary
[One-paragraph summary of what was done and why]

## Results
- [Result item 1]
- [Result item 2]

## Next Steps
[Optional: What the user should do next]
```

## References

- [Link to official docs]
- [Link to API reference]
- [Link to related skill in this repo]
- [Link to external resource]
```

---

## README.md Template

```markdown
# <Skill Name> <Emoji>

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![ClawHub](https://img.shields.io/badge/ClawHub-Published-blue)](https://clawhub.ai/colin/<skill-name>)

> One-line value proposition. This is what shows in social shares and search results.

[Optional: Hero image or GIF showing the skill in action]

## What It Does

2-3 sentences describing the problem this skill solves and how it solves it. Be specific, not generic.

## Quick Start

```bash
# Install via ClawHub (recommended)
npx clawhub@latest install <skill-name>

# Or clone manually
git clone https://github.com/colin/openclaw-skills.git
cp -r openclaw-skills/skills/<skill-name> ~/.openclaw/skills/<skill-name>
```

## Features

- ✅ [Feature one: specific, concrete benefit]
- ✅ [Feature two: specific, concrete benefit]
- ✅ [Feature three: specific, concrete benefit]

## Example Usage

```
User: "[Realistic example prompt]"
Agent: [What the agent does, with a sample of the output]
```

## Requirements

- OpenClaw >= 2026.2.0
- [Required API key or service account]
- Binary: [required CLI tool, e.g., `curl`, `jq`]

## Installation & Setup

### Step 1: Install the skill

```bash
npx clawhub@latest install <skill-name>
```

### Step 2: Configure environment variables

```bash
cp skills/<skill-name>/.env.example ~/.openclaw/skills/<skill-name>/.env
```

Edit `.env` and add your values:

| Variable | Description | How to Get |
|----------|-------------|------------|
| `API_KEY` | [Service] API key | [Get one here](https://...) |
| `REGION` | API region | Defaults to `us-east-1` |

### Step 3: Verify

```bash
openclaw -p "Test <skill-name> with a simple example"
```

Expected output: [describe what success looks like]

## Configuration

[Environment variables, config files, and how to customize behavior.]

## Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| [Common issue] | [Why it happens] | [How to fix it] |

## Contributing

See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines on reporting bugs, suggesting features, and submitting pull requests.

## License

MIT — see [LICENSE](../LICENSE) for details.

---

> If this skill saves you time, consider giving it a ⭐️ on [GitHub](https://github.com/colin/openclaw-skills)!
```

---

## Checklist Before Submitting

Before opening a pull request with a new skill, run through this checklist:

### Structure
- [ ] Skill lives in `skills/<kebab-case-name>/`
- [ ] `SKILL.md` exists and has valid YAML frontmatter
- [ ] `README.md` exists and follows the template above
- [ ] Skill name is descriptive and kebab-case (`seo-audit`, not `my-tool`)

### Quality
- [ ] Trigger description is specific with keyword variations
- [ ] Step-by-step instructions are actionable and numbered
- [ ] At least one concrete example with realistic input → output
- [ ] Requirements (env vars, binaries) are documented in frontmatter and README
- [ ] Error handling is described (what to do when things fail)
- [ ] Skill is tested locally with a real OpenClaw session

### Documentation
- [ ] README has a clear hero section (name, badges, one-liner)
- [ ] Quick Start section is copy-pasteable
- [ ] Features list is specific, not generic
- [ ] Example Usage shows real agent interaction
- [ ] Configuration table is scannable
- [ ] Troubleshooting section covers common issues

### Meta
- [ ] `.env.example` provided if env vars are required
- [ ] `scripts/` included only if helper scripts are needed
- [ ] `examples/` included if sample outputs are helpful
- [ ] CHANGELOG.md updated under "Unreleased"
- [ ] Main repo README.md skill index updated

---

## Tips for a Great Skill

1. **Solve one problem well.** A skill that does one thing clearly is more useful than one that does ten things poorly.
2. **Be specific in triggers.** The agent should know exactly when to use your skill. Ambiguous triggers lead to misfires.
3. **Show, don't just tell.** Include real examples. Abstract descriptions are less useful than concrete patterns.
4. **Assume the user is busy.** Make setup copy-pasteable. Make the first win happen in under 60 seconds.
5. **Handle failure gracefully.** Document what goes wrong and what to do about it. Nothing is worse than a skill that silently fails.
6. **Test before you ship.** Run the skill through a real OpenClaw conversation. If it feels clunky, it probably is.

---

Need help? Open an issue or start a discussion. We're here to help you ship great skills.
