# REPORT.md — Daily Design Report

## Date
March 12, 2026 — 5:00 AM

## The Problem

**Feature request management is a solved problem for enterprises, but completely broken for indie hackers.**

**Source signals**:
- Hacker News "Ask HN: What Are You Working On? (March 2026)" — 285 pts, 1098 comments (indie builders discussing product-market fit discovery)
- r/SomebodyMakeThis — developer asking "what small annoying problem would you want an app for?" — upvoted answers include "remembering things you promised to do" (tracking promises/requests)
- Twitter/X search for "frustrated with" and "startup idea productivity" — consistent pattern: founders drowning in scattered feedback
- Pattern observed across HN WAYO, Indie Hackers, and founder Twitter: "I don't know which feature to build next"
- Market validation: Canny ($49+/mo), ProductBoard ($20/user/mo), UserVoice (enterprise) — all too expensive for sub-$2K MRR founders

## Why I Chose This

**5 specific reasons**:

1. **Daily pain**: Every indie founder with users experiences this every day. Not an occasional problem — a persistent one. Every tweet, email, and support reply is potential signal being lost.

2. **Clear market gap**: The pricing gap between "nothing" and "Canny ($49/mo)" is enormous. Founders at $500-3000 MRR need a real solution, not a spreadsheet.

3. **AI creates genuine new value**: AI deduplication isn't just "cheaper Canny" — it's categorically better. Showing "13 users want dark mode" instead of 13 separate tickets is a qualitative improvement in information quality.

4. **The close-the-loop email is a retention mechanic for their users**: This is the big idea that makes it more than a feedback board. When users get an email "your request shipped!", it creates a moment of genuine delight. It's a loyalty driver.

5. **Not in past builds**: Completely clean. No overlap with anything previously designed.

## Design Summary

**Product**: FeaturePulse  
**Tagline**: Know what your users want. Ship what matters.

**Core loop**:
1. Founder embeds 1 line of JS in their app
2. Users click "Feedback" button → submit requests, upvote existing ones
3. AI deduplicates similar requests, clusters by topic
4. Founder sees "13 users want dark mode" — not 13 separate tickets
5. Founder marks request "shipped" → FeaturePulse emails all 13 voters automatically

**Tech**: React + TypeScript frontend, Node/Express backend, Supabase (PostgreSQL + auth), pgvector for semantic dedup, Claude for clustering, Stripe for billing, Resend for email

**Pricing**: Free (1 project, 50 users) / Solo $12/mo / Builder $29/mo

**Estimated build**: ~74 hours for experienced dev

## GitHub Repo
https://github.com/cody-nixon/featurepulse

## The Unique Insight

**Every other feature request tool thinks the founder is the user. FeaturePulse knows the founder's users are also users.**

The "shipped" notification email isn't just a nice-to-have — it's a retention feature for the founder's SaaS. When someone's request gets built and they get a personalized email, they feel heard. Heard users churn less. This makes FeaturePulse not just a tool for the founder to organize feedback, but a tool that actively improves their product's retention.

No other tool in this space makes this the centerpiece. Canny can technically send notifications, but it requires manual triggering, custom email setup, and assumes you're on the $49+ tier. FeaturePulse makes it the default behavior from day 1.

## Competitive Positioning
```
                          HIGH AI VALUE
                               │
         [FeaturePulse]        │        [ProductBoard]
              •                │              •
              │                │
    INDIE ────┼──────────────────────────── ENTERPRISE
    SCALE     │                │
              │                │
           [Notion]            │          [Canny]
              •                │              •
                               │
                         LOW AI VALUE
```

FeaturePulse occupies the empty quadrant: indie-scale + high AI value.
