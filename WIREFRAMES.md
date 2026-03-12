# WIREFRAMES.md — UI/UX Wireframes

All wireframes are text-based. [BUTTON], [INPUT], {section} notation used.

---

## Screen 1: Landing Page

```
┌─────────────────────────────────────────────────────────────────┐
│  HEADER                                                          │
│  ● FeaturePulse        Pricing  Sign In  [Start for free →]     │
├─────────────────────────────────────────────────────────────────┤
│  HERO (full width, light background)                            │
│                                                                  │
│        Know what your users want.                               │
│        Ship what matters.                                        │
│                                                                  │
│  One line of code. Collect feature requests, see patterns,      │
│  close the loop when you ship.                                  │
│                                                                  │
│           [Start for free →]   [See live demo]                  │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  PRODUCT SCREENSHOT / ANIMATED DEMO                     │   │
│  │  Left: Your app with widget floating bottom-right        │   │
│  │  Widget open: showing "Top requests" + submit form       │   │
│  │  Right: FeaturePulse dashboard with request cards        │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Trusted by 340 indie builders on Product Hunt, Indie Hackers  │
├─────────────────────────────────────────────────────────────────┤
│  HOW IT WORKS (3 steps)                                         │
│                                                                  │
│  1. Add one line of JS    2. Users submit requests    3. Ship   │
│  <script src="...">       Widget appears in your app   Close    │
│  That's it.               AI deduplicates dupes        the loop  │
├─────────────────────────────────────────────────────────────────┤
│  SOCIAL PROOF                                                    │
│  "Finally a Canny alternative that doesn't cost more than       │
│   my whole MRR." — @IndieHacker, 89 paying users                │
│  "Shipped dark mode after seeing 14 votes. My churn dropped."   │
│   — @sarahbuilds, founder of NotePad Pro                        │
├─────────────────────────────────────────────────────────────────┤
│  PRICING                                                         │
│                                                                  │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────┐              │
│  │  FREE    │  │  SOLO $12/mo │  │ BUILDER $29  │              │
│  │          │  │              │  │              │              │
│  │ 1 project│  │ 3 projects   │  │ Unlimited    │              │
│  │ 50 users │  │ 500 users    │  │ Unlimited    │              │
│  │ Basic    │  │ AI dedup ✓   │  │ Custom domain│              │
│  │ dedup    │  │ GitHub auto  │  │ No branding  │              │
│  │          │  │ ship ✓       │  │              │              │
│  │[Start]   │  │[Start]       │  │[Start]       │              │
│  └──────────┘  └──────────────┘  └──────────────┘              │
├─────────────────────────────────────────────────────────────────┤
│  FOOTER                                                          │
│  © 2026 FeaturePulse  |  Privacy  |  Terms  |  @featurepulse   │
└─────────────────────────────────────────────────────────────────┘
```

**Copy notes**:
- Hero H1: Large, bold, two lines — short enough to read in 1 second
- "One line of code" — immediately answers "how hard is this to set up?"
- Demo screenshot/GIF is critical — show don't tell
- Social proof: real-feeling quotes with specifics (numbers, names)
- "doesn't cost more than my whole MRR" — direct pain-point acknowledgment

---

## Screen 2: Sign Up Page

```
┌─────────────────────────────────────────────────────────────────┐
│  HEADER  ● FeaturePulse                                         │
├─────────────────────────────────────────────────────────────────┤
│                    Create your account                          │
│                                                                  │
│         [G]  Continue with Google                               │
│                                                                  │
│              ─────────── or ───────────                         │
│                                                                  │
│  [___________________________]  Email                           │
│  [___________________________]  Password (min 8 chars)          │
│                                                                  │
│           [Create account →]                                    │
│                                                                  │
│  Already have an account?  Sign in                              │
│                                                                  │
│  By creating an account, you agree to our                       │
│  Terms of Service and Privacy Policy.                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## Screen 3: Onboarding — Create First Project

```
┌─────────────────────────────────────────────────────────────────┐
│  HEADER  ● FeaturePulse      [Profile menu ▾]                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Welcome to FeaturePulse! 🎉                                   │
│   Let's set up your first project in 2 minutes.                 │
│                                                                  │
│   Step 1 of 2: Name your project                                │
│   ○───────●──────○                                              │
│                                                                  │
│   What are you building?                                        │
│   [________________________________]                            │
│    e.g. "My SaaS App", "NotePad Pro", "BudgetBot"               │
│                                                                  │
│                      [Next →]                                   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Screen 4: Onboarding — Get Your Snippet

```
┌─────────────────────────────────────────────────────────────────┐
│   Step 2 of 2: Add the widget to your app                       │
│   ○────────○───────●                                            │
│                                                                  │
│   Copy this line and paste it into your app's HTML:             │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │ <script src="https://cdn.featurepulse.com/widget.js"    │   │
│  │   data-project="fp_abc123xyz"                           │   │
│  │   async></script>                               [Copy]  │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                  │
│   That's it. The widget will appear in the bottom-right         │
│   corner of your app automatically.                             │
│                                                                  │
│   ┌─────────────────────────────────────────────────────┐      │
│   │  📋 Can't add code right now?                       │      │
│   │  Send to developer →  [Email snippet]               │      │
│   └─────────────────────────────────────────────────────┘      │
│                                                                  │
│              [Go to your dashboard →]                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Note**: "Send to developer" email feature — founder can email the snippet to their dev. Important because many indie founders have a technical helper.

---

## Screen 5: Founder Dashboard — Project List

```
┌─────────────────────────────────────────────────────────────────┐
│  SIDEBAR              │  MAIN CONTENT                           │
│  ● FeaturePulse       │                                         │
│                       │  Your Projects                          │
│  [+ New Project]      │                        [+ New Project]  │
│                       │                                         │
│  My Projects          │  ┌─────────────────────────────────┐   │
│  › My SaaS App ⑫      │  │  My SaaS App                    │   │
│    BudgetBot  ③       │  │  12 new requests  •  47 total   │   │
│                       │  │  3 planned  •  8 shipped        │   │
│  ─────────────        │  │                    [View →]     │   │
│  Account              │  └─────────────────────────────────┘   │
│  Billing              │                                         │
│  Docs                 │  ┌─────────────────────────────────┐   │
│                       │  │  BudgetBot                       │   │
│                       │  │  3 new requests  •  11 total    │   │
│                       │  │  1 planned  •  2 shipped        │   │
│                       │  │                    [View →]     │   │
│                       │  └─────────────────────────────────┘   │
│                       │                                         │
│                       │  ┌─────────────────────────────────┐   │
│                       │  │  + Add another project           │   │
│                       │  │  (Solo: 1 of 3 used)             │   │
│                       │  └─────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

**Copy notes**: 
- "⑫" badge in sidebar for unread count
- Project card shows actionable numbers at a glance (new, total, planned, shipped)

---

## Screen 6: Project View — Request Feed (Core Dashboard)

```
┌─────────────────────────────────────────────────────────────────┐
│  SIDEBAR              │  My SaaS App                            │
│  ● FeaturePulse       │  47 requests  •  12 unread              │
│                       │                                         │
│  My SaaS App ⑫        │  [All] [Open] [Planned] [Shipped]       │
│  BudgetBot            │  [Search requests...]    [Sort ▾]       │
│                       │  ──────────────────────────────────     │
│  ── CLUSTERS ──       │                                         │
│  Export & Import  18  │  ┌──────────────────────────────────┐  │
│  UI / Dark Mode   12  │  │  👍 14  CSV export                │  │
│  Integrations      8  │  │  "Would love to export my data"   │  │
│  Performance       6  │  │  🏷 Export & Import  📊 High      │  │
│  Other             3  │  │  [Open ▾]  [Under Review] [Plan]  │  │
│                       │  └──────────────────────────────────┘  │
│  ── ACTIONS ──        │                                         │
│  Get snippet          │  ┌──────────────────────────────────┐  │
│  Widget settings      │  │  👍 11  Dark mode                 │  │
│  Public roadmap OFF   │  │  "My eyes hurt at night"          │  │
│                       │  │  🏷 UI / Dark Mode  📊 High       │  │
│                       │  │  [Open ▾]           [Plan]        │  │
│                       │  └──────────────────────────────────┘  │
│                       │                                         │
│                       │  ┌──────────────────────────────────┐  │
│                       │  │  👍 9   Zapier integration        │  │
│                       │  │  "Need to connect to my CRM"      │  │
│                       │  │  🏷 Integrations  📊 Medium       │  │
│                       │  │  [Open ▾]                         │  │
│                       │  └──────────────────────────────────┘  │
│                       │                                         │
│                       │  ┌──────────────────────────────────┐  │
│                       │  │  👍 7   API access                │  │
│                       │  │  (3 similar merged) ← AI dedup    │  │
│                       │  │  🏷 Integrations  📊 Medium       │  │
│                       │  │  [Open ▾]         [Plan]          │  │
│                       │  └──────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

**Key design choices**:
- Status change buttons **on the card** — no need to click into detail
- AI cluster sidebar — lets founder filter by topic at a glance
- "(3 similar merged)" — shows AI did work, transparent about dedup
- 📊 Impact score: High/Medium/Low (not a number — simpler to scan)
- Cluster counts in sidebar are live — clicking filters the feed

---

## Screen 7: Request Detail View (Modal)

```
┌─────────────────────────────────────────────────────────────────┐
│  ╔═════════════════════════════════════════════════════════╗    │
│  ║  CSV export                                          ✕  ║    │
│  ║  👍 14 votes  •  Cluster: Export & Import  •  Open      ║    │
│  ║                                                         ║    │
│  ║  Status:  [Open ▾]  [Under Review]  [Planned]  [Ship ✓] ║    │
│  ║                                                         ║    │
│  ║  ─────────────────────────────────────────────────      ║    │
│  ║  Original request (by user@example.com, Mar 1)          ║    │
│  ║  "Would love to export my data as a CSV file so I        ║    │
│  ║   can analyze it in Excel."                              ║    │
│  ║                                                         ║    │
│  ║  3 similar requests merged into this one:               ║    │
│  ║  › "Can you add an export button?" (5 votes)            ║    │
│  ║  › "Export to Excel" (3 votes)                          ║    │
│  ║  › "Download my data" (1 vote)                          ║    │
│  ║  [View all]  [Un-merge]                                 ║    │
│  ║                                                         ║    │
│  ║  Voters (14 total)                                      ║    │
│  ║  7 paying  •  7 free  •  11 with email (will notify)    ║    │
│  ║                                                         ║    │
│  ║  [Mark as Planned]    [🚀 Mark as Shipped]              ║    │
│  ╚═════════════════════════════════════════════════════════╝    │
└─────────────────────────────────────────────────────────────────┘
```

**Design notes**:
- Status in both places: top (current status) + bottom (action buttons)
- Merged requests listed visibly — founder sees the dedup did work
- "7 paying / 7 free" — impact of implementing this by user tier
- "11 with email (will notify)" — clear expectation of what "Ship" will do

---

## Screen 8: "Mark as Shipped" Confirmation

```
┌─────────────────────────────────────────────────────────────────┐
│  ╔════════════════════════════════════════════╗                 │
│  ║  🚀 Mark "CSV export" as Shipped?           ║                 │
│  ║                                             ║                 │
│  ║  11 users will receive an email:            ║                 │
│  ║                                             ║                 │
│  ║  Subject: "Your request shipped! 🎉"         ║                 │
│  ║  ─────────────────────────────────          ║                 │
│  ║  Hi [name],                                  ║                 │
│  ║  You asked for CSV export in My SaaS App.   ║                 │
│  ║  It's live! Try it now →                     ║                 │
│  ║                                             ║                 │
│  ║  Link to feature (optional):                ║                 │
│  ║  [https://myapp.com/dashboard___________]   ║                 │
│  ║                                             ║                 │
│  ║    [Cancel]          [✓ Ship it!]           ║                 │
│  ╚════════════════════════════════════════════╝                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Screen 9: Public Roadmap Page (User-Facing)

```
URL: featurepulse.com/roadmap/my-saas-app
(or custom: myapp.com/roadmap with custom domain on Builder tier)

┌─────────────────────────────────────────────────────────────────┐
│  My SaaS App — What's coming                                    │
│  Powered by FeaturePulse (small, dimmed logo)                   │
│  ─────────────────────────────────────────────                  │
│                                                                  │
│  PLANNED                                                         │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  📋  Dark mode                          👍 11  Planned   │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  📋  CSV export                         👍 14  Planned   │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  SHIPPED ✓                                                       │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  ✅  Mobile app                         👍 22  Mar 2026  │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  Got an idea?                                                    │
│  [Request a feature →]      ← opens the same widget             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Screen 10: Embedded Widget (End User)

### Trigger (floating button in host app)
```
                                    ┌──────────────────┐
                                    │ 💬 Feedback    ↑ │ ← bottom-right
                                    └──────────────────┘
```

### Widget Expanded — Desktop (Slide-up panel)
```
                          ┌────────────────────────────┐
                          │  What would you like next? │
                          │  ─────────────────────     │
                          │  TOP REQUESTS              │
                          │                            │
                          │  👍12 Dark mode        [+] │
                          │  👍 9 CSV export       [+] │
                          │  👍 7 Mobile app    ✓ voted│
                          │  👍 5 Zapier           [+] │
                          │                            │
                          │  ──────────────────────    │
                          │  [+ Submit a new idea]     │
                          └────────────────────────────┘
```

### Widget — Submit New Idea
```
                          ┌────────────────────────────┐
                          │  ← Back                    │
                          │  Share your idea           │
                          │                            │
                          │  What would you like?      │
                          │  [________________________]│
                          │                            │
                          │  Tell us more (optional)   │
                          │  [________________________]│
                          │  [________________________]│
                          │                            │
                          │  Email (optional)          │
                          │  [________________________]│
                          │  Notify me when it ships   │
                          │                            │
                          │        [Submit →]          │
                          └────────────────────────────┘
```

### Widget — Success State
```
                          ┌────────────────────────────┐
                          │                            │
                          │         🎉                  │
                          │                            │
                          │     Got it! Thanks!         │
                          │                            │
                          │  4 others want this too.   │
                          │  We'll email you when it   │
                          │  ships.                    │
                          │                            │
                          │        [Close]             │
                          └────────────────────────────┘
```

**"4 others want this too"** — immediate social validation, makes the user feel heard and not alone.

### Widget — Mobile (Full Screen)
```
┌─────────────────────────────────┐
│  What would you like next?   ✕  │
│  ─────────────────────────────  │
│                                 │
│  TOP REQUESTS                   │
│                                 │
│  👍12  Dark mode         [+]    │
│  👍 9  CSV export        [+]    │
│  👍 7  Mobile app     ✓ voted   │
│  👍 5  Zapier            [+]    │
│                                 │
│  ─────────────────────────────  │
│                                 │
│  [+ Submit a new idea]          │
│                                 │
│  Powered by FeaturePulse        │
└─────────────────────────────────┘
```

---

## Screen 11: Empty State (New Project, No Requests Yet)

```
┌─────────────────────────────────────────────────────────────────┐
│  My SaaS App                                                    │
│  0 requests                                                     │
│  ─────────────────────────────────────────────────             │
│                                                                  │
│              📭                                                  │
│                                                                  │
│         No requests yet.                                        │
│         Once users start submitting, you'll                     │
│         see them here.                                          │
│                                                                  │
│         ┌─────────────────────────────────────┐                │
│         │  Your embed snippet                 │                │
│         │  <script src="..." data-project=    │                │
│         │  "fp_abc123"> </script>  [Copy]     │                │
│         └─────────────────────────────────────┘                │
│                                                                  │
│         [Preview widget →]  ← opens demo widget locally        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Screen 12: Settings — Widget Customization

```
┌─────────────────────────────────────────────────────────────────┐
│  SIDEBAR              │  Widget Settings                        │
│  ● FeaturePulse       │  My SaaS App                            │
│  My SaaS App ⑫  ───── │  ─────────────────────────────────      │
│  Settings             │                                         │
│    Widget settings    │  Widget text                            │
│    Embed snippet      │  [Feedback_____________________]        │
│    Public roadmap     │  (shown on floating button)             │
│    GitHub             │                                         │
│    Notifications      │  Widget title                           │
│    Danger zone        │  [What would you like next?___]         │
│                       │                                         │
│                       │  Accent color                           │
│                       │  [■ #6366F1 ▾]  (indigo default)        │
│                       │                                         │
│                       │  Widget position                        │
│                       │  (●) Bottom right  ( ) Bottom left      │
│                       │                                         │
│                       │  Allowed domain (CORS)                  │
│                       │  [myapp.com_________________________]   │
│                       │                                         │
│                       │  [Save settings]                        │
│                       │                                         │
│                       │  ── LIVE PREVIEW ──                     │
│                       │  ┌─────────────────────────────────┐   │
│                       │  │     [Preview of widget here]    │   │
│                       │  └─────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Screen 13: Error States

### Widget — Project at Capacity (Free tier limit)
```
                          ┌────────────────────────────┐
                          │  Feedback temporarily       │
                          │  unavailable.               │
                          │  Try again soon.            │
                          └────────────────────────────┘
```
(Neutral message — don't expose internal limits to users)

### Dashboard — Upgrade Prompt (Free tier feature locked)
```
┌──────────────────────────────────────────────────────────────┐
│  🔒  AI Deduplication                                         │
│  Automatically merge similar requests. See patterns.         │
│  Available on Solo plan ($12/mo)                             │
│                   [Upgrade to Solo →]                        │
└──────────────────────────────────────────────────────────────┘
```
(Always show, never hide — click to upgrade)

### Widget — Submit Error
```
                          ┌────────────────────────────┐
                          │  Couldn't submit.          │
                          │  Please try again.    [↺]  │
                          └────────────────────────────┘
```

---

## Mobile Layout Considerations

| Screen | Desktop | Mobile |
|--------|---------|--------|
| Dashboard | Sidebar + main | Hamburger menu, stacked cards |
| Request feed | Side-by-side with cluster sidebar | Cluster filter as horizontal scroll chips |
| Request detail | Modal overlay | Full-screen bottom sheet |
| Widget | Floating panel (380px wide) | Full-screen overlay |
| Landing page | 3-column pricing | Vertical stack, horizontal scroll pricing |

**Typography**: No smaller than 14px on any functional text. Mobile CTAs: minimum 44px tap targets.
