# REVIEW.md — Multi-Role Design Review

## Product Review

**Core value prop in one sentence**: FeaturePulse gives indie SaaS founders a one-line embed to collect, deduplicate, and prioritize user feature requests — so you always know what to build next.

**Is this actually solving the problem?**
Yes. The problem is well-defined: scattered feedback, expensive alternatives, no AI dedup. The product directly addresses all three. The one-line embed is the key unlock — it eliminates the friction of users going to a separate site to submit feedback.

**Risks to value prop**:
1. **AI dedup behind paywall (Solo tier)**: On the free tier, requests won't be deduplicated. This could make the free tier feel broken if a founder gets 20 duplicate "dark mode" requests. **Fix**: Run basic text similarity (Levenshtein) on the free tier, save semantic similarity for Solo+. This way free tier still deduplicates obvious dupes.
2. **Widget trust**: Some users won't submit their email (privacy). **Fix**: Email is optional. Anonymous votes are valid. Notification opt-in only happens if email is provided.
3. **Cold start problem**: First project has 0 requests. Empty state must be brilliant. **Fix**: Pre-load 3 "example" requests in the widget to show the founder what it looks like before going live.

**Missing feature for retention**: 
The "close the loop" email (notify users when shipped) is crucial for retention of the founder's users, not just the founder. This should be highlighted on the landing page — it's a genuine differentiator. Users don't just forget about requests if they know they'll be notified.

---

## QA Review

**Most likely things to break**:

1. **Widget cross-origin requests (CORS)**
   - The widget is served from a different domain than the host app
   - Need explicit CORS headers on all `/api/v1/` endpoints
   - Must allow the host app's domain to POST requests
   - **Fix**: CORS whitelist based on project's registered domain (add to project settings) OR allow all origins with rate limiting as the defense

2. **Embedding collision / duplicate votes**
   - Same user submits same request twice (page refresh, double-click)
   - Same user votes twice for same request
   - **Fix**: `UNIQUE(request_id, user_email)` constraint on votes table (already in schema). For anonymous users, use localStorage to track voted request IDs per project.

3. **Email notification failure on "shipped"**
   - If BullMQ job fails, voters don't get notified
   - **Fix**: BullMQ retry logic (3 retries with exponential backoff). Log failures. Dead letter queue for manual inspection.

4. **pgvector dedup creates wrong merges**
   - "Add export" and "Add dark mode" might both have "Add" in the title and somehow cluster together
   - **Fix**: Cosine similarity threshold must be high (>0.92). Manual founder override: "Un-merge" button in dashboard. Never auto-merge without surfacing to founder for confirmation.

5. **Widget loads slowly, blocking host app**
   - **Fix**: Load widget script with `async` attribute. Widget renders in Shadow DOM — no layout shift. Set explicit width/height on container.

6. **Rate limiting**
   - Malicious actor could flood a project with fake requests
   - **Fix**: Rate limit by IP: max 5 requests per minute per project key. Redis-based sliding window counter.

7. **Stripe webhook replay attacks**
   - **Fix**: Verify `stripe-signature` header on every webhook. Already noted in PLAN.md.

**Edge cases to test**:
- Submitting a request with an emoji in the title
- Very long description (>5000 chars) — add client + server validation
- Project with 0 requests (empty state)
- User who voted on a request that later gets merged — notification should still fire
- Founder deletes a project — cascade delete requests + votes (already in schema)
- Free tier user hits 50-request limit — graceful error in widget: "This project is at capacity."

---

## Engineering Review

**Is this the simplest way to build it?**

Mostly yes. A few potential over-complications:

1. **pgvector for dedup is potentially overkill for MVP**
   - 1000 requests in a project = tiny dataset. Simple TF-IDF cosine similarity in Node.js is enough.
   - **Recommendation**: Start with simple string similarity (fuse.js + Levenshtein) for MVP. Add pgvector in Phase 2 when you have enough data to validate the approach. Don't add infrastructure complexity before you need it.
   - BUT: If we're designing (not building), pgvector is the right long-term choice. Keep it in the plan.

2. **BullMQ for just 3 job types is overkill in early days**
   - For MVP, could use `setImmediate()` for fire-and-forget background tasks
   - BUT: When you have 500 projects running, you need proper queuing. Design it right from the start.

3. **Shadow DOM for widget** — correct choice. No alternative is as clean for style isolation.

4. **Supabase RLS** — should be used on all tables. The plan mentions it but doesn't specify the policies.
   - **Policy needed**: Users can only read/write their own projects. Requests for a project can be read by anyone with the API key.

5. **One concern on the data model**: `merged_into UUID REFERENCES requests(id)` — this creates a "canonical" request system. What if the canonical gets merged into another? You'd have a chain: req_3 → req_2 → req_1. Need to dereference to the canonical always. **Fix**: Add a `getCanonical(requestId)` helper that walks the chain.

---

## Design Review

**First-time user lands on landing page — do they understand what to do in 5 seconds?**

Current plan: The landing page needs to make this obvious immediately.
- **Above the fold**: Product name, tagline, and a single screenshot/demo showing the widget embedded in an app + the dashboard.
- **Hero CTA**: "Add to your app in 2 minutes — Start free" — this sets a specific expectation
- **Social proof**: "Used by 200 indie hackers" (even if it's just early access number)

**Widget UX concerns**:
1. "Share Feedback" floating button — where exactly? Bottom-right (like Intercom) is standard. Don't override the host app's existing bottom-right widget (Intercom, etc.). **Fix**: Widget position should be configurable (bottom-left, bottom-right, or custom).
2. Widget title should be customizable: "Request a Feature" / "Share Feedback" / "What would you like next?"
3. Empty state (no existing requests): Show a message like "Be the first to share an idea!" — not just a blank widget.

**Dashboard UX concerns**:
1. The AI cluster sidebar is potentially confusing if the clusters are wrong. Need "rebuild clusters" button.
2. Status change is the most important action — make it obvious. Don't bury it in a dropdown inside a detail view. Surface it as a button group on the request card itself.

**Mobile considerations**:
- Dashboard: Responsive, but mobile is secondary. Founders mostly use desktop.
- Widget: MUST be mobile-first. Most users will submit requests on mobile. Full-screen slide-up on mobile, not a side panel.

---

## Security Review

**Attack surface**:
1. **Widget API key exposed in public JS** — API key (project key) is visible in the page source. This is intentional (like Stripe's publishable key) but needs rate limiting to prevent abuse.
2. **User email in votes table** — PII. Need to comply with GDPR/CCPA. Privacy policy must state how emails are used. Emails should be hashed before storage OR encrypted at rest. **Recommendation**: Store email hash (SHA-256) for deduplication, store plain email separately for notifications only.
3. **Stripe webhook** — already in plan (verify signature). Good.
4. **API keys in env vars** — must never be in source code. Anthropic, OpenAI, Stripe, Resend keys all in env vars.
5. **SSRF risk** — no URL fetching in this app. Not a concern.
6. **SQL injection** — Prisma parameterizes all queries. Safe.
7. **Rate limiting** — already noted in QA section. Must implement.
8. **Founder dashboard auth** — all `/api/v1/dashboard/*` routes must verify Supabase JWT. Never trust client-provided user_id.
9. **Project ownership check** — when founder updates a request, verify that request belongs to one of their projects. Don't just check request_id, check project_id ownership.
10. **GitHub OAuth for integration** — store GitHub access token encrypted in DB, not plain text.

---

## Revisions to PLAN.md Based on Review

1. ✅ Add simple Levenshtein dedup for free tier (don't leave free tier with broken dedup)
2. ✅ Add CORS policy: allow-listed domains OR all-origins + IP rate limiting
3. ✅ Clarify RLS policies in data model
4. ✅ Add `getCanonical()` helper to data model notes
5. ✅ Widget position: make configurable (bottom-left/right)
6. ✅ Widget is mobile-first (full-screen on mobile)
7. ✅ Email stored hashed for privacy; plain for notifications only
8. ✅ Status change surfaced on request card (not buried in detail view)
9. ✅ Empty state: pre-show example requests for new projects
10. ✅ AI dedup threshold: >0.92 cosine similarity, with founder merge confirmation
