---
name: visual-explainer
description: Builds a single self-contained, visual HTML page that teaches a topic in depth to a design-lead audience — React + Tailwind stack, comfortable in devtools, not yet writing much code from scratch. Use when the user asks to "explain X visually", "teach me X", "make me an explainer for X", or invokes /visual-explainer [topic].
argument-hint: "[topic]"
---

# Visual Explainer

Produce one offline-capable HTML file that teaches `$ARGUMENTS` (the topic) from
zero, visually, to a specific reader: a design lead who ships pixel-polish
personally, reads browser devtools fluently, but doesn't yet write much code
from scratch. Their stack is React + Tailwind. Their two goals are (1) get
good enough to polish UI themselves and (2) communicate precisely with
engineers about it.

If no topic was given, ask for one before doing anything else.

## Before writing any HTML

1. **Research the real mechanics**, not the surface-level definition. If the
   topic touches browser/CSS/React behavior you're unsure about (spec edge
   cases, actual algorithm, current best practice), look it up rather than
   relying on a half-remembered summary — a wrong mental model is worse than
   no page at all.
2. **Think like a senior design engineer** at a place like Linear, Stripe,
   Vercel, Ramp, xAI, Anthropic, or Notion would about this topic day-to-day.
   What do they actually care about? Where's the naive approach people reach
   for first, and why do senior folks avoid it? Bake that judgment into the
   page's opinions — don't just list neutral facts.
3. **Fold in the reader's context** explicitly: frame examples in Tailwind
   classes and React components (not raw unfamiliar frameworks), assume they
   can open devtools and inspect an element but would not know how to derive
   the underlying CSS/JS from scratch, and keep the throughline pointed at
   "ship pixel-polish yourself" + "talk to engineers precisely."

## Output contract

A single `.html` file, saved to `./explainers/<topic-slug>.html` in the
current working directory (create the folder if needed), that:

- **Is visual-first.** Every concept gets something to *see*: a live-rendered
  example, a diagram, a side-by-side of "design intent vs. code," or an
  interactive toggle/slider/input the reader can manipulate. Never explain in
  prose what could instead be demonstrated.
- **Is progressive.** Start from zero, build up in order, define every piece
  of jargon inline the first time it's used (a hover tooltip or inline
  parenthetical, not a glossary they have to scroll to).
- **Pairs real, copyable code with its rendered result.** Show actual CSS,
  Tailwind classes, and/or React snippets directly next to (or toggled with)
  the live example they produce, so cause and effect are adjacent, not
  described.
- **Ends with two sections**: "How this shows up in your day-to-day" (concrete
  scenarios tied to their design-lead role) and a short, practical checklist
  they can run through while reviewing or shipping UI.
- **Is fully self-contained and offline-capable.** No build step, no external
  CDN or network dependency (no CDN Tailwind, no remote fonts/icons/scripts) —
  everything inline in one file: `<style>`, `<script>`, and any SVG/diagrams.
  Because Tailwind itself can't run without its CDN/build step, render
  Tailwind examples as plain inline CSS that matches what the class would
  compute to, and label each with the Tailwind class name as an annotation
  (e.g. a small code chip reading `gap-4` next to the CSS it maps to) so the
  reader learns the real vocabulary without the page depending on a network
  fetch.

## Design of the page itself

Treat the explainer's own visual design as part of the deliverable — it's
teaching a design lead, so it should model good craft, not just describe it:

- Clean type hierarchy, generous whitespace, a restrained color system (pick
  2-3 accent colors max, reuse consistently for semantic meaning e.g. one
  color = "the thing changing," another = "the constraint causing it").
  If a chart or color palette is involved, check the `dataviz` skill's
  conventions.
- Consistent spacing scale, not arbitrary pixel values.
  Interactive elements (sliders, toggles) get obvious affordances and
  immediate visual feedback — no submit button, no delay.
- Dark-mode-friendly if trivial to do inline; skip if it would add real
  complexity.
- Section headers that let the reader skim the whole arc in 10 seconds before
  committing to read linearly.

## After writing the file

Open it for the user (`open ./explainers/<topic-slug>.html` on macOS) so they
land straight in the rendered page rather than reading raw HTML.
