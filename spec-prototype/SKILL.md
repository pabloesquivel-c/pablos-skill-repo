---
name: spec-prototype
description: >-
  Turns an approved spec into one medium-fidelity interactive HTML prototype
  that proves the mechanics, states, and interaction model before production.
  Inherits spec-audit's tags: anything rendered from a suggested or undecided spec line
  shows up in a visible assumptions panel inside the prototype, so a guess
  never reads as a decision. Ships a state toolbar that force-sets every state
  in one click, because a prototype that only demos the happy path proves
  nothing. Convergence, not divergence — for several different answers to one
  UI question, use the prototype skill instead. Use when a spec is
  design-ready and the open question is whether the thing actually works.
  Triggers on: prototype this spec, build a prototype, interactive prototype,
  clickable prototype, html prototype, prove the mechanics, does this flow
  work, test the interaction model, another round of the prototype, state
  matrix, mock data, medium fidelity.
argument-hint: "[approved spec: link, paste, or file]"
---

# spec prototype

your job is to find out whether a spec's mechanics actually work, by building the smallest thing
someone can click.

## core principle

a prototype is a spec that has learned to be clicked. every pixel of it makes a claim. so
spec-audit's rule — *you can propose, but you must not silently decide* — becomes this, one layer
down:

**you can render an assumption, but you must never render it as a decision.** a claim that came
from a `[suggested]` or `[needs decision]` line is marked as one inside the prototype, where the
person judging it is looking — not in a handoff message they won't read.

and the other half: **prototype to answer questions, not to impress people.**

## when to use

- a spec came out of `/spec-audit` design-ready, and the open question is whether it actually works
- a flow has enough states, branches, or agent steps that nobody can tell from reading it
- a mechanic needs testing on a real person before design or dev commits
- a spec changed and the existing prototype needs to catch up

**not this skill:** exploring several genuinely different answers to one UI question. that's
`prototype` — a divergence skill, N variants behind a visual picker. this one is convergence: one
approved spec, one prototype, proving the mechanics the spec already decided. if you don't yet know
what the thing should *be*, run `prototype` first and come back with the winner.

also not for: production code, design-system mapping, visual design, or writing and updating the
spec — that stays with whoever owns it.

## required inputs

1. **the spec** — linear issue, notion page, doc link, pasted text, or a file. fetch it **in full**
   with whatever integration is available. never prototype a paraphrase or your memory of an
   earlier conversation.
2. **whether it's tagged.** if it came through `/spec-audit`, the tags are your inheritance map —
   read them before anything else. **if it isn't tagged, don't refuse.** say so, classify inline at
   lower confidence using spec-audit's three-way test (safe to infer / best-practice default /
   product-specific), and note that running `/spec-audit` first would make the assumptions panel
   trustworthy instead of best-effort.
3. optional, use if offered: existing screens to match, figma references, the parent feature spec,
   design tokens, prior mobbin research.

if nothing was given, ask for the spec. don't start.

## the workflow

```
spec (tagged)
→ recon: stack, tokens, component vocabulary
→ capped interview
→ prototype plan  ── stop 1, approve before any build
→ skeleton + mock data + core interactions  ── stop 2, one scoped question
→ states, edges, assumptions panel
→ hand over, then stop
```

what happens to the prototype afterwards — who reviews it, what they say, how the spec changes — is
not this skill's job. hand over cleanly and get out of the way.

if the spec comes back changed and a prototype for it already exists, **extend that file in place**
and say which parts you're rebuilding. never fork a second file for the same spec.

### 1. recon

read the ground before designing anything. 6–8 read-only calls, in order.

1. **styling system** — grep the package manifest for tailwind / styled-components / emotion /
   vanilla-extract / sass. tailwind v4 means css-first `@theme` and the browser build matches
   exactly; v3 means a js config you translate (`theme.extend.colors.brand.500` →
   `--color-brand-500`).
2. **token source**, in preference order: an `@theme` block → `tailwind.config.*` `theme.extend` →
   a design-system package's tokens → `:root` custom properties → a css-in-js theme. none found →
   the neutral defaults in [HARNESS.md](HARNESS.md), logged as your first assumption.
3. **prefer the semantic layer and flatten the indirection.** with raw ramps *and* semantic
   aliases, copy the semantic **names** with the final **hex** — names are what appear in real
   components, hex is what the browser needs. ~85 lines, not 900: skip raw ramps, alias layers,
   dark-mode overrides, decorative shadow families, unused type-scale entries.
4. **component vocabulary — do not skip this.** read three real components: the button, the densest
   repeated element, and one overlay. lift the **literal class strings** for control height, radius,
   horizontal padding, border-vs-ring, shadow, and transition. this, not the colours, is what makes
   a prototype read as native — right colours with the wrong control height feels like a different
   product.
5. **font** — map to the closest system stack, never fetch a webfont, log the substitution.
6. **note and skip** dark mode, rtl, and i18n as assumptions.
7. **verify before writing** — grep each token name in real components. if it isn't there, you
   invented it. also check whether `*.html` is gitignored, so the file doesn't get committed by
   accident.

### 2. capped interview — max 3, plus the catch-all

ask via `AskUserQuestion`, skipping anything the spec or context already answers. frame every
question as a **real-world event a person would recognise**, never in codebase terms.

1. what has to be true for this to count as working — the one thing you'd be embarrassed to get
   wrong.
2. who is going to click it, and whether they know the product already.
3. the highest-uncertainty part: which screen or moment is the one nobody can picture yet.
4. always ask, even when the first three are fully answered: "anything specific you want included,
   avoided, or paid attention to?"

state the defaults as assumptions rather than spending question slots on them — screen count, task
count, where the file lands. and on anything the user may not own, offer **"flag it for the team"**
as an explicit option instead of forcing a call; it stays a `[needs decision]` row in the panel.

### 3. the plan — stop 1, approve before any build

```markdown
# prototype plan

## intent
- mechanics being validated:
- assumptions from the spec that need proof:
- **not to be evaluated yet:**

## screens (max 3)
| screen | why it's in | spec line |
|---|---|---|

## interactions to prove
-

## state matrix
| surface | state | why it's required | spec line |
|---|---|---|---|

## mock data
- shape, and which ugly cases are built in by construction

## inherited assumptions
| what the prototype has to invent | tag | source line |
|---|---|---|

## task script (max 4, phrased as jobs)
1.

## build order
-
```

**derive the state matrix; never work from a fixed checklist.** take the spec's own key states
verbatim, then add loading and error for every async surface, empty / too-many / long-content for
every list, no-results for every search, and denied for every permissioned action. **cap at ~10**,
rank them, and name each against the spec line that requires it — an unranked list of fifteen gets
built whether or not the spec needs them. importance follows the mechanic: when the core mechanic
*is* search, no-results is the most important state in the matrix, not an afterthought.

**the task script is jobs, not clicks.** "find every technical requirement that needs attention" is
a task; "click the second tab" is a demo. and `done when` is not a separate section — for a
prototype the task script *is* the acceptance criteria.

### 4. build

read [HARNESS.md](HARNESS.md) and copy the chrome verbatim. build in this order, without stopping
between the first two:

1. **skeleton + mock data** — page shell, layout regions, real content in real rows.
2. **core interactions** — tabs, search, filters, expand/collapse, the primary action.
3. **states, edges, and the assumptions panel** — every row in the matrix, reachable from the
   toolbar.

**stop 2 sits between 2 and 3, and it is deliberately narrow.** ask one question — *does the
interaction model read right?* — and nothing else. it earns a stop because states and edges get
built **on** the screens, so a misread model means rebuilding everything above it. it stays narrow
because at that moment the prototype is a happy-path demo, which is exactly the artifact that
invites spacing feedback before mechanics are proven. no screenshots, no fidelity claims, no "what
do you think". if the answer is "keep going", it cost one turn.

**when a spec line can't be built as written — a contradiction, a state with no data behind it, two
rules claiming the same slot — stop and log it. don't resolve it in code.** an unbuildable spec line
is the most valuable thing this skill finds, and smoothing it over in javascript is how it gets
lost. this is the mechanism by which prototyping *audits* a spec instead of illustrating it.

### 5. hand over, then stop

the gate is mechanical, not aesthetic: **every state in the matrix reachable in one click from the
toolbar.** never "it looks right". if you're adjusting spacing while a state is still unreachable,
you're in the wrong step.

then open the file and give exactly four things:

1. the path, and the keys (`a` assumptions, `0` reset).
2. the state list on the toolbar, demoed **first** — the happy path is the least informative thing
   in the build.
3. the task script.
4. **the assumptions panel, read aloud.** this is the moment tag inheritance is most likely to get
   skipped, so it doesn't get skipped here.

then add anything the *build itself* revealed, which is the one thing you know and the reviewer
doesn't: **spec lines that couldn't be built as written**, and anything the state matrix forced
into the open. keep it to a short list. it's raw material for whoever updates the spec, not a
recommendation about what to do — and it is not a substitute for what people find by clicking.

then stop. building and reviewing are separate asks.

## guardrails

- **never invent a field name, enum value, status, or data source.** if a screen needs data the
  spec doesn't describe, that's a product-specific gap: it renders as a `[needs decision]` row, and
  the panel says so.
- **an empty assumptions panel means one of two things: the spec was fully confirmed, or you
  laundered something.** it is almost always the second.
- **mirror the project's tokens, never its components.** a hand-rolled thing that looks like the
  design system's button is worse than an obviously plain one, because it makes the design-system
  mapping question look answered before it has been asked.
- **the fidelity ceiling, as prohibitions:** no custom easing, no illustration, one icon weight, no
  hover-state tuning, no dark mode, no charts unless the spec is about a chart. state changes are
  instant. motion only where the motion **is** the mechanic — a drag, a reorder, a sheet.
- **no polish step.** either the spacing and hierarchy were right while you built, or the thing
  can't be judged for usability at all. a polish pass at the end only guarantees the last
  impression is aesthetic, which is the failure this skill exists to prevent.
- **the prototype's own header states what it is and isn't** — *"proves mechanics and states.
  layout, copy, and components are not decided here."* the person who screenshots it into slack
  will not read your handoff message.
- **nothing in production ever imports from it**, and the hand-over names what has to be rebuilt
  properly rather than ported.
- **cap at the screens the spec's core flow names, max 3.** more than 3 → prototype the single
  highest-uncertainty screen, say which and why, and offer the rest as follow-up runs. no routing,
  no persistence, no auth, no state that survives a reload.
- **every control works, is visibly disabled, or says so.** a dead button costs you the reviewer's
  trust in the whole file, and they stop probing.
- **no instant success.** an async action that resolves immediately means the loading state is
  never seen and never gets designed.
- **the ugly cases are in the default data, not behind a state.** a long value nobody opts into is
  a long value nobody looks at.
- **narrow viewports are out of scope** unless the spec's core flow happens on a phone.
- **one self-contained html file, in a scratch location.** an in-app dev route is opt-in only, for
  when mechanics are already proven and the question has become design-system mapping — a route
  compiles into the app, participates in its build, and can break it.
- **keep the code easy to throw away.** no framework, no build step, no dependency beyond the one
  cdn script.

## self-check before you hand over

1. every state in the matrix reachable in one click, and every replacement state named in the
   negation list of the content it replaces?
2. every product-specific guess in the panel, with what the spec omitted and why it matters?
3. no invented field names, enums, or statuses?
4. one long value, one row of empty optionals, and one edge row visible in the default data?
5. every control working, disabled, or saying so? console clean?
6. screens + interaction code at least 3× the harness?
7. did you check it in a browser, or are you reporting that it works?
