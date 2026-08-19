# Profile design system

Terminal × modern minimal, but alive. The card should look like an
instrument that's still running, not a screenshot.

## Palette

| Token | Value | Use |
| :-- | :-- | :-- |
| Background | `#0d1117` | Card fill (matches GitHub dark) |
| Surface | `#161b22` | Chips, pipeline nodes |
| Dot grid | `#1c2430` | Ambient texture |
| Border | `#21262d` / `#30363d` | Hairlines, node outlines |
| Primary text | `#e6edf3` | Names, stage labels |
| Secondary text | `#7d8590` | Roles, output, captions |
| Accent | `#58a6ff` | Prompts, pulses, packets, haloes |
| Accent 2 | `#a371f7` | One background glow only |

## Type

`ui-monospace → SF Mono → Menlo → Consolas → monospace` for everything
except the wordmark, which uses the system sans stack at 800 weight.

Web fonts are deliberately unused: an SVG referenced through `<img>` gets no
network access on GitHub, so `@import` would silently fail and fall back
anyway.

## Motion

Everything loops, because a one-shot animation means most visitors arrive to
a still image.

| Asset | What moves |
| :-- | :-- |
| `header.svg` | Chrome sweep across the wordmark, four cycling role lines, a scan line, two drifting glows |
| `neural.svg` | A signal wave propagating input → hidden → hidden → output, nodes firing and ringing as it lands |
| `pipeline.svg` | Each stage lights in sequence while a packet travels the flow line |

Rules that keep it from being obnoxious: no strobing, nothing faster than a
3s cycle, accent used for *one* idea per asset, and entrance animations use
`animation-fill-mode: backwards` so the base state is the finished state — if
CSS animation never runs, the card still renders complete rather than blank.

Travelling pulses use `pathLength="100"` on every edge, so one shared
keyframe drives 78 lines of different lengths.

## Layout

Authored at **860px** — the GitHub README content column — and referenced
with `width="860"` so they scale down on mobile.

Monospace advance widths vary by platform (≈0.55–0.62em). Text is laid out
against the 0.62em worst case so nothing overflows its box on any renderer.

## Content rules

- No fabricated metrics — no streak counters, star totals or trophies.
- No dead links. A project with no public repo gets no link.
- Honest status words: `Active`, `Experimental`, `Built`, `In progress`.
- Emphasis is earned: bold marks the tools actually reached for first.
