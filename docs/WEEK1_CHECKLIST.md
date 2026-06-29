# Week 1 Checklist — Colin's OpenClaw Skills Launch

_Extracted from the [GitHub Optimization Guide](../GITHUB_OPTIMIZATION.md). Tasks are ordered by dependency and impact. Check them off as you go._

---

## 🎯 Week 1 Goal

Get **1 polished skill** live on GitHub and ClawHub, with all foundational repo files in place. This is your public face — make it count.

---

## Day 1: Choose & Name

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 1.1 | **Choose your first skill.** Pick a problem YOU have. You'll be the first user. | 30 min | You can describe the skill in one sentence and explain why someone would use it. |
| 1.2 | **Name the repo.** Use the formula: `openclaw-[domain]-[action]-skill` or a branded collection name. | 10 min | Name is under 30 chars, uses hyphens, starts with a keyword. |
| 1.3 | **Create the GitHub repo.** Add description, 20 topics, MIT license. | 15 min | Repo exists on GitHub with description, topics, and license selected. |

### 💡 Tips for Day 1
- **Pick the skill you use most often.** If you don't use it, others won't either.
- **Check ClawHub first.** Search for similar skills. If something exists, make yours better or more focused.
- **Topics matter.** Use all 20. Mix framework (`openclaw`, `ai-agent`), domain (`seo`, `marketing`), and use-case (`automation`, `productivity`) tags.

---

## Day 2: Write the Core Docs

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 2.1 | **Write SKILL.md.** Proper YAML frontmatter, clear trigger, actionable steps. | 1–2 hrs | Another person (or your future self) can read the SKILL.md and know exactly when to use this skill and what to do. |
| 2.2 | **Write README.md.** Hero section, quick start, features, example, badges. | 1–2 hrs | A stranger can understand the skill's value in 10 seconds and install it in under 2 minutes. |

### 💡 Tips for Day 2
- **Frontmatter is the contract.** The `description` field is the trigger. Make it specific. List keyword variations. Say "Use when..."
- **README is the landing page.** Lead with the problem, not the tech. Show a GIF or screenshot if possible.
- **Badges build trust.** Add at minimum: License, ClawHub link. Optional: build status, version.

---

## Day 3: Polish & Test

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 3.1 | **Add examples.** At least one concrete input → output example. | 30 min | Example uses a realistic prompt and shows realistic output. |
| 3.2 | **Add screenshots or GIFs.** Visual proof of the skill working. | 30–60 min | At least one visual asset shows the skill in action. |
| 3.3 | **Test the skill locally.** Run it through a real OpenClaw session. | 1 hr | The trigger fires correctly. Steps execute as written. Output is useful. |
| 3.4 | **Fix issues found in testing.** | 30 min | No broken commands, no missing env var warnings, no confusing output. |

### 💡 Tips for Day 3
- **Test like a user, not an author.** Use a prompt you didn't write. See if the skill still makes sense.
- **Record your screen.** A 10-second GIF of the skill working is worth 100 words.

---

## Day 4: Foundation Files (This is where this repo helps!)

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 4.1 | **Add CONTRIBUTING.md.** How to submit issues, suggest features, open PRs. | 30 min | File is welcoming, specific, and includes a code of conduct. |
| 4.2 | **Add CHANGELOG.md.** Start with v0.1.0. List all skills included. | 20 min | File follows Keep a Changelog format. v0.1.0 entry exists. |
| 4.3 | **Add LICENSE.** MIT License with Colin's name and year 2026. | 5 min | LICENSE file is present at repo root with correct attribution. |
| 4.4 | **Add .gitignore.** Appropriate for an OpenClaw skills repo. | 10 min | Ignores env files, local configs, temp files, and OS artifacts. |
| 4.5 | **Add SKILL_TEMPLATE.md.** Template for future contributors. | 30 min | Template includes frontmatter, step-by-step structure, example, and checklist. |
| 4.6 | **Enable GitHub Discussions.** Settings → Features → Discussions. | 5 min | Discussions tab is visible on the repo. |

### 💡 Tips for Day 4
- **These files are your reputation.** A repo with good foundation files looks professional. A repo without them looks abandoned.
- **Copy from this repo.** The files in `github-publish/` are ready to use. Adapt them, don't rewrite from scratch.

---

## Day 5: Publish to ClawHub

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 5.1 | **Install ClawHub CLI.** `npm install -g clawhub` | 5 min | `clawhub --version` returns a version number. |
| 5.2 | **Authenticate.** `clawhub login` | 2 min | Successfully linked to GitHub account. |
| 5.3 | **Publish the skill.** `clawhub skill publish ./skills/<skill-name>/` | 10 min | Command completes without errors. |
| 5.4 | **Verify listing.** Visit `clawhub.ai/colin/<skill-name>` | 5 min | Skill appears on ClawHub with correct name, description, and metadata. |

### 💡 Tips for Day 5
- **ClawHub ranking matters.** Encourage users to install via `clawhub install` (not just `git clone`) — installs count toward ranking.
- **Update your ClawHub profile.** Add a bio and link to your GitHub. Verified publishers rank higher.

---

## Day 6: Share & Promote

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 6.1 | **Write a Twitter/X thread.** 4–6 tweets. Include GIF/screenshot. | 30 min | Thread is posted. Includes install command and repo link. |
| 6.2 | **Post on Reddit.** Choose the most relevant subreddit (r/selfhosted, r/webdev, r/programming, etc.). | 30 min | Post is live. Follows subreddit rules. No corporate marketing speak. |
| 6.3 | **Cross-post on LinkedIn.** Adapt the Twitter thread for a professional audience. | 15 min | Post is live. Focuses on problem solved, not just product. |

### 💡 Tips for Day 6
- **Lead with the problem, not the skill.** "I was doing X manually every week. So I built this." → Works. "Check out my new skill!" → Doesn't.
- **Be in the comments.** Answer questions within 15 minutes of posting. Early engagement drives algorithmic distribution.
- **Timing matters.** Post between 9:00–11:00 AM EST, Tuesday–Thursday.

---

## Day 7: Track & Iterate

| # | Task | Time | Success Criteria |
|---|------|------|------------------|
| 7.1 | **Post in OpenClaw Discord.** Share in #skills or #showcase. | 15 min | Post is live. Includes link to repo and ClawHub listing. |
| 7.2 | **Track metrics.** Record: stars, ClawHub installs, issues, feedback. | 15 min | Baseline metrics recorded in a spreadsheet or note. |
| 7.3 | **Respond to feedback.** Reply to all comments, issues, and DMs. | 30 min | Every piece of feedback has a response. |
| 7.4 | **Plan Week 2.** Review what worked. Decide on next content piece. | 15 min | Week 2 plan is written down. |

### 💡 Tips for Day 7
- **Feedback is a gift.** Even negative feedback is useful. Say thank you. Fix what you can. Explain what you can't.
- **Don't chase vanity metrics.** 5 stars from people who actually use the skill > 50 stars from people who never installed it.

---

## ✅ Week 1 Success Criteria

Check these off at the end of the week. If any are missing, that's your priority for Week 2 Day 1.

- [ ] **1 skill published on GitHub** with full README, SKILL.md, and foundation files
- [ ] **1 skill published on ClawHub** and verified on clawhub.ai
- [ ] **5+ stars** on the GitHub repo (seed from your network if needed)
- [ ] **1+ meaningful piece of feedback** from someone who isn't you
- [ ] **All foundation files in place:** CONTRIBUTING.md, CHANGELOG.md, LICENSE, .gitignore, SKILL_TEMPLATE.md
- [ ] **GitHub Discussions enabled**
- [ ] **At least 1 social post** (Twitter/X, Reddit, or LinkedIn) with engagement
- [ ] **Metrics tracked** for comparison next week

---

## 📊 Week 1 Time Budget

| Category | Estimated Time |
|----------|---------------|
| Choosing & naming | 55 min |
| Writing core docs | 2–4 hrs |
| Polish & testing | 2–3 hrs |
| Foundation files | 1.5 hrs |
| Publishing to ClawHub | 20 min |
| Sharing & promotion | 1.5 hrs |
| Tracking & iteration | 1 hr |
| **Total** | **~9–12 hours** |

**Spread across 7 days:** ~1.5–2 hours per day. That's a serious but achievable commitment.

---

## 🚀 If You Finish Early

Done with Week 1 by Day 5? Here's what to do next:

1. **Start skill #2.** Pick a different domain. Build authority across multiple areas.
2. **Write a Dev.to article.** "How I Built [Skill Name] with OpenClaw" — drives SEO and establishes authority.
3. **Engage on Reddit.** Answer questions in your domain. Don't promote yet. Build reputation first.
4. **Optimize your GitHub profile.** Add a README to your GitHub profile showcasing your skills.

---

## 📝 Notes & Reflections

Use this space to jot down what worked, what didn't, and what you'll do differently next week.

```
Date: ___________

What worked well:
_______________________________________________

What was harder than expected:
_______________________________________________

What I'll change next week:
_______________________________________________

Metrics snapshot:
- Stars: ____
- ClawHub installs: ____
- Issues opened: ____
- PRs received: ____
- Social engagement: ____
```

---

*This checklist was generated from the [GitHub Optimization Guide](GITHUB_OPTIMIZATION.md). Update it as you learn what works for you.*
