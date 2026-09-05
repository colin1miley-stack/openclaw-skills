---
name: browser-use-icm
description: Browser automation for AI agents, ICM-converted from browser-use/browser-use. Enables Signal to auto-post content to social platforms via browser automation when APIs are unavailable or restricted. Use for LinkedIn, Twitter/X, Instagram, Facebook, Pinterest posting.
---

# Browser-Use — ICM Converted

**Source:** `browser-use/browser-use` (MIT License)  
**Converted:** 2026-09-05  
**Purpose:** Signal distribution layer — auto-post content to social platforms via browser automation  

---

## ICM Layer Mapping

| Layer | File | Usage |
|-------|------|-------|
| **Layer 0** | `SOUL.md`, `AGENTS.md` | Security boundaries, no credential exposure |
| **Layer 1** | Root `CONTEXT.md` | Route to Signal distribution stages |
| **Layer 2** | This SKILL.md | Task contract for browser automation |
| **Layer 3** | `skills/browser-use-icm/`, `references/` | Browser config, platform selectors |
| **Layer 4** | `stages/05-signal-distribution/output/` | Scripts, credentials (encrypted), logs |

---

## What Browser-Use Does

```
Signal generates content (text, image, video)
    ↓
Browser-use Agent opens browser (headless or visible)
    ↓
Navigates to platform (LinkedIn, Twitter, Instagram, etc.)
    ↓
Logs in with stored credentials
    ↓
Composes post with generated content
    ↓
Publishes / Schedules
    ↓
Returns: URL, timestamp, screenshot proof
```

**Core Features:**
- Natural language task descriptions ("Post this to LinkedIn")
- Automatic element detection and interaction
- Screenshot proof of completion
- Headless or visible browser modes
- Cookie/session persistence
- Proxy support
- Real browser or remote/cloud browser

---

## Architecture for Signal Integration

### Distribution Router Pattern

```python
from browser_use import Agent, Browser, ChatBrowserUse
from browser_use.browser import BrowserSession

class SignalDistributionRouter:
    """Routes Signal content to platforms via browser automation."""
    
    def __init__(self):
        self.browser = Browser(headless=True)
        self.llm = ChatBrowserUse()  # Fastest, cheapest
        
    async def post(self, content: SignalContent, platform: Platform) -> PostResult:
        """Post content to specified platform."""
        agent = Agent(
            task=platform.build_task(content),
            llm=self.llm,
            browser=self.browser,
            max_steps=50,
        )
        history = await agent.run()
        return PostResult.from_history(history)
```

### Platform Task Templates

#### LinkedIn
```python
LINKEDIN_TASK = """
1. Go to https://www.linkedin.com
2. Log in with credentials from env vars LINKEDIN_EMAIL, LINKEDIN_PASSWORD
3. Click "Start a post" button
4. Type the following content exactly:
   {text_content}
5. If image provided: Click "Add media" → upload {image_path}
6. Click "Post" button
7. Wait for confirmation
8. Return the post URL
"""
```

#### Twitter/X
```python
TWITTER_TASK = """
1. Go to https://twitter.com
2. Log in with credentials from env vars TWITTER_EMAIL, TWITTER_PASSWORD
3. Click "What is happening?!" text box
4. Type the following content exactly:
   {text_content}
   (ensure under 280 chars)
5. If image provided: Click media icon → upload {image_path}
6. Click "Post" button
7. Return the tweet URL
"""
```

#### Instagram
```python
INSTAGRAM_TASK = """
1. Go to https://www.instagram.com
2. Log in with credentials from env vars INSTAGRAM_EMAIL, INSTAGRAM_PASSWORD
3. Click "Create" → "Post"
4. Upload {image_path} or {video_path}
5. Add caption:
   {text_content}
6. Click "Share"
7. Return the post URL
"""
```

---

## Signal Integration Endpoint

### New API Route: `POST /v1/distribute`

```typescript
// Signal Worker (Hono) addition
app.post('/v1/distribute', async (c) => {
  const { contentId, platforms, schedule } = await c.req.json();
  
  // Fetch content from KV
  const content = await c.env.SIGNAL_KV.get(`content:${contentId}`);
  
  // Queue distribution jobs
  for (const platform of platforms) {
    await c.env.DISTRIBUTION_QUEUE.send({
      content,
      platform,
      scheduledAt: schedule,
    });
  }
  
  return c.json({ queued: platforms.length });
});
```

### Worker Queue Handler

```python
# browser-use worker (runs on local machine or Railway)
import asyncio
from browser_use import Agent, Browser, ChatBrowserUse
from signal_types import SignalContent, Platform

async def handle_distribution_job(job: dict):
    content = SignalContent.from_dict(job['content'])
    platform = Platform(job['platform'])
    
    browser = Browser(headless=True)
    agent = Agent(
        task=platform.get_task_template(content),
        llm=ChatBrowserUse(),
        browser=browser,
        max_steps=50,
        sensitive_data={
            'email': os.getenv(f'{platform.name}_EMAIL'),
            'password': os.getenv(f'{platform.name}_PASSWORD'),
        }
    )
    
    history = await agent.run()
    
    # Return proof to Signal
    return {
        'platform': platform.name,
        'url': history.urls()[-1],
        'screenshot': history.screenshots()[-1],
        'success': len(history.errors()) == 0,
    }
```

---

## ICM Stage Contract for Browser-Use Integration

```json
{
  "stageName": "signal-distribution",
  "branchName": "feat/browser-use-distribution",
  "qualityGate": "quality-gate.md",
  "subTasks": [
    {
      "id": "s5-001",
      "title": "Install browser-use and verify",
      "agent": "builder",
      "acceptance": ["pip install browser-use", "basic agent run succeeds"],
      "estimatedTokens": 5000
    },
    {
      "id": "s5-002",
      "title": "Create LinkedIn posting script",
      "agent": "builder",
      "acceptance": ["Logs into LinkedIn", "Posts content", "Returns URL"],
      "estimatedTokens": 8000
    },
    {
      "id": "s5-003",
      "title": "Create Twitter/X posting script",
      "agent": "builder",
      "acceptance": ["Logs into Twitter", "Posts content under 280 chars", "Returns URL"],
      "estimatedTokens": 8000
    },
    {
      "id": "s5-004",
      "title": "Create Instagram posting script",
      "agent": "builder",
      "acceptance": ["Logs into Instagram", "Uploads media", "Posts with caption", "Returns URL"],
      "estimatedTokens": 10000
    },
    {
      "id": "s5-005",
      "title": "Integrate into Signal Worker",
      "agent": "builder",
      "acceptance": ["New /v1/distribute endpoint", "Queue handler", "Credential management"],
      "estimatedTokens": 10000
    }
  ]
}
```

---

## Quality Gate

- [ ] Browser-use installs without dependency conflicts
- [ ] LinkedIn script posts successfully (test with dummy account)
- [ ] Twitter/X script posts successfully
- [ ] Instagram script posts successfully
- [ ] Credentials stored in environment variables (never in code)
- [ ] Screenshot proof captured for each post
- [ ] Error handling for login failures, CAPTCHAs, rate limits
- [ ] Signal Worker endpoint accepts distribution requests

---

## Security & Privacy

**Critical Rules:**
1. **Never commit credentials** — use `.env` files, never commit to git
2. **Use `sensitive_data` parameter** — browser-use masks these in logs
3. **Headless mode for production** — visible browser for debugging only
4. **Rate limiting** — max 1 post per platform per 5 minutes to avoid bans
5. **Session persistence** — save cookies to avoid repeated logins

---

## Cost Estimate

| Component | Cost/Month |
|-----------|-----------|
| Browser-use library | Free (open source) |
| ChatBrowserUse LLM | ~$10-20 (pay-per-use) |
| Browser compute (local) | $0 |
| Browser compute (cloud) | ~$10-30 |
| **Total** | **~$10-50/month** |

---

## Anti-Patterns

| Anti-Pattern | Why It Fails |
|-------------|--------------|
| Hardcoded credentials | Security breach, git history exposure |
| No rate limiting | Account banned by platform |
| Ignoring CAPTCHAs | Automation blocked |
| No screenshot proof | Can't verify post succeeded |
| Posting identical content | Platform spam detection |

---

## Files to Review in Source

| File | Purpose |
|------|---------|
| `browser_use/agent.py` | Core Agent class |
| `browser_use/browser.py` | Browser session management |
| `browser_use/tools.py` | Available actions |
| `skills/open-source/references/agent.md` | Agent configuration |
| `skills/open-source/references/browser.md` | Browser configuration |

---

## Platform-Specific Notes

| Platform | API Available? | Browser-Use Needed? | Difficulty |
|----------|---------------|---------------------|------------|
| **LinkedIn** | ⚠️ Restricted | ✅ Yes (primary method) | Medium |
| **Twitter/X** | 🟡 Paid ($100/mo) | ✅ Yes (cheaper) | Medium |
| **Instagram** | ⚠️ Restricted | ✅ Yes (primary method) | High (stories/reels) |
| **Facebook** | 🟡 Available | ⚠️ Optional | Low |
| **Pinterest** | 🟡 Available | ⚠️ Optional | Low |
| **YouTube** | ✅ Available | ❌ No (use API) | Low |

---

*ICM-converted from browser-use/browser-use. See original repo for full documentation.*
