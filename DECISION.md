# DECISION.md — Problem Selection

## Chosen Problem
**Feature request aggregation and prioritization for indie SaaS builders**

## The Core Pain (in 1 sentence)
Indie hackers and solo SaaS founders have user feedback scattered across 5 inboxes and no affordable way to collect, deduplicate, and prioritize it — so they either ignore users or spend hours manually organizing feedback.

---

## Why This Problem Wins

### The Scoring Table

| Criterion | Score (1-5) | Notes |
|-----------|-------------|-------|
| Urgency | 5 | Every indie hacker with paying users feels this daily |
| Impact | 4 | 1M+ indie hackers globally, growing fast with vibe coding wave |
| Novelty | 4 | Tools exist (Canny) but all wrong for this segment |
| Feasibility | 5 | Standard web app, no exotic tech required |
| Monetization | 5 | Clear B2B value, obvious upgrade path |
| Bookmark test | 5 | Would check daily for new user requests |
| Uniqueness from past builds | 5 | Zero overlap with any past build |

**Total: 33/35**

---

## Deep Reasoning

### Who Is This Person?
**Alex**, 28, built a SaaS on weekends. 120 paying users at $9/month. Ships every week. Users love it but keep emailing random feature requests. Alex's workflow:
1. Someone emails "can you add CSV export?"
2. Alex makes a note in Notion
3. Two weeks later, same request via Twitter DM
4. A week after that, someone posts it in the Discord
5. A month later, Alex ships it — but never told anyone
6. Alex has 80 feature requests in Notion with no way to know which matter most

Alex would pay $15/month without hesitation for a tool that:
- Embeds a request widget in the app (1 line of code)
- Deduplicates "CSV export" across 7 channels
- Shows "12 users want this"
- Lets Alex mark it "shipped" and auto-notify all 12 users

### Why Existing Solutions Fail This User

**Canny**: 
- Free tier: 200 tracked users max, forced Canny branding, no integrations
- Paid: $49/month — painful when you're making $1,080/month ARR
- UX: Complex, designed for product managers at 50-person companies

**ProductBoard**: $20/user/month — designed for enterprise product teams
**UserVoice**: Enterprise only, $15K+/year
**Linear**: Internal team tool, users can't submit requests
**Notion**: Fully manual, no user-facing widget, no voting, no dedup

**The Gap**: No tool built specifically for indie hackers / solo founders with <$500 MRR who need 80% of Canny's features at 10% of the price.

### The AI Angle (Why This Is Different From "Just Cheaper Canny")
1. **AI deduplication**: "dark mode", "dark theme", "can you add a dark background?" → all merged to 1 request, "12 users"
2. **AI sentiment analysis**: Detect frustrated requests vs feature requests vs bug reports
3. **Impact scoring**: Weight requests by user tier (paying vs free), engagement, churn risk
4. **Auto-clustering**: "UI improvements" cluster, "Export/import" cluster, "Integrations" cluster
5. **Natural language search**: "What do users want for onboarding?" → shows all onboarding-related requests

These features don't exist in Canny even at their paid tier.

### Why Now?
- Vibe coding wave (Cursor, Claude Code, Lovable, Bolt) is producing more indie SaaS products than ever
- Indie hacker community is exploding — Product Hunt, Indie Hackers, X/Twitter all full of solo builders
- AI tools make building fast, but user retention/product-market fit still requires listening to users
- No good tool serves this segment

### The Bookmark Test
Would Alex check FeaturePulse tomorrow? **Yes.** The moment you have users, you want to know what they want. This dashboard is the first thing Alex opens on Monday morning.

### Monetization Clarity
- **Free**: 1 project, 50 tracked users, 50 requests/month — enough to try
- **Solo ($12/mo)**: 3 projects, 500 tracked users, unlimited requests, AI dedup, GitHub integration
- **Builder ($29/mo)**: Unlimited projects, unlimited users, custom domain, Slack notifications, priority support
- Revenue model: simple, clear value steps

---

## Rejected Alternatives (Why They Lost)

**Changelog writer** (Candidate 10): Solves the same user's problem but at a different moment. Also, CodeBrief (past build) partially covers repo summaries. The changelog is a feature of this product, not a standalone product.

**Freelancer rate benchmarking** (Candidate 9): Network effects problem — needs data to work. Cold start problem is severe. Glassdoor took years to build their dataset. For an indie builder, this is too hard.

**Local service pricing** (Candidate 11): Also a network effects problem. Data collection is the hard part. Not solvable with a clean web app design.

---

## Product Name
**FeaturePulse** — feel the pulse of what your users want.

Tagline: *Know what your users want. Ship what matters.*

Alternative names considered: PulseBoard, RequestHub, VoxBoard, UserPulse
→ FeaturePulse wins: memorable, descriptive, brandable

---

## What This Is NOT
- Not a full product management suite (no Gantt charts, no sprint planning)
- Not an internal tool (users interact with it, not just the founder)
- Not a customer support system (no ticket routing, no SLA tracking)
- Not enterprise-grade (no SSO, no audit logs, no compliance tooling)

It is exactly one thing: **the simplest possible way for users to submit feature requests, for founders to see patterns, and to close the loop when something ships.**
