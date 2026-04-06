# F1 Companion OS — Architecture

## Monorepo Structure

```
f1-os-companion/
  packages/
    core/                   # Shared engines and logic
      src/
        openf1/             # OpenF1 API client (polling REST)
        events/             # Event engine (timing data -> race moments)
        explanations/       # Explanation engine (templates + LLM enrichment)
        voice/              # Voice orchestration (queue, priority, TTS)
        strategy/           # Tyre strategy predictor (rules-based)
        mood/               # Race excitement calculator
        store/              # Zustand store definitions
        types/              # Shared TypeScript types
      __tests__/            # All engine tests live here
    web/                    # Standalone React + Vite app
      src/
        components/         # UI components
        pages/              # App pages (Home, Live, Replay, Settings)
        App.tsx
    extension/              # Plasmo browser extension
      src/
        contents/           # Content scripts (sidebar injection)
        background/         # Service worker (polling, if needed)
        popup/              # Extension popup (settings, status)
        components/         # Extension-specific UI
  pnpm-workspace.yaml
  package.json
  tsconfig.base.json
```

## System Architecture

```
  ┌─────────────────────────────────────────────────────┐
  │                    packages/core                      │
  │                                                       │
  │  ┌──────────────┐  ┌─────────────────┐  ┌──────────┐ │
  │  │ OpenF1Client │  │  Event Engine   │  │ Voice    │ │
  │  │ (polling     │──▶ (detect events  │──▶ Queue    │ │
  │  │  REST, 1-3s) │  │  from timing)   │  │ Manager  │ │
  │  └──────────────┘  └────────┬────────┘  └──────────┘ │
  │                             │                         │
  │                    ┌────────▼────────┐                │
  │                    │ Explanation     │                │
  │                    │ Engine          │                │
  │                    │ (template +    │                │
  │                    │  Ollama async) │                │
  │                    └─────────────────┘                │
  │                                                       │
  │  ┌──────────────┐  ┌─────────────────┐               │
  │  │ Strategy     │  │ Mood/Excitement │               │
  │  │ Predictor    │  │ Calculator      │               │
  │  │ (rules-based)│  │ (rolling score) │               │
  │  └──────────────┘  └─────────────────┘               │
  │                                                       │
  │  ┌──────────────┐                                    │
  │  │ Zustand      │ (shared state store)               │
  │  │ Store        │                                    │
  │  └──────────────┘                                    │
  └───────────────────────┬───────────────────────────────┘
                          │
              ┌───────────┼───────────┐
              │                       │
  ┌───────────▼──────────┐  ┌────────▼──────────┐
  │   packages/web       │  │ packages/extension │
  │   (React + Vite)     │  │ (Plasmo)           │
  │                      │  │                    │
  │   - Standalone app   │  │ - Content script   │
  │   - Full viewport    │  │ - 380px sidebar    │
  │   - localStorage     │  │ - chrome.storage   │
  └──────────────────────┘  └────────────────────┘

  External Dependencies:
  ┌──────────────┐    ┌──────────────┐
  │ OpenF1 API   │    │ Ollama       │
  │ (REST)       │    │ (localhost)  │
  │ openf1.org   │    │ :11434       │
  └──────────────┘    └──────────────┘
```

## Data Flow

### Live Race (Happy Path)

```
OpenF1 REST poll (every 1-3s)
    │
    ▼
OpenF1Client stores raw timing in Zustand
    │
    ▼
Event Engine compares consecutive snapshots
    │
    ├── Position changed?     → Overtake event
    ├── Pit duration appears? → Pit stop event
    ├── Race control message? → Safety car event
    ├── Stint exceeds window? → Tyre degradation event
    ├── DRS field active?     → DRS event
    └── Gap shrinking?        → Gap collapse event
    │
    ▼
Explanation Engine receives RaceEvent
    │
    ├── Template path (instant, <100ms)
    │   └── Interpolate template with event data
    │       └── Render card immediately
    │
    └── LLM path (async, Ollama localhost)
        └── If completes within 2s → update card
        └── If exceeds 2s → template stands
    │
    ▼
Voice Queue (if voice mode enabled)
    │
    ├── Check priority (SC > pit > overtake > ...)
    ├── Check queue (max 2 items)
    └── SpeechSynthesis.speak() or card-only
```

### Data Flow (Error Paths)

```
OpenF1 poll fails
    │
    ├── 1-2 failures → silent retry
    ├── 3+ failures  → "Live data paused" banner
    │                   cached leaderboard shown
    │                   event engine paused
    └── 429 response → exponential backoff

Ollama not running
    │
    └── Template-only mode (zero degradation)
        App works fully without LLM

SpeechSynthesis blocked (autoplay policy)
    │
    └── "Start voice" button required
        Cards continue working
```

## State Management (Zustand)

```typescript
interface F1Store {
  // Timing data
  drivers: Driver[]           // P1-P20 with gaps, tyres, pit status
  lastUpdate: number          // timestamp of last successful poll
  isStale: boolean            // true if data > 10s old
  isConnected: boolean        // OpenF1 connection status

  // Events
  eventFeed: RaceEvent[]      // chronological event history
  lastInteractionAt: number   // for catch-up button

  // Features
  depthMode: 'beginner' | 'regular'
  outputMode: 'voice' | 'cards' | 'both'
  syncOffset: number          // -30 to +120 seconds
  moodScore: number           // 0-20 excitement score

  // Strategy
  predictions: Map<string, StopPrediction>  // driverId -> prediction

  // Replay
  isReplayMode: boolean
  replayTimestamp: number | null
  replaySpeed: 1 | 2 | 4

  // Settings
  ollamaConnected: boolean
  ollamaModel: string
}
```

## Key Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Monorepo approach | pnpm workspaces | Zero duplication between web and extension |
| Data source | OpenF1 REST polling | Free, community-maintained, sufficient for MVP |
| LLM provider | Ollama (local) | Free, no API key management, no backend needed |
| LLM fallback | Template-only | App works fully without Ollama running |
| State management | Zustand | Lightweight, works in extensions, minimal boilerplate |
| Strategy predictor | Rules-based | No LLM needed, deterministic, fast |
| Voice | Browser SpeechSynthesis | Free, no external dependency, test quality early |
| Extension framework | Plasmo | React-based, good DX, handles manifest complexity |
