# The Harness

The harness is the instrument around the prototype: the boot check, the state toolbar, the
assumptions panel, and the interaction primitives. Its appearance is **not a design decision** — it
is this spec. Copy the markup, CSS, and wiring below verbatim. Only the state list, screen list, and
assumption rows change per run.

It stays identical across every project so it always reads as harness chrome, never as part of the
design being judged. Do not restyle it with the project's tokens, fonts, or colors. The assumptions
panel especially: the moment it adopts the product's design language, a reviewer reads it as a
designed feature instead of a warning.

Everything below is verified in Chromium under `file://` — token compilation, every state swap, URL
restore on reload, focus in and out of the modal, Esc ordering, the keys-ignored-in-fields rule, and
the offline banner with the CDN blocked.

## Head, tokens, and the boot check

The Tailwind browser build must load **first and un-deferred** — the compiler runs before paint.
`type="text/tailwindcss"` is what makes `@theme` work; a plain `<style>` is not compiled.

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Prototype — <feature> · not production</title>

<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>

<!-- The token mirror. Values come from recon, never from this file. -->
<style type="text/tailwindcss">
  @theme { /* … */ }
</style>

<!-- Harness chrome. Plain CSS on purpose: it must render even if the CDN is dead. -->
<style> /* … */ </style>
</head>
```

Last elements in `<body>`, before the scripts:

```html
<div id="ph-probe" class="hidden"></div>
<div id="ph-offline" hidden><strong>Styles failed to load.</strong> This prototype fetches Tailwind
from a CDN on first open. Connect to the network and reload — do not review the page in this
state.</div>
```

```js
/* If `.hidden` never resolves to display:none, Tailwind never compiled. Poll rather than
   check once on load, so the banner can't flash while the compiler is still working. */
(function bootCheck(tries) {
  if (getComputedStyle(document.getElementById('ph-probe')).display === 'none') return;
  if (tries > 0) return setTimeout(() => bootCheck(tries - 1), 250);
  document.getElementById('ph-offline').hidden = false;
})(8);
```

The banner is styled in the plain-CSS block, so it is legible in exactly the situation where
Tailwind isn't. Without it the failure mode is a reviewer judging an unstyled page and thinking
that's the proposal.

**Never use `localStorage`** — it sits on an opaque origin under `file://` and behaves
inconsistently. URL params only.

### Neutral default tokens

Use these only when recon finds no project tokens. Log "no project tokens found; using restrained
neutral defaults" as the first assumption.

```css
@theme {
  --color-bg-white-0: #ffffff;   --color-bg-weak-50: #f7f7f7;
  --color-bg-soft-200: #ebebeb;  --color-bg-sub-300: #d1d1d1;
  --color-bg-strong-950: #171717;
  --color-text-strong-950: #171717; --color-text-sub-600: #5c5c5c;
  --color-text-soft-400: #a3a3a3;   --color-text-white-0: #ffffff;
  --color-stroke-soft-200: #ebebeb; --color-stroke-sub-300: #d1d1d1;
  --color-primary-base: #335cff;    --color-primary-darker: #2547d0;
  --color-error-base: #fb3748;   --color-error-light: #ffc0c5; --color-error-lighter: #ffebec;
  --color-warning-base: #fa7319; --color-success-base: #1fc16b;
  --shadow-regular-xs: 0 1px 2px 0 #0a0d1408;
  --shadow-regular-sm: 0 2px 4px #1b1c1d0a;
  --shadow-regular-md: 0 16px 32px -12px #0e121b1a;
  --radius-10: 0.625rem;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}
```

**Never fetch a webfont.** Map to the closest system stack, name the real family as a fallback, and
log the substitution as an assumption.

### The offline fallback

Only on request — a reviewer with no network, or a locked-down machine. The fallback is **not**
hand-written utility classes; writing `.flex`, `.gap-3`, `.p-4` by hand is Tailwind with extra steps
and it will be incomplete. Instead: the same variables in a plain `:root` block, plus ~15 real
semantic classes (`.card`, `.row`, `.btn`, `.btn-primary`, `.field`, `.pill`, `.stack`, `.cluster`)
with real CSS inside, and inline `style` for one-off layout. Budget ~180 lines of CSS instead of ~85
of tokens, and expect a rougher spacing rhythm because every box gets tuned by hand.

## Chrome styles

Plain CSS, `ph-` prefixed, dark glass, never theme-aware. Dark glass works on top of any page, which
is why it is not theme-aware — same reasoning as the picker.

```css
/* Prototype harness chrome. Not a design decision. Copy verbatim. */
body { padding-bottom: 72px; }              /* the bar never covers content */
[hidden] { display: none !important; }      /* never depend on preflight for this */

.ph-bar, .ph-sheet, #ph-offline {
  position: fixed; z-index: 2147483647;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  font-size: 12px; line-height: 1; -webkit-font-smoothing: antialiased;
  user-select: none; -webkit-user-select: none;
}
.ph-bar {
  bottom: 16px; left: 50%; transform: translateX(-50%);
  display: flex; align-items: center; gap: 6px; padding: 6px;
  border-radius: 999px; background: rgba(10,10,10,.86);
  -webkit-backdrop-filter: blur(12px) saturate(1.4); backdrop-filter: blur(12px) saturate(1.4);
  box-shadow: 0 0 0 1px rgba(255,255,255,.08) inset, 0 8px 24px rgba(0,0,0,.24);
  max-width: calc(100vw - 32px); flex-wrap: wrap; justify-content: center;
}
.ph-tag { padding: 0 6px; height: 24px; display: flex; align-items: center; border-radius: 999px;
  background: rgba(255,255,255,.12); color: #fff; font-size: 10px; font-weight: 600;
  letter-spacing: .08em; }
.ph-btn { height: 24px; padding: 0 10px; border: 0; border-radius: 999px; background: transparent;
  color: rgba(255,255,255,.55); font: inherit; cursor: pointer;
  transition: color 120ms ease-out, background 120ms ease-out; }
.ph-btn:hover { color: rgba(255,255,255,.9); background: rgba(255,255,255,.06); }
.ph-btn[data-active] { color: #fff; background: rgba(255,255,255,.16); }
.ph-btn:focus-visible { outline: 2px solid rgba(255,255,255,.4); outline-offset: 2px; }
.ph-select { height: 24px; border: 0; border-radius: 6px; padding: 0 6px;
  background: rgba(255,255,255,.12); color: #fff; font: inherit; }
.ph-div { width: 1px; height: 16px; background: rgba(255,255,255,.14); }
.ph-badge { margin-left: 6px; padding: 0 5px; border-radius: 999px; background: #fb3748;
  color: #fff; font-size: 10px; font-weight: 600; }

/* Assumptions sheet — right edge, so it never covers the centre of the design. */
.ph-sheet { top: 0; right: 0; bottom: 0; width: 340px; overflow-y: auto; padding: 16px;
  background: rgba(10,10,10,.94);
  -webkit-backdrop-filter: blur(12px); backdrop-filter: blur(12px);
  box-shadow: 0 0 0 1px rgba(255,255,255,.08) inset;
  color: rgba(255,255,255,.82); line-height: 1.5; user-select: text; }
.ph-sheet-head { display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 8px; color: #fff; font-size: 13px; }
.ph-x { border: 0; background: transparent; color: rgba(255,255,255,.6); font-size: 18px;
  cursor: pointer; line-height: 1; }
.ph-note { margin: 0 0 12px; color: rgba(255,255,255,.5); font-size: 11px; }
.ph-list { margin: 0; padding: 0; list-style: none; display: grid; gap: 8px; }
.ph-item { padding: 8px 10px; border-radius: 8px; background: rgba(255,255,255,.05);
  font-size: 12px; cursor: default; }
.ph-item[data-target] { cursor: pointer; }
.ph-item:hover { background: rgba(255,255,255,.09); }
.ph-kind { display: inline-block; margin-right: 6px; padding: 1px 6px; border-radius: 4px;
  font-size: 10px; font-weight: 600; text-transform: uppercase; }
.ph-item[data-kind="needs"]     .ph-kind { background: #fb3748; color: #fff; }
.ph-item[data-kind="suggested"] .ph-kind { background: #fa7319; color: #171717; }
.ph-item[data-kind="inferred"]  .ph-kind { background: rgba(255,255,255,.16); color: #fff; }

/* Magenta, so it can never be mistaken for a product focus ring. */
.ph-hit { outline: 2px solid #ff00aa !important; outline-offset: 3px; border-radius: 6px; }

#ph-offline { top: 0; left: 0; right: 0; padding: 12px 16px; background: #fb3748; color: #fff;
  font-size: 13px; line-height: 1.4; text-align: center; }

@media (prefers-reduced-motion: reduce) { .ph-btn { transition: none; } }
```

## Toolbar markup

One bar carries everything: the screen jump menu, the state buttons, and the assumptions toggle. Do
not build a separate screen-switcher component — it shares `go()` with the prototype's own links.
Omit the `<select>` entirely for a single-screen prototype.

```html
<aside class="ph-bar" aria-label="Prototype harness — not part of the design">
  <span class="ph-tag">DEV</span>

  <select class="ph-select" id="ph-screen" aria-label="Jump to screen">
    <option value="list">1 · List</option>
    <option value="detail">2 · Detail</option>
  </select>

  <span class="ph-div" aria-hidden="true"></span>

  <div id="ph-states" role="group" aria-label="Force state">
    <button class="ph-btn" data-state-set="default" data-active>Default</button>
    <button class="ph-btn" data-state-set="loading">Loading</button>
    <button class="ph-btn" data-state-set="empty">Empty</button>
    <button class="ph-btn" data-state-set="no-results">No results</button>
    <button class="ph-btn" data-state-set="error">Error</button>
    <button class="ph-btn" data-state-set="denied">No permission</button>
    <button class="ph-btn" data-state-set="many">Many items</button>
  </div>

  <span class="ph-div" aria-hidden="true"></span>

  <button class="ph-btn" id="ph-sheet-toggle" aria-expanded="false" aria-controls="ph-sheet">
    Assumptions <span class="ph-badge" id="ph-needs" hidden>0</span>
  </button>
</aside>
```

## How state is applied

**One `data-state` attribute on `<html>`, plus a `[data-when]` visibility pass over pre-rendered
DOM.** No JS store, no re-render.

Every state's markup exists in the document from the start and is toggled with the `hidden`
attribute. This is not laziness. It buys three things: devtools shows every state at once, the file
reads top to bottom, and the prototype's own interactions can never be clobbered by a re-render.

```html
<section data-screen="list">
  <div data-when="!empty !no-results !error !denied">
    <div data-when="loading">…skeleton rows…</div>
    <ul id="rows" data-when="!loading">…real rows…</ul>
  </div>
  <div data-when="empty">…empty state…</div>
  <div data-when="no-results">…no-results state…</div>
  <div data-when="error">…error state + Retry…</div>
  <div data-when="denied">…permission denied…</div>
</section>
```

**The one authoring gotcha, and it fails silently:** every state that *replaces* the main content
must be named in that content's negation list. Miss one — `!empty !error` without `!no-results` —
and both render at once, which reads as a layout bug rather than a modeling mistake. When you add a
replacement state to the toolbar, add it to the negation list in the same edit.

For states that change **quantity** rather than presence, use CSS instead of markup. Zero JS:

```html
<style type="text/tailwindcss">
  @layer utilities {
    html:not([data-state="many"]) #rows > li:nth-child(n + 7) { display: none; }
    html[data-state="denied"] #bulk-actions { opacity: .4; pointer-events: none; }
  }
</style>
```

## Behavior contract

- Exactly one state button carries `data-active` at all times.
- Screen **and** state persist in the URL (`?s=detail&state=empty`) via `history.replaceState`. This
  is what makes a finding reproducible — *"the empty state at `?s=detail&state=empty` reads as an
  error"* is something a human can re-open — and it is what lets a debrief work on someone else's
  observations.
- Real in-prototype navigation (`<a data-goto="detail">` on an actual row) and the jump menu share
  one code path.
- **Three keys only:** `a` toggles the assumptions sheet, `0` resets to default, `Escape` closes the
  modal if one is open and otherwise the sheet. No digit→state map: the mapping is invisible, and
  digits are exactly what people type into a prototype's search and number fields.
- All keys are ignored when focus is in an input, textarea, select, or contenteditable, or when a
  modifier is held.
- The harness only ever writes `data-state` on `<html>` and `hidden` on `[data-when]`/`[data-screen]`.
  The prototype only ever writes inside screens. No shared mutable surface, so nothing can fight.
- **Transient interaction states are not harness states.** Clicking Save runs its own pending →
  success/error sequence locally via `withPending()`. If the global state attribute drove that too,
  the toolbar and the prototype would overwrite each other.

## Assumptions panel

The anti-laundering mechanism. A static `<ul>`, not a rendered array — fewer lines, can't flicker,
stays diffable.

```html
<section class="ph-sheet" id="ph-sheet" hidden aria-label="Assumptions in this prototype">
  <header class="ph-sheet-head">
    <strong>What the spec didn't decide</strong>
    <button class="ph-x" id="ph-sheet-close" aria-label="Close (A)">&times;</button>
  </header>
  <p class="ph-note">Everything below is this prototype's invention, not a decision. Hover an item
  to highlight what it affects.</p>
  <ul class="ph-list">
    <li class="ph-item" data-kind="needs" data-target="#save">
      <span class="ph-kind">Needs decision</span>
      Save stays enabled with unresolved warnings. The spec doesn't say whether warnings block
      submission — which decides whether this is a gate or a nudge.
    </li>
    <li class="ph-item" data-kind="suggested" data-target="#add-context">
      <span class="ph-kind">Suggested</span>
      Adding context opens a modal rather than editing inline.
    </li>
    <li class="ph-item" data-kind="inferred">
      <span class="ph-kind">Inferred</span>
      Light theme only; dark mode is out of scope for this prototype.
    </li>
  </ul>
</section>
```

Rules:

- **The badge count is computed, never typed.** An agent that adds an assumption and forgets to bump
  a hard-coded number produces exactly the laundering this panel exists to prevent.
- **It opens on load whenever the needs-decision count is above zero.** The reviewer must not be
  able to miss them.
- Severity is a text label plus colour, never colour alone.
- **No persistent markers on the design.** Pins or dots become part of what's being judged. The
  magenta outline appears on hover only.
- Every `needs` row states what the spec omitted **and** why it matters, and points at a real
  element. "Needs decision: colour of the button" is noise — if an item has no consequence, it's an
  `inferred` row or it's deleted.
- **Completion criterion:** if you wrote a threshold, a sort order, a truncation limit, a debounce,
  a permission rule, or a copy string with a number in it that the spec didn't state — it has a row.

## Reference wiring

Verbatim. `withPending`, `toast`, and the modal render as **product** UI and use product tokens — a
toast is something the reviewer is meant to judge. The chrome above never does.

```js
/* ── Harness. ── */
const root = document.documentElement;
const sheet = document.getElementById('ph-sheet');
const sheetToggle = document.getElementById('ph-sheet-toggle');

function setState(state) {
  root.dataset.state = state;
  document.querySelectorAll('[data-when]').forEach(el => {
    const toks = el.dataset.when.split(/\s+/);
    el.hidden = toks[0][0] === '!'
      ? toks.some(t => t.slice(1) === state)   // hide in these states
      : !toks.includes(state);                 // show only in these states
  });
  document.querySelectorAll('#ph-states .ph-btn').forEach(b =>
    b.toggleAttribute('data-active', b.dataset.stateSet === state));
  syncUrl();
}

function go(screen) {
  document.querySelectorAll('[data-screen]').forEach(s => s.hidden = s.dataset.screen !== screen);
  document.getElementById('ph-screen').value = screen;
  syncUrl();
  window.scrollTo(0, 0);
}

function syncUrl() {
  const url = new URL(location);
  url.searchParams.set('s', document.getElementById('ph-screen').value);
  url.searchParams.set('state', root.dataset.state || 'default');
  history.replaceState(null, '', url);
}

function toggleSheet(force) {
  const open = force ?? sheet.hidden;
  sheet.hidden = !open;
  sheetToggle.setAttribute('aria-expanded', String(open));
}

document.getElementById('ph-states').addEventListener('click', e => {
  const b = e.target.closest('[data-state-set]'); if (b) setState(b.dataset.stateSet);
});
document.getElementById('ph-screen').addEventListener('change', e => go(e.target.value));
sheetToggle.addEventListener('click', () => toggleSheet());
document.getElementById('ph-sheet-close').addEventListener('click', () => toggleSheet(false));

document.addEventListener('click', e => {
  const t = e.target.closest('[data-goto]');
  if (t) { e.preventDefault(); go(t.dataset.goto); }
});

/* Hover an assumption → outline what it affects. */
sheet.addEventListener('mouseover', e => {
  const item = e.target.closest('.ph-item[data-target]');
  document.querySelectorAll('.ph-hit').forEach(el => el.classList.remove('ph-hit'));
  const t = item && document.querySelector(item.dataset.target);
  if (t) { t.classList.add('ph-hit'); t.scrollIntoView({ block: 'nearest' }); }
});
sheet.addEventListener('mouseleave', () =>
  document.querySelectorAll('.ph-hit').forEach(el => el.classList.remove('ph-hit')));

/* Derived, so it cannot drift from the list. */
const needs = sheet.querySelectorAll('[data-kind="needs"]').length;
const badge = document.getElementById('ph-needs');
badge.textContent = needs; badge.hidden = needs === 0;

document.addEventListener('keydown', e => {
  if (/^(INPUT|TEXTAREA|SELECT)$/.test(e.target.tagName) || e.target.isContentEditable) return;
  if (e.metaKey || e.ctrlKey || e.altKey) return;
  if (e.key === 'a') toggleSheet();
  else if (e.key === '0') setState('default');
});

/* ── Interaction primitives. Product UI — these use product tokens. ── */
let lastFocus = null;
function openModal(id) {
  const m = document.getElementById(id);
  lastFocus = document.activeElement;
  m.hidden = false;
  /* [autofocus] queried separately: querySelector returns the first match in DOCUMENT
     order, not selector order, so a list would let an earlier field win. */
  (m.querySelector('[autofocus]') ||
   m.querySelector('button, input, textarea, select, [tabindex]') || m).focus();
}
function closeModal(id) {
  document.getElementById(id).hidden = true;
  lastFocus?.focus();
}
document.addEventListener('keydown', e => {
  if (e.key !== 'Escape') return;
  const open = document.querySelector('[data-modal]:not([hidden])');
  if (open) closeModal(open.id);
  else if (!sheet.hidden) toggleSheet(false);
});
document.addEventListener('click', e => {
  if (e.target.matches('[data-modal-backdrop]')) closeModal(e.target.closest('[data-modal]').id);
  const c = e.target.closest('[data-modal-close]');
  if (c) closeModal(c.closest('[data-modal]').id);
  const o = e.target.closest('[data-modal-open]');
  if (o) openModal(o.dataset.modalOpen);
});

/* Simulated async: real latency, reachable failure. Never instant — an instant success
   means the loading state is never seen and never gets designed. */
const sleep = ms => new Promise(r => setTimeout(r, ms));
async function withPending(btn, fn, { fail = false } = {}) {
  btn.disabled = true;
  const label = btn.textContent;
  btn.textContent = 'Saving…';
  await sleep(500 + Math.random() * 400);
  btn.disabled = false; btn.textContent = label;
  if (fail) return toast("Couldn't save. Try again.", 'error');
  fn?.(); toast('Saved');
}

function toast(msg, kind = 'success') {
  const t = document.createElement('div');
  t.className = 'fixed left-1/2 top-4 z-50 -translate-x-1/2 rounded-10 px-4 py-2.5 ' +
    'text-label-sm shadow-regular-md ring-1 ring-inset ' +
    (kind === 'error'
      ? 'bg-error-lighter text-error-base ring-error-light'
      : 'bg-bg-strong-950 text-text-white-0 ring-transparent');
  t.textContent = msg;
  document.body.appendChild(t);
  setTimeout(() => t.remove(), 2600);
}

/* Boot: restore screen and state from the URL. */
const params = new URLSearchParams(location.search);
go(params.get('s') || document.querySelector('[data-screen]').dataset.screen);
setState(params.get('state') || 'default');
if (needs) toggleSheet(true);
```

`toast()` builds its classes by concatenating string literals, so they appear in the inline
`<script>` source and are compiled on the compiler's first pass — verified, no flash. A class name
computed from a variable at runtime would not be, and needs a safelist:
`<div hidden class="…the classes…"></div>` in the initial markup.

## File skeleton and line budget

**Target ~800 lines. Ceiling 1200.** Past that the reviewer won't finish clicking it.

| # | Part | Lines | Verbatim? |
|---|---|---|---|
| 1 | `<head>`: meta, title, CDN script | 8 | yes |
| 2 | `@theme` token mirror | 85 | recon-derived, then fixed |
| 3 | Harness chrome CSS | 75 | **verbatim** |
| 4 | `MOCK` object | 50–80 | per spec |
| 5 | Harness markup: toolbar, sheet, probe, banner | 45 | structure verbatim, rows per spec |
| 6 | Screens — every state pre-rendered | 350–600 | per spec — **this is the deliverable** |
| 7 | Interaction JS: search, filter, tabs, expand | 100–180 | per spec |
| 8 | Interaction primitives | 40 | **verbatim** |
| 9 | Harness JS | 60 | **verbatim** |
| 10 | Inline `<svg><symbol>` sprite, 6–8 icons | 15 | per spec |

**The ratio rule:** screens plus interaction JS must be **at least 3×** the harness. If the harness
is more than a third of the file, either the prototype is too thin to review or the harness grew a
feature it shouldn't have. Cut the harness, not the prototype.

**Write order — three calls, not fifteen.** Parts 1, 3, 5, 8, 9 are ~230 lines of verbatim
boilerplate: one call. Parts 2 and 4: one call. Parts 6, 7, 10: one or two. Keeping the boilerplate
byte-identical across runs is what makes this cheap — an agent that "improves" the chrome each run
has misunderstood what it is.

## Mock data

One `const MOCK = {…}` at the top of the first `<script>`. Nothing else in the file holds data. The
reviewer can then read the whole fixture in one screen and say "that's not what our data looks
like" — one of the most valuable things a prototype can provoke, and impossible if the strings are
scattered across 400 lines of markup.

`MOCK` is the source of truth for anything the interaction JS touches — the list being
searched/filtered/sorted, counts, the current user, permissions — and rows for that list are
generated from it once at load. Static one-off screens keep their content inline; don't build a
rendering layer for a detail screen that renders once.

Bad mock data creates fake confidence. These rules are what make it expose design problems instead:

1. **6–8 rows in the default view.** Enough to see rhythm, few enough to read.
2. **Realistic domain content** — real-looking entity names, plausible numbers, correct formatting
   and separators for the locale. Never `Acme Corp`, `John Doe`, or lorem ipsum.
3. **Dates relative to one hardcoded `TODAY`**, including one today, one past, one far future — so
   relative-time labels are visible at all and don't rot.
4. **One deliberately brutal long value, always visible** — longest realistic title and longest
   name. Not behind a state.
5. **One row with empty optionals**, to test every `—` / placeholder / "Unassigned" decision. This
   is where specs are silent most often.
6. **One warning or edge row** the design must react to: past deadline, over budget, flagged.
7. **Mixed name lengths**, which is what breaks fixed-width avatar-plus-name layouts.
8. **The current user is the least-privileged plausible role**, so permission gaps surface without
   touching the toolbar.
9. **No fields invented past what the spec implies.** An extra field is an assumption; if you add
   one, it gets a row in the panel.

The states that change *quantity* extend this by cloning with varied fields — never by repeating one
row.
