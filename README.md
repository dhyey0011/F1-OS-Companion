# F1 Companion OS

## Overview

F1 Companion OS is a companion-first Formula 1 app built for average watchers, new fans, and regular race-day viewers who want one place to follow the sport without juggling multiple apps.

The product is designed around a simple fan need:

> "I do not want multiple apps. I want one stop shop for everything. It should show me the race, also show me stats, explain what is going on, and let me interact with my friends."

The first version is a second-screen companion that works alongside F1 TV or any broadcaster stream. The long-term product direction keeps a clean path toward a future streaming-integrated version if licensing becomes possible.

## Product Vision

Build the default companion app for Formula 1 fans by combining four things in one product:

- Live race following
- Plain-English race explanation
- Stats and context on demand
- Social interaction with friends

This is not meant to be another schedule widget app or another raw data dashboard. The core idea is to make F1 easier to follow in real time while still giving deeper information when users want it.

## Target Users

### New fans

People who enjoy the spectacle of F1 but often do not understand strategy, tyres, pit windows, penalties, safety car impact, or why one overtake matters more than another.

### Regular race watchers

Fans who watch most weekends and want a faster, cleaner way to track the race, understand major events, and discuss the action with friends.

### Advanced fans

Users who already understand the sport well but still want a better live companion, stronger context, and friend features in one place.

## Problem Statement

Current F1 fans often piece together their experience across multiple surfaces:

- One app for video
- One app for stats
- Social media or chat for discussion
- Articles or commentators for explanations

This is fragmented, especially for newer fans. Most existing products are either too shallow, too data-heavy, or too narrowly focused on one function such as widgets, fantasy, or live timing.

## Market Gap

The current market is crowded in:

- Race schedules and countdown apps
- Standings and widget apps
- Fantasy and prediction products
- Raw live timing and data companions

The market is much less convincing in:

- Plain-English live explanation for casual fans
- A single app that combines race context, stats, and social features
- Progressive depth for different fan levels in one product
- Post-race summaries that explain what actually decided the result

The gap is not "more data." The gap is "better understanding in one place."

## Product Positioning

F1 Companion OS should be positioned as:

> The all-in-one F1 companion that helps fans follow the race, understand the strategy, and watch with friends.

This combines two ideas:

- Companion OS: the main product shell
- AI explainer: the core wedge that makes the app different

## Product Design

### Core principles

- Companion first, not streaming first
- Explanation is built into the live experience, not hidden in a chatbot tab
- Stats are progressive, not overwhelming
- Social features stay close to the live race context
- Architecture must support future streaming integration without a rebuild

### Primary app surfaces

#### 1. Home

The home screen gives users a clear weekend entry point:

- Upcoming sessions in local timezone
- Favorite drivers and teams
- Weekend storylines
- Quick links to live session, stats, and friend rooms

#### 2. Live Race

This is the most important screen in the product.

It should combine:

- Running order
- Time gaps and intervals
- Tyre compounds and stint context
- Pit stop status
- Safety car, yellow flag, and incident markers
- Plain-English event cards explaining what is happening and why it matters

Example event cards:

- "Piastri pits now to protect against the undercut."
- "The safety car helps drivers who still need to stop."
- "This medium tyre stint is longer than expected, which may limit late-race pace."

The live screen should answer two questions by default:

- What is happening right now?
- Why does it matter?

#### 3. Explore

This section gives users deeper information without cluttering the main race flow.

It includes:

- Driver pages
- Team pages
- Circuit guides
- Standings
- Historical context
- Explainers for rules, tyres, DRS, qualifying formats, and penalties

#### 4. Friends

This is the social layer inside the product.

It includes:

- Private friend rooms
- Group chat
- Reactions during sessions
- Polls and predictions
- Shared post-race discussions

The goal is to keep users inside the race experience instead of pushing them to external chat apps.

#### 5. Recap

After each race, the app generates a short, clear recap explaining:

- The moments that changed the race
- Key strategy calls
- Winners and losers from safety cars or pit timing
- Driver-specific storylines

This is especially useful for new fans and for users who missed part of the session.

## AI Explainer Layer

The explainer system is the main differentiator.

It should not behave like a generic chatbot. Instead, it should be embedded across the app:

- Event cards during live sessions
- Context chips on stats screens
- Tap-to-explain interactions for tyres, strategy, penalties, and gaps
- Session summaries before and after races

### Adaptive fan modes

The same app should adapt explanation depth based on fan level:

- Beginner mode: simple, plain-English explanations
- Regular mode: more race-context detail
- Advanced mode: denser strategic and technical framing

This allows one product to serve all audience groups without becoming unusable.

## Migration Path To Streaming-Integrated

The first version should assume users are watching the race elsewhere.

To preserve a future path to integrated streaming:

- Keep the live race view as the main product surface
- Treat video as a future module that can be docked into the live screen
- Build the event engine, social graph, personalization, and recap system independently of any video source
- Sync around session events and timestamps rather than tying the app to a single video provider

This means the future streaming version can add video into the existing experience rather than redefining the product from scratch.

## MVP Features

The MVP should stay tightly focused on the core use case: helping users follow and understand a live race with friends.

### MVP feature set

- User onboarding with favorite drivers, teams, and fan level
- Race weekend schedule and session countdowns
- Live race screen with leaderboard, gaps, tyre compounds, and pit activity
- Plain-English live explanation cards for major race events
- Driver and team quick stats
- Beginner-friendly rule and strategy explainers
- Private friend rooms
- In-app group chat during sessions
- Lightweight polls or race predictions
- Post-race recap with top moments and strategy summary
- Push notifications for major session events and race start

### Features explicitly out of MVP

- Hosting licensed live race video
- Full fantasy product
- Deep historical database product
- Advanced telemetry-grade analysis tools
- Broad social network features beyond private groups

## Why The MVP Works

This MVP is strong because it solves the most common fan pain points in one experience:

- It reduces fragmentation
- It helps new fans understand the sport
- It still serves regular watchers
- It creates habit around race weekends
- It leaves room for premium upgrades later

## Possible Monetization

Potential monetization options after the MVP:

- Freemium app with premium live insights
- Premium friend-room features
- Advanced recap and personalized insight tier
- Sponsored race-weekend content
- Future bundle with licensed streaming if rights become available

## Product Summary

F1 Companion OS is an all-in-one Formula 1 companion product built around a real market gap: fans do not need yet another F1 stats app, they need a better way to follow, understand, and share the sport in one place.

The MVP should focus on:

- Live race companion
- Embedded AI explanations
- Stats that are useful but not overwhelming
- Friend interaction during sessions
- A product architecture that can later absorb streaming

---

## Design Decisions (from /plan-design-review — 2026-04-06)

### Platform & Tech Stack

- **MVP:** Web app (React + Vite) + browser extension (Plasmo framework)
- **Extension:** Sidebar overlay on F1 TV — the primary demo vehicle for early users
- **Live data:** OpenF1 API (openf1.org) — free, WebSocket live timing, REST historical
- **Backend:** Supabase (auth, friend rooms, realtime)
- **Mobile:** React Native / Expo — post-MVP, after PMF

### Design System

**Colors (dark-first):**
```
Background:  #0a0a0a
Surface:     #141414
Border:      #1f1f1f
Primary:     #ffffff
Secondary:   #999999
Accent:      Team color (badge/dot only — never as background fill)
Error:       #ff4444
Success:     #00c853
```

**Typography:**
- Timing data (gaps, lap times, positions): JetBrains Mono
- UI text (headings, body, nav): DM Sans or Barlow
- Event cards: DM Sans — plain English, readable at a glance

**Spacing:** 4px base (dense data)
**Border radius:** 4px rows, 8px cards, 0px timing displays

### Screen Hierarchy

| Screen | Primary (first attention) | Secondary | Tertiary |
|--------|--------------------------|-----------|----------|
| Home (pre-race) | Next session + countdown | Favorite driver card | Weekend storylines |
| Home (live) | LIVE pill → jump to race | Friend room activity | Schedule |
| Home (off-season) | Latest recap card | Driver/team updates | Trending stats |
| Live Race | Running order (P1-P20) | Gap to car ahead | Tyre/pit status per row |
| Live Race (event) | AI event card (floats top, 8-12s) | Updated running order | Context chips |
| Explore | Search bar | Browse by driver/team | Featured explainer |
| Friends | Active room (if any) | Race-contextual CTA | Past discussions |
| Recap | Key moment headline | Race verdict (3 lines) | Moment-by-moment |

### Interaction States

| Feature | Loading | Empty | Error | Success | Partial |
|---------|---------|-------|-------|---------|---------|
| Live Race leaderboard | Skeleton rows (P1-P20) | "No race active" + next session | "Can't connect" + retry | Live data | Stale indicator (Xm old) |
| AI Event cards | Skeleton card with team color + event type label (pulsing) | No card shown | Raw event fallback | Explanation card | Abbreviated if AI slow |
| Friend rooms | Loading spinner | Race-contextual CTA: "[Race] is live — watch with friends" | Retry + offline mode | Room list | Room with pending invites |
| Home | Skeleton cards | Off-season: last recap + next race date | Partial content shown | Full weekend view | Some sessions loaded |
| Recap | "Generating recap..." | Not applicable (post-race only) | "Recap coming soon" | Full recap | Key moments only |

### AI Explainer Layer

- **Beginner mode:** More frequent AI event cards + inline jargon tooltips (e.g., "DRS" tappable → tooltip)
- **Regular/Advanced mode:** Fewer cards, denser data, no tooltips by default
- **Card lifecycle:** Floats at top of Live Race view → auto-dismiss after 8-12s → collapses to scrollable event feed below leaderboard
- **Loading state:** Skeleton card with team color + event type label, pulsing body area
- **Simultaneous events:** Priority TBD (eng review decision)

### Connection & Data Freshness

- **Connection drop:** Persistent "Live data paused — reconnecting..." banner at top, leaderboard shows last-known positions with timestamp
- **Data latency:** OpenF1 has ~1-3s delay — acceptable for MVP. Display staleness indicator if data is >10s old.
- **Update rate:** Only animate rows where position changed — do not re-render all 20 rows on every update

### Anti-Slop Constraints

- **No 3-column card grids with team-color border-left accents** — the #1 sports app template pattern
- Tyre compounds use official labels (S/M/H + color) — never custom icon shapes
- Driver/team pages: lead with "This Weekend" context (qualifying position, tyre strategy, recent form) — career stats are secondary
- Leaderboard rows feel like timing screens, not social feed items
- Friend rooms: message-first UI, not avatar grid
- **No centered-everything layout** — data tables are left-aligned

### Browser Extension Specifics (sidebar layout)

- Sidebar width: ~380px, right side of screen
- Collapse behavior: TBD (deferred — needs eng review)
- Primary viewport: 1280px+ desktop
- Mobile web: 375px single-column (supported, not primary)

### Responsive & Accessibility

- WCAG AA color contrast on all text (minimum 4.5:1 against dark backgrounds)
- Tyre compounds: MUST show letter (S/M/H) — color alone is not sufficient (colorblind users)
- Touch targets: 44px minimum (mobile web)
- Screen reader: race event announcements throttled to major events only (not every gap update)
- Keyboard navigation required for extension sidebar

### Deferred Design Decisions

| Decision | Deferred reason |
|----------|----------------|
| Fan level change mid-session | Implementation detail — define during build |
| Post-race recap generation timing | Product decision — define with first race test |
| Friend room capacity | Scale decision — define post-launch |
| Event card priority (simultaneous events) | Eng review decision |
| Extension sidebar collapse behavior | Eng review — depends on extension framework |
| Nav Live tab behavior during race | Define during implementation |

---

## GSTACK REVIEW REPORT

| Review | Trigger | Why | Runs | Status | Findings |
|--------|---------|-----|------|--------|----------|
| CEO Review | `/plan-ceo-review` | Scope & strategy | 0 | — | — |
| Codex Review | `/codex review` | Independent 2nd opinion | 0 | — | — |
| Eng Review | `/plan-eng-review` | Architecture & tests (required) | 0 | — | — |
| Design Review | `/plan-design-review` | UI/UX gaps | 1 | issues_open | score: 2/10 → 7/10, 12 decisions |
| DX Review | `/plan-devex-review` | Developer experience gaps | 0 | — | — |

**VERDICT:** Design Review run — eng review required before shipping.
