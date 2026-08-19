# Profile design system

One card, terminal × modern minimal, still running. Everything else on the
page is plain markdown — the profile should read fast, not perform.

## Palette

| Token | Value | Use |
| :-- | :-- | :-- |
| Background | `#0d1117` | Card fill (matches GitHub dark) |
| Dot grid | `#1c2430` | Ambient texture |
| Edges | `#212b36` | Idle network connections |
| Border | `#21262d` / `#30363d` | Card outline, node rings |
| Wordmark | `#ffffff → #8b96a5` | Vertical metal gradient |
| Secondary text | `#7d8590` / `#adbac7` | Prompt, role line |
| Accent | `#58a6ff` | Prompt sigil, sweep, pulses, node cores |

## Type

`ui-monospace → SF Mono → Menlo → Consolas → monospace`, except the wordmark,
which uses the system sans stack at 800 weight.

Web fonts are deliberately unused: an SVG referenced through `<img>` gets no
network access on GitHub, so `@import` would silently fail and fall back
anyway.

## Motion

The card boots, then goes live. Two phases in one asset, so the loading
sequence costs no extra height.

**Phase 1 — boot (0 to 2.5s, plays once)**

`$ ./init-profile`, a progress bar filling left to right, a percentage
counter, and three status lines: `initializing` → `loading modules` →
`ready`.

The bar fills **linearly**, and each percentage label is the fill value at
the *midpoint* of the window it is on screen for. Easing the bar, or
labelling each window's start, leaves the number visibly trailing the bar.

There is no scripting in the SVG, so the counter is six stacked `<text>`
elements sharing one keyframe:

```css
@keyframes hold { 0%,100% { opacity: 1 } }
```

with `opacity: 0` as the base and **no fill mode** — each label is invisible
during its delay, opaque for its duration, and invisible again after. Stagger
the delays and it counts.

**Phase 2 — live (2.5s onward, loops forever)**

The boot group cross-fades out while the content group fades in. Everything
inside carries a `+2.5s` delay so nothing starts under the loading screen:

- a chrome sweep across the wordmark every 4.5s (a gradient rect clipped to
  the text itself, so it survives any font substitution)
- four role lines cycling on a 13s rotation, 3.25s apart
- a signal wave through the network — 36 edges, node cores firing as it
  lands, 3s cycle, 0.55s per layer

Constraints: no strobing, nothing faster than a 3s cycle, accent reserved for
one idea. Elements that must survive a renderer without CSS animation use
`animation-fill-mode: backwards`, so their base state is the finished state.

Travelling pulses set `pathLength="100"` on every edge, so one shared keyframe
drives 36 lines of different lengths.

## Layout

`860 × 190`, authored at the GitHub README content width and referenced with
`width="860"` so it scales down on mobile. Identity left, network right, 44px
margins on both sides.

Monospace advance widths vary by platform (≈0.55–0.62em). Text is laid out
against the 0.62em worst case so nothing overflows.

## Stat cards

Three third-party services, all themed to the palette above so they read as
one system with the header:

| Card | Service | Params that matter |
| :-- | :-- | :-- |
| Stats | `awesome-github-stats.azurewebsites.net` | `Background=0D1117&Border=21262D` — the `github-dark` theme alone renders `#1e2228`, which is visibly lighter than the rest |
| Streak | `github-readme-streak-stats.herokuapp.com` | full hex override: `ring`/`fire` = accent, `sideLabels` = secondary |
| Activity | `github-readme-activity-graph.vercel.app` | `bg_color`, `line`, `area_color`, `border_color` |

Two services that most profiles use are **dead** and were deliberately left
out rather than shipped as broken images:

- `github-readme-stats.vercel.app` — `DEPLOYMENT_PAUSED` (this also kills the
  usual Top Languages card; there is no working public instance)
- `github-profile-trophy.vercel.app` — `DEPLOYMENT_DISABLED`

If they come back, or you self-host `github-readme-stats` on your own Vercel,
they drop straight in.

## Content rules

- Counters come from the GitHub API. Nothing decorative, nothing invented.
- No dead links, and no embeds pointing at a service that is down.
- Honest status words: `Active`, `Experimental`, `Built`, `In progress`.
- Emphasis is earned: bold marks the tools actually reached for first.
