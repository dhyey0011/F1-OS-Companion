# Next Steps — F1 Companion OS

Status as of 2026-04-06.

## What's Done

### Planning & Design (complete)
- [x] /office-hours — Product design doc (`design/race-engineer-design.md`)
- [x] /plan-design-review — Design system, screen hierarchy, interaction states, anti-slop rules (baked into `README.md` and `DESIGN.md`)
- [x] /plan-ceo-review (partial) — Scope expansion, CEO plan, architecture decisions, tech stack, 3 rounds of adversarial spec review

### Key Decisions Made
- **Phase 1 scope:** Web app + browser extension (not extension-only)
- **Architecture:** Shared core monorepo (packages/core, packages/web, packages/extension)
- **LLM provider:** Ollama (local, localhost:11434) with template fallback
- **Data source:** OpenF1 REST polling (not WebSocket)
- **7 scope expansions accepted** (pre-race briefing, catch-up button, progressive disclosure, tyre predictor, learning card, mood indicator, depth toggle)

## What's Left

### CEO Review (paused at Section 1 of 11)

The /plan-ceo-review was paused after completing:
- Step 0: Premise challenge, scope expansion, mode selection
- Step 0D: All 7 expansion proposals presented and decided
- Step 0D-POST: CEO plan persisted with 3 rounds of adversarial spec review
- Section 1: Architecture review (partial — API key decision made)

**Remaining review sections (2-11):**
- Section 2: Error & Rescue Map — map every failure path
- Section 3: Security & Threat Model — attack surfaces, input validation
- Section 4: Data Flow & Interaction Edge Cases — shadow paths, UI edge cases
- Section 5: Code Quality Review — patterns, DRY, naming
- Section 6: Test Review — test coverage diagram
- Section 7: Performance Review — polling efficiency, memory
- Section 8: Observability & Debuggability — logging, metrics
- Section 9: Deployment & Rollout — Chrome Web Store, feature flags
- Section 10: Long-Term Trajectory — tech debt, reversibility
- Section 11: Design & UX Review — information architecture, states
- Outside Voice — independent second opinion from different AI model

**To resume:** Run `/plan-ceo-review` and it will pick up context from the CEO plan at `docs/CEO-PLAN.md`. Or skip remaining sections and go straight to engineering.

### Engineering Review (not started)

Run `/plan-eng-review` for architecture, data flow diagrams, edge cases, test coverage, and performance analysis. This is the required shipping gate.

### Implementation (not started)

Recommended build order:

#### Week 1: Feasibility + Foundation
1. **Validate F1 TV DOM injection** — Plasmo content script on F1 TV page. If blocked by CSP, pivot to side panel.
2. **Validate polling under tab switching** — test if Chrome throttles timers in backgrounded tabs.
3. **Validate SpeechSynthesis autoplay** — test voice works after user gesture.
4. **Scaffold monorepo** — pnpm workspaces, TypeScript, ESLint, packages/core + packages/web + packages/extension
5. **OpenF1 client** — polling REST client with caching and backoff

#### Week 2: Core Engines
6. **Event engine** — 6 core event types with deduplication
7. **Template engine** — beginner + regular variants for all 6 event types
8. **Ollama integration** — async enrichment with 2s timeout and template fallback
9. **Zustand store** — shared state for timing data, events, settings
10. **Basic leaderboard UI** — P1-P20, gaps, tyre compounds

#### Week 3: Features + Voice
11. **Voice orchestration** — priority queue, SpeechSynthesis, Cards/Voice/Both toggle
12. **Stream sync slider** — display-layer delay on event cards/voice
13. **Tyre strategy predictor** — rules-based 1-stop/2-stop badges
14. **Mood/excitement indicator** — rolling score with color bar
15. **Mid-race depth toggle** — beginner/regular switch

#### Week 4: Expansion Features + Polish
16. **Pre-race briefing** — LLM-generated storylines before session start
17. **Catch-up button** — summarize missed events
18. **Progressive disclosure** — expand cards for deeper explanation
19. **Post-session learning card** — "what you learned" summary
20. **Replay mode** — historical session playback with time scrubber
21. **Extension settings page** — Ollama connection, model selection, preferences

#### Week 5: Testing + Ship
22. **Test suite** — event engine, template engine, strategy predictor, voice queue
23. **Live race test** — test with an actual race session (or replay of recorded session)
24. **Chrome Web Store submission** — prepare listing, screenshots, description
25. **Firefox Add-ons submission** — secondary distribution

### Open Issues to Resolve During Build

1. **Pre-race briefing data source.** OpenF1 doesn't expose championship standings. Need Ergast API or scope briefing to grid + circuit info only.
2. **chrome.storage.sync quota (100KB).** Use chrome.storage.local for session data. Only preferences in sync.
3. **LLM call volume.** Consider ~30 calls/session cap to avoid Ollama overload on slower machines.
4. **Replay 2x/4x speed.** Possibly YAGNI for Phase 1. 1x sufficient for demos.
5. **Service worker keepalive.** If polling moves to Manifest V3 SW, need chrome.alarms strategy.

## Key Files

| File | Purpose |
|------|---------|
| `README.md` | Product brief + design decisions from /plan-design-review |
| `DESIGN.md` | Design system (colors, typography, spacing, components, anti-patterns) |
| `design/race-engineer-design.md` | /office-hours design doc (problem statement, architecture, tech stack, specs) |
| `docs/CEO-PLAN.md` | CEO review plan (vision, scope decisions, feature specs, reviewer concerns) |
| `docs/ARCHITECTURE.md` | System architecture, data flow diagrams, state management, monorepo structure |
| `docs/LANDSCAPE.md` | Competitive landscape research |
| `CLAUDE.md` | AI skill routing rules |

## Phase 2 Features (post-validation)

- Supabase auth + cross-device preferences
- Fan mode selector (Beginner / Regular / Advanced — 3 levels)
- Friend rooms + in-race chat
- Post-race recap generation
- Push notifications for race events
- Full Explore section (driver/team pages)
- ElevenLabs voice upgrade
- Quiet detector (detect when TV commentary covers the moment)
- Circuit-specific tyre compound calibration
- Auto-detect stream sync offset
