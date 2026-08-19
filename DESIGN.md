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

It loops, because a one-shot animation means most visitors arrive to a still
image.

- A chrome sweep crosses the wordmark every 4.5s (a gradient rect clipped to
  the text itself, so it survives any font substitution).
- Four role lines cycle on a 13s rotation, 3.25s apart.
- A signal wave propagates left → right through the network — 36 edges, node
  cores firing as it lands. 3s cycle, 0.55s per layer.

Constraints: no strobing, nothing faster than a 3s cycle, accent reserved for
one idea. Entrance animations use `animation-fill-mode: backwards`, so the
base state is the finished state — if CSS animation never runs, the card
still renders complete rather than blank.

Travelling pulses set `pathLength="100"` on every edge, so one shared keyframe
drives 36 lines of different lengths.

## Layout

`860 × 190`, authored at the GitHub README content width and referenced with
`width="860"` so it scales down on mobile. Identity left, network right, 44px
margins on both sides.

Monospace advance widths vary by platform (≈0.55–0.62em). Text is laid out
against the 0.62em worst case so nothing overflows.

## Content rules

- No fabricated metrics — no streak counters, star totals or trophies.
- No dead links. A project with no public repo gets no link.
- Honest status words: `Active`, `Experimental`, `Built`, `In progress`.
- Emphasis is earned: bold marks the tools actually reached for first.
