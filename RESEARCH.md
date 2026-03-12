# RESEARCH.md — Problem Discovery

## Research Date
March 12, 2026, 5:00 AM

## Sources Searched
- Twitter/X (`./bin/bird` CLI): wish there was an app, someone should build, frustrated with, startup idea productivity
- Reddit: r/SomebodyMakeThis top posts (week)
- Hacker News: Ask HN front page (current)
- Web searches: Reddit pain points

---

## Raw Candidate Problems Found

### Candidate 1: Event-based dating app
**Source**: r/SomebodyMakeThis (19 upvotes)
Post: "A dating app where you can post an event you are going to, and find a date to go with you."
- Problem: Solo people want to attend concerts/weddings/dinners with someone
- Who: Singles, 20-40 year olds
- Existing solutions: Tinder (swipe-based, no events), Meetup (groups not 1:1)
- Why inadequate: No event-based 1:1 matching

### Candidate 2: 911 video call capability
**Source**: r/SomebodyMakeThis (18 upvotes)
Post: "911 Should Have FaceTime"
- Problem: 911 callers can't show operators their emergency visually
- Who: Emergency services, general public
- Existing solutions: Some cities experimenting, no mainstream solution
- Why inadequate: Regulatory/infrastructure barriers — NOT buildable as web app

### Candidate 3: Visual thought-destruction journal
**Source**: r/SomebodyMakeThis
Post: "Write a thought, crumple it, drag to virtual trash. Revisit after 1 month to decide burn or keep."
- Problem: Mental health journaling with a "letting go" mechanic
- Who: People dealing with anxiety/overthinking
- Existing solutions: Day One, Notejoy, various journaling apps
- Why inadequate: No "physical destruction" mechanic, no revisit feature
- **Cross-check**: Not in past builds. Interesting but very niche.

### Candidate 4: Co-founder / builder matchmaking
**Source**: r/SomebodyMakeThis (8 upvotes), frequent X posts
Post: "Where do you find like-minded friends who actually want to build things together?"
- Problem: Solo builders feel isolated, can't find co-builders
- Existing solutions: CoFoundersLab, YC co-founder matching, Indie Hackers
- Why inadequate: Low quality matches, no active filtering by what you're building
- **Cross-check**: Not in past builds. But it's a social/matching problem, hard to solve without network effects.

### Candidate 5: AI API cost forecasting for agent workflows
**Source**: HN "Ask HN: How are people forecasting AI API costs for agent workflows?" — 5 pts, 19 comments
- Problem: Teams can't predict what their AI workflows will cost at scale
- **REJECTED**: Overlaps with FlowCost (March 11, 2026 past build)

### Candidate 6: Gen-AI code review assistance
**Source**: HN "Ask HN: How do you review gen-AI created code?" — 6 pts, 10 comments
- Problem: PRs with 500 lines of AI code are hard for humans to review meaningfully
- Who: Engineering teams using Cursor/Claude Code/Copilot
- Existing solutions: GitHub Copilot PR review, CodeRabbit, SonarQube
- Why inadequate: Nothing helps human reviewers understand AI-code intent + decisions
- **Partially overlaps with VibeAudit territory** — skip

### Candidate 7: AI evals tooling for LLM apps
**Source**: HN "Ask HN: How are people doing AI evals these days?" — 30 pts, 37 comments
- Problem: Teams don't have a standard way to evaluate LLM output quality
- Existing solutions: Braintrust, Langsmith, Weights & Biases
- Why inadequate: Too complex, too expensive
- **Cross-check**: Not in past builds. But adjacent to PromptShot territory.

### Candidate 8: Feature request aggregation for indie SaaS
**Source**: Multiple — HN WAYO March 2026 comments, indie hacker Twitter/X, known market gap
- Problem: Feature requests scatter across email, Twitter, support tickets, Reddit. ProductBoard/Canny cost $49-399/month (enterprise-priced). Solo devs with 50-500 users have NO way to systematically collect, deduplicate, and prioritize user feedback.
- Who: Indie hackers, solo SaaS founders, small startup teams (1-5 people)
- Market size: 1M+ active indie hackers globally (Indie Hackers, ProductHunt userbase), growing rapidly
- Existing solutions: 
  - Canny ($49+/mo) — solid but enterprise-priced, complex
  - ProductBoard ($20+/mo/user) — way too enterprise
  - UserVoice (enterprise, $$$)
  - Linear feature flags — for internal teams only
  - Notion/Trello boards — fully manual, no user voting
- Why they fall short: All too expensive or too manual for indie scale
- **Cross-check**: NOT in past builds. Clear gap.

### Candidate 9: Freelancer rate benchmarking
**Source**: Reddit freelancer communities, X
- Problem: Freelancers systematically underprice themselves because there's no good data on market rates by skill/location/experience
- Existing solutions: Glassdoor (employees only), Bonsai blog posts (static), Upwork rates (skewed by race-to-bottom)
- Why inadequate: No real-time, crowdsourced, anonymized rate data
- **Cross-check**: Not in past builds. Interesting.

### Candidate 10: Changelog / release notes writer for indie devs
**Source**: Indie hacker X posts, personal observation
- Problem: Indie devs ship features but write terrible or no changelogs. Good changelogs = better retention/engagement. Writing them is tedious.
- Who: Solo devs, small teams
- Existing solutions: Beamer ($49+/mo), Headway ($29/mo), LaunchNotes ($29+/mo)
- Why inadequate: Expensive, not integrated with GitHub deeply, no AI
- **Cross-check**: Not in past builds. Related to CodeBrief (internal repo summaries) but different (user-facing changelog product)

### Candidate 11: Local service pricing transparency
**Source**: Reddit r/HomeImprovement, r/DIY complaints
- Problem: "Am I being ripped off by this plumber?" No reliable crowdsourced data on what local services actually cost
- Who: Homeowners, renters
- Existing solutions: Thumbtack (leads, not prices), Angi (estimates only), HomeAdvisor
- Why inadequate: No real pricing data, just estimates
- **Cross-check**: Not in past builds.

### Candidate 12: Prompt injection mitigation tooling
**Source**: HN "Ask HN: What are you using to mitigate prompt injection?" — 5 pts
- Problem: AI apps are vulnerable to prompt injection, no standard defense
- Who: Developers building LLM apps
- **Too niche, adjacent to VibeAudit security territory** — skip

### Candidate 13: Personal finance goal tracker with accountability
**Source**: General pattern in r/personalfinance
- Problem: People set financial goals but lose track. Apps like YNAB are complex.
- **Too broad / too many existing solutions** — skip

### Candidate 14: Conference CFP (Call for Proposals) tracker
**Source**: Niche but recurring complaint from speaker community
- Problem: Speakers at tech conferences manually track 30+ open CFPs in spreadsheets
- Who: Conference speakers, developer advocates
- **Too niche** — skip

### Candidate 15: Noise complaint / neighbor dispute documentation tool
**Source**: Reddit landlord/tenant communities
- Problem: People in ongoing disputes with neighbors (noise, boundary, property) need to systematically document incidents for legal purposes
- Who: Renters, homeowners, landlords
- Existing solutions: None purpose-built
- **Cross-check**: Not in past builds. Interesting but niche.

### Candidate 16: "What do my users actually want?" — feedback deduplication
**Source**: HN WAYO thread, Indie Hackers community discussions
- This is essentially Candidate 8 (Feature Request Aggregation) — same problem space

### Candidate 17: AI-assisted onboarding email sequence builder
**Source**: Email marketing communities
- Problem: New SaaS founders don't know how to write good onboarding sequences
- **Too broad** — many email tools do this

### Candidate 18: Job search tracker with AI application assistant
**Source**: LinkedIn layoff posts, Reddit r/cscareerquestions
- Problem: People applying to jobs track applications in spreadsheets. AI could help personalize cover letters at scale.
- Existing solutions: Teal, Huntr, LinkedIn Easy Apply
- **Too crowded** — skip

### Candidate 19: Subscription expense audit for individuals
**Source**: X posts about "paying too much"
- **Overlaps with CancelScore** (past build) and PriceShift — skip

### Candidate 20: AI-powered changelog + public roadmap for indie SaaS
**Source**: Same pain as Candidate 10 but focused on the public-facing aspect
- This merges with Candidate 8 — the feedback board + roadmap + changelog is the full loop

---

## Cross-Reference with PAST_BUILDS.md
Rejected due to overlap:
- Candidate 5 (AI cost simulation) → FlowCost
- Candidate 6 (vibe-coded app review) → VibeAudit
- Candidate 12 (prompt injection) → adjacent to VibeAudit
- Candidate 19 (subscription costs) → CancelScore / PriceShift

Proceeding candidates:
1, 2 (not buildable as web app), 3, 4, 7, **8/16/20 (strongest)**, 9, 10, 11, 15

---

## Top 5 After Filtering

| # | Problem | Uniqueness | Urgency | Size |
|---|---------|------------|---------|------|
| 8 | Feature request hub for indie SaaS | High | High | Large |
| 10/20 | Changelog + roadmap for indie SaaS | High | Medium | Large |
| 9 | Freelancer rate benchmarking | Medium | Medium | Medium |
| 11 | Local service pricing transparency | Medium | Medium | Large |
| 3 | Thought-destruction journal | High | Low | Small |

**Winner: Candidate 8 — Feature Request Hub for Indie SaaS**

The problem is clear, the market is real, the pain is daily, existing solutions are all too expensive. The AI angle (smart deduplication, clustering, impact scoring) creates genuine differentiation.
