# F1 Companion OS — Design System

> Stub generated from /plan-design-review (2026-04-06).
> Run /design-consultation for a complete system.

## Visual Tone

Dark-first, data-dense, precision aesthetic. Think cockpit display, not social app.
Team colors are accent elements only — never used as background fills.

## Color System

```css
/* Core surfaces */
--color-bg:         #0a0a0a;
--color-surface:    #141414;
--color-border:     #1f1f1f;

/* Text */
--color-text-primary:   #ffffff;
--color-text-secondary: #999999;
--color-text-muted:     #555555;

/* State */
--color-error:    #ff4444;
--color-success:  #00c853;
--color-warning:  #ff9800;

/* Team colors — use for badges/dots ONLY */
/* Never as background fills. Apply sparingly. */
```

## Typography

| Use case | Font | Notes |
|----------|------|-------|
| Timing data (gaps, lap times, positions) | JetBrains Mono | Monospace for scannable alignment |
| UI text (headings, body, nav) | DM Sans or Barlow | Geometric sans — not Inter/Roboto/Arial |
| Event card body | DM Sans | Plain English, readable at a glance |

## Spacing

Base unit: **4px**

| Token | Value |
|-------|-------|
| `space-1` | 4px |
| `space-2` | 8px |
| `space-3` | 12px |
| `space-4` | 16px |
| `space-6` | 24px |
| `space-8` | 32px |

## Border Radius

| Context | Radius |
|---------|--------|
| Data rows (leaderboard) | 4px |
| Cards (event cards, stat blocks) | 8px |
| Timing displays | 0px |
| Buttons | 6px |

## Component Vocabulary

Core atoms for this product — do NOT invent new patterns without checking here first.

### Driver Row (leaderboard)
- Position number (monospace, right-aligned)
- Team color dot (4px circle, accent only)
- Driver abbreviation (3 letters, caps, primary color)
- Gap to leader (monospace, secondary color)
- Tyre badge: letter (S/M/H) + compound color
- Pit status: small indicator if in pit lane

### Tyre Badge
- **Always** show letter: S (red), M (yellow), H (white)
- Letter MUST be visible — color alone fails colorblind users
- 20px × 20px minimum

### AI Event Card
- Loading state: skeleton with team color left accent + pulsing body
- Dismissed state: collapses to compact entry in event feed
- Active state: floats above leaderboard, auto-dismisses after 8-12s
- Copy style: plain English, past tense for completed events, present tense for ongoing

### Connection Status Banner
- Text: "Live data paused — reconnecting..."
- Color: --color-warning background
- Position: top of Live Race screen
- Behavior: dismisses automatically when reconnected

## Screen Layout Principles

### Live Race (primary surface)
- Running order (P1-P20) is always the anchor
- AI event cards float above on major events, then collapse to feed
- Tyre/pit data is per-row tertiary — same visual weight as gap data
- NO card grids. Rows, not cards.

### Explore — Driver/Team Pages
- Lead with "This Weekend" context section (current session position, tyres, recent form)
- Career/historical stats are secondary, collapsed by default
- NO stat card grids with team-color border-left

### Friends
- Empty state: race-contextual CTA ("Australian GP is live — watch with friends")
- Message-first UI in rooms — not an avatar grid

## Anti-Patterns (never do these)

1. 3-column card grid with team-color accents
2. Border-left on cards as the only design element
3. Centered text on everything
4. Team colors as background fills
5. Inter/Roboto/Arial as body fonts
6. Color as the only differentiator (always add label/letter)
7. Generic hero copy ("Your all-in-one F1 companion")
8. Uniform border-radius on all elements

## Platform Notes

- **Primary:** Web app (React + Vite) + browser extension (Plasmo)
- **Extension sidebar:** ~380px right-side overlay on F1 TV
- **Mobile web:** 375px supported, not primary experience
- **Native mobile:** Post-MVP (React Native / Expo)

## Accessibility

- WCAG AA contrast on all text (minimum 4.5:1)
- Touch targets: 44px minimum
- Tyre compound labels: always show letter, not just color
- Screen reader events: throttle to major events only
- Keyboard navigation: required for extension sidebar
