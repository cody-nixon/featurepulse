# DESIGN.md — FeaturePulse Final Design Document

## Executive Summary

**The problem**: Indie hackers and solo SaaS founders get feature requests scattered across 5 different channels (email, Twitter, support tickets, Discord, Reddit) with no affordable way to collect, deduplicate, or prioritize them — and existing tools (Canny, ProductBoard) cost $49-399/month, more than many indie founders' entire MRR.

**The solution**: FeaturePulse is a one-line JavaScript embed that places a floating widget in your app, collects feature requests from users with zero friction, uses AI to deduplicate and cluster similar requests, and closes the loop by emailing voters when their request ships.

**Why it's different**: The only feature request tool designed for indie scale — free tier that actually works, AI dedup that Canny doesn't have, GitHub auto-ship integration, and pricing that doesn't exceed an indie founder's first month of revenue.

---

## Target User Persona

**Alex Chen, 31 — Indie SaaS Founder**
- Built a productivity SaaS on weekends: 180 paying users at $9/month ($1,620 MRR)
- Ships every 1-2 weeks using Cursor + Claude Code
- Gets feature requests via: email (daily), Twitter DMs (weekly), Discord (daily), support@email (weekly)
- Current workflow: copy/paste into Notion. 94 items with no pattern visible.
- Pain: doesn't know if "CSV export" is wanted by 2 users or 20. Doesn't know if it's worth building.
- Would pay: Up to $20/month without hesitation for something that solves this

**Secondary persona: Dev shop / micro-agency founder**
- 3-5 products across clients
- Needs multi-project management
- Builder tier ($29/mo) is clearly worth it for professional work

---

## Complete Technical Specification

### Stack
- **Frontend**: React 18 + TypeScript + Vite + shadcn/ui + Tailwind
- **Widget**: Vanilla TS → ESM bundle, Shadow DOM isolation, ~15KB gzipped
- **Backend**: Node.js + Express + TypeScript
- **ORM**: Prisma + PostgreSQL (Supabase)
- **AI**: OpenAI text-embedding-3-small (dedup) + Claude claude-sonnet-4-6 (clustering/scoring)
- **Queue**: BullMQ + Upstash Redis
- **Auth**: Supabase Auth (email/password + Google OAuth)
- **Email**: Resend
- **Payments**: Stripe (subscriptions)

### Architecture
```
Widget (Shadow DOM, host app)
  ↓ POST /api/v1/requests
Backend (Node/Express)
  ↓ Prisma
PostgreSQL (Supabase) + pgvector
  ↓ BullMQ jobs
AI Workers: embed → dedup → cluster → notify
```

### Data Model (key tables)
- `users`: id, email, tier, stripe_*
- `projects`: id, user_id, api_key, widget_config, public_roadmap
- `requests`: id, project_id, title, description, status, vote_count, embedding (vector), merged_into, ai_cluster, ai_impact
- `votes`: id, request_id, user_email (hashed for privacy), notified

### API Surface
**Widget (public)**:
- `POST /api/v1/requests` — submit request
- `GET /api/v1/projects/:key/requests` — list for widget
- `POST /api/v1/requests/:id/vote` — upvote

**Dashboard (auth required)**:
- Projects CRUD
- `GET /api/v1/dashboard/projects/:id/requests` (with clusters)
- `PATCH /api/v1/dashboard/requests/:id` — status update
- `GET /api/v1/public/roadmap/:id` — public roadmap

### Auth Flow
Email/password + Google OAuth via Supabase. JWT tokens. Password reset via email link.

### Security Highlights
- All API keys in environment variables
- Stripe webhook signature verification
- Rate limiting (5 req/min/IP per project key)
- User emails hashed (SHA-256) for privacy; plain stored only for notifications
- Project ownership verified on all mutations
- CORS: allowed-domains list per project

### Pricing
| Tier | Price | Projects | Users/mo | AI Features |
|------|-------|----------|----------|-------------|
| Free | $0 | 1 | 50 | Text dedup only |
| Solo | $12/mo | 3 | 500 | Full AI dedup + clustering |
| Builder | $29/mo | Unlimited | Unlimited | + Custom domain, no branding |

---

## Wireframes

See [WIREFRAMES.md](./WIREFRAMES.md) for full text-based wireframes of all 13 screens:
1. Landing page
2. Sign up
3. Onboarding Step 1 (create project)
4. Onboarding Step 2 (get snippet)
5. Dashboard — project list
6. Dashboard — request feed (core view)
7. Request detail modal
8. "Mark as Shipped" confirmation
9. Public roadmap page
10. Embedded widget (desktop + mobile + success state)
11. Empty state
12. Widget settings
13. Error states + upgrade prompts

---

## Implementation Roadmap

### Week 1 — MVP (3-4 days)
**Day 1**: Auth (Supabase), project creation, API key generation  
**Day 2**: Backend API (create request, vote, list), widget JS bundle  
**Day 3**: Founder dashboard (request feed, status updates)  
**Day 4**: Email notifications (Resend) + Stripe billing  

### Week 2 — AI & Differentiation (2-3 days)
**Day 5**: OpenAI embeddings on request creation, pgvector setup  
**Day 6**: BullMQ dedup job (cosine similarity merge)  
**Day 7**: Claude clustering + impact scoring  

### Week 3 — Growth Features (2 days)
**Day 8**: Public roadmap page, GitHub integration (auto-ship)  
**Day 9**: Weekly digest email, widget customization (color, position, text)  

**Total**: ~9 days for experienced developer

---

## Estimated Effort (Experienced Dev)

| Component | Hours |
|-----------|-------|
| Auth + project management | 8h |
| Widget JS bundle (Shadow DOM, submit, vote) | 10h |
| Backend API | 8h |
| Dashboard UI (React) | 12h |
| AI pipeline (embed, dedup, cluster) | 8h |
| Stripe integration | 6h |
| Email notifications (Resend) | 4h |
| GitHub integration | 6h |
| Public roadmap page | 4h |
| Testing + polish | 8h |
| **Total** | **~74 hours** |

---

## Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Widget breaks host app CSS | Medium | High | Shadow DOM — guaranteed isolation |
| AI dedup creates wrong merges | Medium | Medium | Threshold >0.92, founder can un-merge |
| Canny launches free tier / slashes prices | Low | High | Our AI dedup + GitHub integration are unique |
| Cold start (no users, no data to dedup) | High | Low | Text similarity works without AI for small datasets |
| Email notifications go to spam | Medium | Medium | SPF/DKIM/DMARC from day 1 with Resend |
| Stripe webhook failure | Low | High | BullMQ retry, idempotency keys |
| User privacy (email collection in widget) | Low | Medium | Email optional, hashed in DB, explicit opt-in |

---

## Success Metrics

**Week 1** (launch):
- 50 signups from Product Hunt / Indie Hackers launch
- 10 projects with embed snippet installed
- 5 projects with at least 1 real user request received

**Month 1**:
- 200 total projects created
- 15 paying customers (Solo or Builder)
- $180+ MRR
- NPS > 40

**Month 3**:
- 500 projects, 50 paying
- $600+ MRR
- Top activation metric: time to first user request < 15 minutes

**The single most important metric**: % of projects that receive their first real user request within 48 hours of embedding the widget. If this is >50%, the product is working. If it's <25%, the widget UX needs fixing.

---

## Key Insight: Why This Beats Canny for Indie Hackers

1. **Price**: $0 to start vs $49/month. The free tier is usable, not crippled.

2. **AI dedup**: Canny shows you 17 separate "dark mode" requests. FeaturePulse shows "13 users want dark mode." The difference between noise and signal.

3. **The close-the-loop email**: When you ship something, FeaturePulse emails everyone who asked for it. This is a retention mechanic for *your* users — they get a pleasant surprise. Canny doesn't do this automatically.

4. **GitHub auto-ship**: Connect a GitHub repo and requests auto-close when PRs merge. The developer loop is complete. Nobody has to remember to mark things as shipped.

5. **Built for solo**: No seat-based pricing, no "per user" complexity, no enterprise jargon. One founder. One dashboard. Just the right features.

6. **Embeddable, not a separate site**: Users don't go to "yourapp.canny.io" — they submit right inside your app. Lower friction = more feedback = better signal.
