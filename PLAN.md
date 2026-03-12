# PLAN.md — Technical Plan

## Product: FeaturePulse
**Tagline**: Know what your users want. Ship what matters.

---

## User Stories (only the ones that matter)

### Founder (Product Owner)
1. As a founder, I can create a project and get a JavaScript snippet to embed on my app in < 2 minutes
2. As a founder, I see a dashboard of all feature requests, sorted by vote count + AI impact score
3. As a founder, I see AI-grouped clusters ("12 users want export features")
4. As a founder, I can change request status: Under Review → Planned → Shipped
5. As a founder, when I mark something "Shipped", all users who upvoted it get an email notification
6. As a founder, I can see which requests come from paying users (if I pass a user tier via the embed)
7. As a founder, I can create a public roadmap page (optional toggle) to show users what's coming

### End User (Your App's User)
1. As a user, I see a "Feature Requests" widget embedded in the app — no separate signup needed
2. As a user, I can submit a feature request with a title and optional description
3. As a user, I can upvote existing requests (before submitting my own)
4. As a user, I receive an email when a request I voted for ships
5. As a user, I can optionally view the public roadmap

---

## Tech Stack

### Frontend (Dashboard + Public Pages)
- **React 18 + TypeScript** — standard, fast
- **Vite** — build tool
- **shadcn/ui + Tailwind CSS** — component library
- **React Router v6** — routing
- **TanStack Query** — data fetching/caching
- **Zustand** — lightweight state management

### Embeddable Widget
- **Vanilla JS + TypeScript** — compiled to single ESM bundle (~15KB gzipped)
- **Shadow DOM** — full style isolation, doesn't affect host page CSS
- **No framework dependency** — loads in <100ms
- PostCSS-generated inline styles

### Backend
- **Node.js + Express + TypeScript** — consistent with stack guidelines
- **Prisma ORM** — type-safe DB queries
- **BullMQ + Redis** — async jobs (AI dedup, email notifications)

### Database
- **PostgreSQL (Supabase)** — managed, free tier, built-in Row Level Security
- **pgvector extension** — for semantic similarity search on request embeddings (AI dedup)

### AI
- **OpenAI text-embedding-3-small** — generate embeddings for request dedup
  - Cost: $0.00002/1K tokens — nearly free
  - Each request embeds title+description (~50 tokens)
- **Claude claude-sonnet-4-6 (via Anthropic SDK)** — for AI clustering summaries and impact scoring
  - Called async via BullMQ — no blocking

### Auth
- **Supabase Auth** — email/password + Google OAuth
- JWT tokens via Supabase session
- Google OAuth: "Sign in with Google" button on signup/login pages

### Email
- **Resend** — transactional email (notification when feature ships)

### Payments
- **Stripe** — subscription billing (Free/Solo/Builder tiers)
- Stripe Checkout for upgrades
- Stripe webhooks to update user tier in DB

### Hosting (recommendation for production)
- Frontend: Vercel
- Backend: Railway
- Redis: Upstash (serverless Redis)

---

## Architecture Overview

```
Your App (host page)
  └─ <script src="https://cdn.featurepulse.com/widget.js" data-project="abc123">
       └─ Widget (Shadow DOM) → POST /api/requests → Backend → Postgres

Founder's Browser
  └─ featurepulse.com/dashboard → React App → GET/PUT /api/... → Backend

Backend (Node/Express)
  ├─ POST   /api/requests           — create request from widget
  ├─ POST   /api/requests/:id/vote  — upvote a request
  ├─ GET    /api/projects/:id/feed  — widget fetches existing requests (for dedup preview)
  ├─ GET    /api/dashboard          — founder dashboard (auth required)
  ├─ PATCH  /api/requests/:id       — update status (auth required)
  ├─ GET    /api/projects/:id/roadmap — public roadmap JSON (public)
  └─ Webhooks: /api/webhooks/stripe

BullMQ Workers
  ├─ embed-request  — generate embedding on new request → store in pgvector
  ├─ dedup-check    — on new request: cosine similarity search → find near-duplicates → merge
  ├─ notify-shipped — when request status → "shipped": send email to all voters
  └─ weekly-digest  — every Monday: email founder with top 5 unresolved requests
```

---

## Data Model

### users
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid()
email       TEXT UNIQUE NOT NULL
name        TEXT
avatar_url  TEXT
stripe_customer_id TEXT
stripe_subscription_id TEXT
tier        TEXT NOT NULL DEFAULT 'free'  -- free | solo | builder
created_at  TIMESTAMPTZ DEFAULT now()
```

### projects
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid()
user_id     UUID REFERENCES users(id) ON DELETE CASCADE
name        TEXT NOT NULL
description TEXT
api_key     TEXT UNIQUE NOT NULL DEFAULT gen_random_uuid()  -- used by widget
widget_config JSONB DEFAULT '{}'  -- accent color, title text, etc.
public_roadmap BOOLEAN DEFAULT false
created_at  TIMESTAMPTZ DEFAULT now()
```

### requests
```sql
id            UUID PRIMARY KEY DEFAULT gen_random_uuid()
project_id    UUID REFERENCES projects(id) ON DELETE CASCADE
title         TEXT NOT NULL
description   TEXT
status        TEXT NOT NULL DEFAULT 'open'  -- open | under_review | planned | shipped | declined
vote_count    INTEGER NOT NULL DEFAULT 1
user_email    TEXT  -- submitter email (optional, for notifications)
user_tier     TEXT  -- 'free' | 'paid' (passed by host app via widget config)
embedding     vector(1536)  -- OpenAI text-embedding-3-small
merged_into   UUID REFERENCES requests(id)  -- NULL = canonical, non-NULL = duplicate
ai_cluster    TEXT  -- AI-assigned cluster label e.g. "Export & Import"
ai_impact     FLOAT  -- 0.0-1.0 AI-calculated impact score
source        TEXT DEFAULT 'widget'  -- widget | email | twitter | manual
created_at    TIMESTAMPTZ DEFAULT now()
```

### votes
```sql
id          UUID PRIMARY KEY DEFAULT gen_random_uuid()
request_id  UUID REFERENCES requests(id) ON DELETE CASCADE
user_email  TEXT NOT NULL
project_id  UUID REFERENCES projects(id)
notified    BOOLEAN DEFAULT false  -- true after "shipped" notification sent
created_at  TIMESTAMPTZ DEFAULT now()
UNIQUE(request_id, user_email)
```

### indexes
```sql
CREATE INDEX idx_requests_project_id ON requests(project_id);
CREATE INDEX idx_requests_status ON requests(status);
CREATE INDEX idx_requests_embedding ON requests USING ivfflat (embedding vector_cosine_ops);
CREATE INDEX idx_votes_email ON votes(user_email);
```

---

## API Design

### Widget API (public, rate-limited by project API key)

**POST /api/v1/requests**
```json
// Request
{
  "project_key": "abc123",
  "title": "Add CSV export",
  "description": "Would love to export my data",
  "user_email": "user@example.com",  // optional
  "user_tier": "paid"  // optional, passed by host app
}

// Response 200
{
  "id": "req_xxx",
  "merged": false,  // true if merged into existing request
  "canonical_id": "req_xxx",
  "vote_count": 4,
  "message": "Thanks! 3 others want this too."
}
```

**GET /api/v1/projects/:key/requests**
```json
// Response - top requests for widget display
{
  "requests": [
    {
      "id": "req_xxx",
      "title": "CSV export",
      "vote_count": 12,
      "status": "planned",
      "user_voted": false
    }
  ]
}
```

**POST /api/v1/requests/:id/vote**
```json
// Request
{ "project_key": "abc123", "user_email": "user@example.com" }
// Response
{ "vote_count": 13 }
```

### Dashboard API (auth required)

**GET /api/v1/dashboard/projects** — list founder's projects  
**POST /api/v1/dashboard/projects** — create new project  
**GET /api/v1/dashboard/projects/:id** — project details + snippet  
**PATCH /api/v1/dashboard/projects/:id** — update settings  
**DELETE /api/v1/dashboard/projects/:id** — delete project

**GET /api/v1/dashboard/projects/:id/requests**
```json
// Response - paginated, filterable
{
  "requests": [...],
  "clusters": [
    { "name": "Export & Import", "count": 18, "top_request": "CSV export" },
    { "name": "UI Improvements", "count": 12, "top_request": "Dark mode" }
  ],
  "total": 47,
  "page": 1
}
```

**PATCH /api/v1/dashboard/requests/:id**
```json
// Request
{ "status": "shipped" }
// Triggers BullMQ notify-shipped job
```

**GET /api/v1/public/roadmap/:projectId** — public roadmap (no auth)

---

## Auth Flow

1. User visits featurepulse.com
2. Clicks "Sign Up Free"
3. Options: Email/password OR "Continue with Google"
4. **Email/password**: submit form → Supabase createUser → confirm email → set password → redirect to dashboard
5. **Google OAuth**: click button → Supabase OAuth flow → Google consent → callback → session set → redirect to dashboard
6. **Edge case**: Google email matches existing email account → Supabase links them automatically
7. Password reset: "Forgot password?" → Supabase sendPasswordResetEmail → link in email → new password form

---

## UI Flow (User Journey)

### Founder Onboarding (first time)
1. **Landing page** → "Start for free" CTA
2. **Signup** (email/Google)
3. **Create your first project** — just a name field (e.g., "My SaaS App")
4. **Get your snippet** — one-line JS embed with API key shown, copy button
5. **Dashboard** — empty state: "Add the snippet to your app and you'll see requests here"

### Founder Daily Use
1. **Dashboard home** — project list with request counts + unread badge
2. **Project view** — request feed sorted by impact score (AI-weighted vote count)
   - Cluster sidebar: grouped by AI topic
   - Filter: All / Open / Planned / Shipped
   - Search bar
3. **Click a request** — full detail: title, description, vote count, submitter tiers, similar requests merged in
4. **Change status** — dropdown: Open → Under Review → Planned → Shipped
5. **Shipped action** — confirmation modal: "Notify X users who voted?" → confirm → BullMQ job fires

### End User (embedded widget)
1. User opens widget (floating button "Share Feedback" in corner of host app)
2. Widget slides up — shows existing top requests (pre-loaded, cached)
3. User can:
   a. Upvote an existing request (click 👍)
   b. Click "Submit new idea"
4. Submit form: Title (required), Description (optional), Email (optional, "to notify you when shipped")
5. On submit: AI dedup runs in background; if near-duplicate found, request is merged and vote count increases
6. Confirmation message: "Thanks! 4 others want this too." or "Got it! You're the first to ask for this."

---

## What We Are NOT Building (Scope Cuts)

- No Gantt charts or sprint planning
- No customer support ticket routing
- No user-facing login (widget is anonymous-friendly)
- No native mobile apps
- No API for external integrations (Zapier, Slack) — v2 feature
- No team member roles (founder is the only user of the dashboard)
- No in-app comments on requests — v2 feature
- No custom domains in free or solo tiers — builder only
- No SAML/SSO (enterprise feature, not this product)
- No changelog page — separate product scope (maybe v2)

---

## GitHub Integration (Solo + Builder tiers only)

Connect GitHub repo → when a PR is merged with title containing "closes #requestId" or a special FeaturePulse tag in the PR body → automatically mark request as "Shipped" → trigger notifications.

Implementation: GitHub App webhook → backend receives PR merge event → parse PR body for tags → update request status.

This is a **killer differentiator**: the loop closes automatically. No manual "mark shipped."

---

## Pricing Tiers

| Feature | Free | Solo ($12/mo) | Builder ($29/mo) |
|---------|------|---------------|------------------|
| Projects | 1 | 3 | Unlimited |
| Tracked voters/mo | 50 | 500 | Unlimited |
| Requests/mo | 50 | Unlimited | Unlimited |
| AI deduplication | ❌ | ✅ | ✅ |
| AI clustering | ❌ | ✅ | ✅ |
| GitHub auto-ship | ❌ | ✅ | ✅ |
| Email notifications | ❌ | ✅ | ✅ |
| Custom domain | ❌ | ❌ | ✅ |
| Weekly digest | ❌ | ✅ | ✅ |
| Remove branding | ❌ | ❌ | ✅ |

---

## Implementation Roadmap

### Phase 1 (MVP, 3-4 days)
1. Auth (Supabase, email + Google OAuth)
2. Project creation + API key generation
3. Widget JS bundle (embed, submit form, upvote)
4. Backend API (create request, vote, list)
5. Founder dashboard (project list, request feed, status update)
6. Basic email notifications (Resend) when request ships

### Phase 2 (AI features, 2-3 days)
7. OpenAI embeddings on request creation
8. pgvector cosine similarity dedup (BullMQ job)
9. AI clustering (Claude) — group requests by topic
10. Impact scoring (weighted: votes × tier_weight × recency)

### Phase 3 (Monetization + Growth, 2 days)
11. Stripe integration (subscription tiers)
12. Public roadmap page
13. GitHub integration (auto-ship)
14. Weekly digest email

### Total estimated effort: 7-9 days for experienced dev
