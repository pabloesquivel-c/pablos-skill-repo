# examples

calibration for [SKILL.md](SKILL.md). read the good-vs-bad pairs when you're unsure
about severity or classification. read the worked audit when you're unsure about
length and tone, and the worked compress when you're unsure how much to cut.

---

## 1. worked audit

### input spec (as received)

```markdown
requirements section

users need to see their application requirements. show them in a list, grouped
somehow. they should be able to manage requirements and start the review when
they're ready. some requirements have issues and users should be able to deal with
those.
```

### output

```markdown
# spec audit result

## summary
- design-readiness: blocked
- main risk: "manage" and "deal with" could each mean 4 different things, so any screen drawn now is a guess
- recommended next step: answer the top 3 questions, then re-run this audit — 10 minutes of decisions saves a redesign
- confidence: medium — the flow shape is recognizable, but there's no domain context on what a requirement or a review actually is

## gaps found

| gap | severity | type | why it matters | suggested fix | needs human? |
|---|---|---|---|---|---|
| "manage requirements" undefined | p0 | product-specific | view, edit, approve, delete and add-context each produce a different screen | pick the actions in v1 | yes |
| no user goal | p0 | product-specific | nothing to optimize the screen for, so hierarchy is arbitrary | one sentence: as an applicant i want to… | yes |
| what "issues" means is undefined | p0 | product-specific | drives status design, filtering and the primary action | define the warning condition | yes |
| grouping rule is "somehow" | p0 | product-specific | grouping is the whole information architecture of the screen | group by requirement type, subcategory nested | yes |
| nothing said about who can act | p0 | product-specific | changes what's visible vs disabled for most users | define roles per action | yes |
| no out of scope | p1 | best-practice default | every adjacent idea will get added mid-design | list what v1 excludes | yes |
| no empty state | p1 | safe to infer | a new application has zero requirements | empty state with what to do next | no |
| no loading state | p1 | safe to infer | requirements load async | skeleton list while loading | no |
| no error state on load | p1 | safe to infer | the fetch can fail | error with retry | no |
| "start review" preconditions unclear | p1 | product-specific | decides whether the primary button is enabled | define when it's blocked | yes |
| requirement fields not listed | p1 | product-specific | design will look fine on perfect content and break on real data | list the fields and their limits | yes |
| no acceptance criteria | p1 | best-practice default | nobody can agree when this is done | 5–8 observable checks | no |

## safe inferred fixes
things i can add based on standard product behavior:
- empty state when the application has no requirements
- skeleton list while requirements load
- error state with retry if the load fails
- if search is added, a no-results state
- long descriptions clamp rather than push the layout

## suggested best-practice defaults
things i recommend based on common software patterns:
- group by requirement type, with subcategory nested one level
- filter toggle for "needs attention" rather than a separate tab
- warning requirements show the reason inline under the row, not in a tooltip
- one primary action on the screen: start review
- "+n more in this category" once a group passes 3 items

## questions for product/team
things that need human decision:

1. **what can a user do to a requirement in v1?**
   why it matters: view-only, add-context, and edit are three different screens.
   options: (a) view + add context — *recommended, smallest thing that unblocks review*, (b) view + edit, (c) full crud.
   owner: product. blocks: everything below it.

2. **what makes a requirement "need attention"?**
   why it matters: it drives the status system, the filter, and the primary action.
   options: (a) a backend-flagged warning with a reason string — *recommended*, (b) any incomplete field, (c) both, shown differently.
   owner: product + backend. blocks: status design, filtering.

3. **who can add context to a warning requirement?**
   why it matters: changes whether most users see an action or a disabled state.
   options: (a) anyone on the application — *recommended for v1*, (b) admins only, (c) the assigned owner.
   owner: product. blocks: permissions, empty and denied states.

4. **can review start while warnings exist?**
   why it matters: this is the enable/disable rule for the only primary button.
   options: (a) yes, with a warning summary — *recommended, keeps the user in control*, (b) no, hard block, (c) yes, but requires context on every warning first.
   owner: product. blocks: the primary action and its states.

5. **what requirement data actually exists in the backend today?**
   why it matters: i can't design rows around fields that don't exist, and i won't guess field names.
   options: needs an answer, not options.
   owner: engineering. blocks: data needed, row layout, truncation rules.

**top 3 to answer first:**
1. what can a user do to a requirement in v1?
2. what makes a requirement need attention?
3. can review start while warnings exist?

## improved spec draft

**why**
[needs decision] the user and business problem this solves — the current text doesn't say what goes wrong today
[inferred] users can't tell which requirements are ready and which need work before submitting

**user goal**
[needs decision] as an applicant, i want to \[?], so i can \[?] — needs one sentence from product

**in scope**
[confirmed] show requirements in a list
[needs decision] which actions on a requirement are in v1
[suggested] group requirements by type, subcategory nested
[suggested] filter to "needs attention"
[confirmed] start the review
[needs decision] whether warning requirements can be acted on, and how

**out of scope**
[needs decision] nothing was excluded — needs a list, or every adjacent idea lands in v1

**core flow**
[confirmed] 1. user opens the requirements section
[suggested] 2. user scans requirements grouped by type
[suggested] 3. user filters to needs-attention if there are warnings
[needs decision] 4. user resolves warnings — how, depends on question 1
[confirmed] 5. user starts the review

**behavior rules**
[suggested] grouping is by requirement type, subcategory nested one level
[suggested] the needs-attention filter shows only warning requirements
[suggested] warning requirements show their reason inline below the row
[inferred] long descriptions clamp instead of expanding the row
[needs decision] whether start review is disabled while warnings exist
[needs decision] who can act on a warning requirement

**data needed**
[needs decision] the actual field list — i won't invent field names. engineering to confirm what exists
[confirmed] a requirement has a name, since the spec says they're listed
[needs decision] whether a warning carries a reason string

**key states**
[inferred] default list with requirements
[inferred] empty — no requirements on the application
[inferred] loading
[inferred] error on load, with retry
[suggested] warning state on a requirement
[needs decision] any success state after resolving a warning

**open questions**
[needs decision] the five questions above

**done when**
[needs decision] acceptance criteria can't be written until question 1 is answered
[suggested] once it is: one observable check per in-scope item, plus the four states above

## assumptions
- requirements are read-only reference content, not something users author
- this is one screen inside a larger application flow, not a standalone product
- warnings come from the backend rather than being computed in the UI

## change log
- added out of scope, key states, data needed, and done when — all absent
- split "manage requirements" into a question, since it's the p0 blocking everything
- added empty, loading and error states as inferred
- proposed grouping and filtering as suggested defaults, not decisions
```

note the length. 12 gaps, 5 questions, and a draft roughly the size of a real spec.
that's the ceiling, not the target.

---

## 2. severity inflation

**bad — everything looks urgent, so nothing is**

```markdown
| no analytics events | p0 | ... |
| button copy not specified | p0 | ... |
| no hover state on rows | p0 | ... |
| grouping rule undefined | p0 | ... |
| no dark mode | p1 | ... |
```

five p0s, and the only real one is buried. the reader stops trusting the column.

**good — graded, with the consequence written out**

```markdown
| grouping rule undefined | p0 | grouping is the screen's whole information architecture — two designers build two different screens |
| no empty state | p1 | a new application has zero requirements, and it'll be caught in review instead of now |
| button copy not specified | p2 | convention covers it; changes no layout or logic |
| no analytics events | p2 | core ux is designable without it; add before production |
| no dark mode | — | not a gap. it's out of scope, and belongs in the out of scope section |
```

the test: finish the sentence "if this stays unanswered, then ___". if you can't
finish it concretely, demote it. if the answer is "nothing changes in the design",
it's p2. if it's "this isn't part of this version", it isn't a gap at all.

---

## 3. product-specific gap dressed as a default

this is the failure mode that looks compliant. the tag says `[suggested]`, so it
seems honest — but a business rule got decided by an agent.

**bad**

```markdown
## suggested best-practice defaults
- only admins can approve requirements (standard permission model)
- deleted requirements are soft-deleted and recoverable for 30 days
- the export includes all fields the user can see
```

all three are business rules. "standard permission model" is doing a lot of work
there. 30 days is invented. and what a customer can export is a data-governance
question, not a pattern.

**good**

```markdown
## questions for product/team
1. **who can approve a requirement?**
   why it matters: changes whether most users see an action or a disabled state.
   options: (a) admins only — *recommended, matches the risk*, (b) anyone on the application, (c) the assigned owner.
   owner: product. blocks: permissions and all denied states.

2. **is delete recoverable?**
   why it matters: soft delete needs a restore path, a retention window, and probably an audit trail. hard delete needs a much scarier confirmation.
   options: (a) soft, with a restore path, (b) hard, with a typed confirmation.
   owner: product + legal. blocks: the delete flow end to end.
```

the pass that catches it: does this touch money, permissions, legal/compliance, or a
promise the customer sees? approve → permissions. delete retention → compliance.
export scope → data governance. all three reclassify.

---

## 4. a draft that grew scope

**bad — 4 new features arrived during an audit**

```markdown
**in scope**
[confirmed] show requirements grouped by type
[suggested] bulk select and bulk resolve warnings
[suggested] saved filter views
[suggested] keyboard shortcuts for navigating rows
[suggested] a requirements activity feed
```

none of that was asked for. `[suggested]` doesn't make it okay — it's four new
features in a spec that was meant to get *clearer*, not bigger. this is the change
most likely to cost the team a week.

**good — same items, correct home**

```markdown
**in scope**
[confirmed] show requirements grouped by type
[suggested] group by requirement type, subcategory nested one level  ← detail on an existing item

**out of scope**
[suggested] bulk actions on requirements
[suggested] saved filter views
[suggested] keyboard navigation
```

the rule: clarifying an existing scope item is allowed. adding a scope item is not.
if it's a good idea that isn't in this version, it goes in **out of scope** — which is
genuinely useful, because it stops the same idea arriving mid-design.

---

## 5. false `[confirmed]`

**bad**

```markdown
[confirmed] users can manage requirements
[confirmed] requirements are grouped somehow
[confirmed] users can deal with requirements that have issues
```

every one of those is quoted from the input, and every one is the gap. tagging them
`[confirmed]` launders vagueness into agreement — the exact thing the tags exist to
prevent.

**good**

```markdown
[needs decision] "manage requirements" — view, edit, approve, add context? each is a different screen
[needs decision] grouping rule — "somehow" isn't a rule
[needs decision] what "issues" means, and what the user can do about them
```

the test: quote the line. could a reasonable person still ask "does this mean x or
y?" if yes, it is not confirmed, however literally present it is.

---

## 6. invented data

**bad**

```markdown
**data needed**
- id: uuid
- title: string, max 120 chars
- status: draft | pending | approved | rejected
- assignee_id: uuid, nullable
- due_date: iso 8601
```

confident, specific, plausible, and entirely made up. the spec never enumerated
statuses. someone will build against that enum. this is the single most expensive
thing this skill can get wrong, because it looks like research.

**good**

```markdown
**data needed**
[confirmed] a requirement has a name — the spec says they're listed by it
[needs decision] the full field list. engineering to confirm what exists today; i won't guess field names
[needs decision] the status values, if requirements have a status at all
[inferred] descriptions can be long enough to need clamping, since they're described as text
```

field names, enum values, and data sources are never safe to infer. if a flow needs
data the spec doesn't describe, that's a product-specific gap.

---

## 7. too many questions

**bad — 15 questions, which is the same as zero**

```markdown
1. who can add context?
2. can admins add context?
3. can regular users add context?
4. is add-context permission per requirement or global?
5. what happens if a user without permission clicks add context?
6. should the button be hidden or disabled?
7. what should the tooltip say?
8. can review start with warnings?
9. what if there's one warning?
10. what if there are many warnings?
...
```

items 1–7 are one question. items 8–10 are one question. nobody answers this list;
they schedule a meeting about it instead.

**good — merged to the real decisions**

```markdown
1. **who can add context, and what do people without permission see?**
   why it matters: decides whether the action is hidden, disabled, or visible-and-denied.
   options: (a) anyone on the application, action always visible — *recommended for v1*, (b) admins only, hidden for everyone else, (c) admins only, disabled with a reason.
   owner: product. blocks: permissions, the denied state, and the row layout.

2. **can review start while warnings exist?**
   why it matters: this is the enable/disable rule for the only primary button, at any warning count.
   options: (a) yes, with a warning summary — *recommended, keeps the user in control*, (b) no, hard block until all are resolved.
   owner: product. blocks: the primary action and its states.
```

merge by decision-owner or by flow step. cap at 5. push the rest into the gaps table,
where it's still visible but isn't asking for a reply.

---

## 8. rebuild mode

### input

> we want something that lets users see all their applications in one place and know
> which ones need work

### output — turn 1

```markdown
9/10 sections absent → rebuilding from zero.

this doesn't have enough to work from, so i'm asking instead of guessing. five
questions, then i'll draft a skeleton.

1. **what goes wrong today without this?** — do users miss deadlines, lose track,
   or duplicate work? this decides what the screen optimizes for.
2. **who is this for?** — one applicant managing their own applications, or someone
   overseeing many?
3. **what does "needs work" mean concretely?** — an approaching deadline, a missing
   document, a backend-flagged warning, or something else?
4. **what's the one action a user takes from this screen?** — open an application,
   or fix something in place?
5. **what's explicitly not in v1?** — creating applications, editing them, bulk
   actions, sharing?

once i have 1, 3 and 4 i can draft the skeleton. 2 and 5 sharpen it.
```

note what it does **not** do: invent a dashboard. with nine sections missing,
generating a full 10-section spec would be nine sections of fiction.

### output — turn 2, after answers

a skeleton, mostly `[confirmed]` from the answers and `[needs decision]` for the rest,
then the normal audit pass on it. don't jump straight to a polished spec.

---

## 9. sync mode

### input

the requirements spec, plus: *"we decided only admins can add context, and start
review stays enabled even with warnings — it just shows a summary first"*

### output

```markdown
sync mode — 2 decisions to fold into the existing spec.

decisions i'm applying:
1. only admins can add context
2. start review stays enabled with warnings, showing a summary first

## change log
- behavior rules: add-context is admin-only (per this conversation)
- behavior rules: start review no longer blocked by warnings; shows a summary (per this conversation)
- key states: added the denied state for non-admins, and the warning summary
- open questions: removed both — now decided

## changed lines

**behavior rules**
- was: [needs decision] who can add context to a warning requirement
- now: [confirmed] only admins can add context (per this conversation)

- was: [needs decision] whether start review is disabled while warnings exist
- now: [confirmed] start review is always enabled; it shows a warning summary first (per this conversation)

**key states**
+ [needs decision] what non-admins see on a warning requirement — the action hidden, or disabled with a reason?
+ [suggested] warning summary shown before the review starts

## new gaps these decisions introduced

| gap | severity | type | why it matters | suggested fix | needs human? |
|---|---|---|---|---|---|
| non-admin state undefined | p1 | product-specific | most users are non-admins, so this is the common case | disable with a reason, don't hide | yes |
| what the warning summary contains | p1 | best-practice default | it's now a required step before the primary action | count by type, with a link to each | no |
| whether a non-admin can request context from an admin | p2 | product-specific | otherwise non-admins hit a dead end | out of scope for v1 | yes |
```

everything else in the spec stays as it was. sync does not re-audit what nobody
touched, and it leads with what changed.

---

## 10. a question the spec already asked

the spec's open questions section says:

> - should start review be disabled if warning requirements exist?

**bad — charged as a p0 discovery**

```markdown
| start review state with warnings undefined | p0 | product-specific | two designers would build different primary CTAs | disable until resolved | yes |
```

readiness: blocked.

this is wrong twice. it grades the spec down for surfacing its own unknown, which is
the behavior you want more of. and it pushes the verdict to blocked over something
the team already knows about — burying the gaps nobody had spotted, which is the
whole reason to run an audit.

**good — acknowledged, and given options it didn't have**

```markdown
| start review state with warnings | p1 · already open | product-specific | it's the enable/disable rule for the only primary button | (a) allow with a warning summary — recommended, (b) hard block | yes |
```

readiness: needs fixes. main risk names it once, because it does gate the primary
action — but the spec gets credit for having asked.

the value you add here isn't finding the question. it's turning it into two options
and a recommended default, so it's answerable in one line instead of in a meeting.

---

## 11. worked compress

the same requirements screen as section 1, specced the other way wrong. section 1 was
starved; this one is fed too much. both are undesignable.

### input spec (as received)

```markdown
requirements section

background
our product helps teams respond to complex public applications. these applications are
long and our customers are usually small teams competing against much larger ones,
which is why we started the company. the requirements section is part of the
application flow, which is the core of the product and where most of our usage happens.
we have been iterating on this area since last year and learned a lot from customer
calls. requirements are loaded from the application document by our parser.
requirements always belong to exactly one application.

what we need
users can search requirements. there should be a search bar that filters the
requirements list. requirements can be searched by name. users may also find a
requirement using search when the list is long.

use a debounce of 250ms on the search input and store the query in local state so we
don't refetch on every keystroke. the list should use the existing table component
with virtualization if there are more than 100 rows.

for viewing a requirement's source, use a right-side drawer at 420px wide with the
source excerpt at the top and the parser confidence below it.

we discussed whether needs-attention should be a separate tab or a filter on the main
list. a tab is more discoverable but splits the count in two places. a filter keeps
one list but is easier to miss. both work. worth thinking about.

we will also need to handle localization for the 12 languages we plan to support, and
eventually bulk-resolving warnings, and an activity feed per requirement.

requirements are grouped by requirement type, with subcategory nested one level.
```

### output

```markdown
5 of 23 statements carry a decision → compressing.

## summary
- design-readiness: needs fixes
- main risk: the two real decisions in here — how source is inspected, and whether needs-attention is a tab or a filter — are buried under background, parameters, and future phases
- recommended next step: confirm the lean spec below is still your spec, then answer the two open questions
- confidence: high — nothing is missing that matters; it's all present and hard to find

## simplification opportunities

| issue | type | why it adds noise | suggested compression |
|---|---|---|---|
| search stated 4 ways | duplicate | a reader can't tell if that's one rule or four | one line: users can search requirements by name |
| 250ms debounce, local state | implementation detail | no consequence the user can see | results update as the user types |
| existing table component, virtualization over 100 rows | implementation detail | how it's built; the requirement is that long lists stay usable | long lists stay usable — engineering picks the mechanism |
| "right-side drawer, 420px, excerpt above confidence" | solution too early | closes the design question before design opens it | state the need, suggest the pattern |
| 6 lines of company history | background noise | the founding story changes no decision on this screen | 2 bullets of relevant context |
| tab vs filter discussion | unclear decision | 4 lines of weighing with no ruling, read as if it were a requirement | one [needs decision] line |
| localization, bulk resolve, activity feed | not v1 | three future phases sitting in the same list as v1 work | move to out of scope |
| grouping rule at the end, under background | misplaced content | the screen's whole information architecture, filed last | move to behavior rules |

## lean spec

**why**
[confirmed] requirements are parsed out of an application document, and users need to find and check them before submitting
[needs decision] what goes wrong today — the spec says the area matters, not what's broken

**user goal**
[inferred] as an applicant, i want to find a requirement and check what it's based on, so i can trust the application before i submit it

**in scope**
[confirmed] requirements list, grouped by type
[confirmed] search requirements by name
[confirmed] inspect what a requirement was parsed from

**out of scope**
[confirmed] localization (per the spec: a later phase)
[confirmed] bulk-resolving warnings
[confirmed] per-requirement activity feed

**core flow**
[confirmed] 1. user opens the requirements section
[confirmed] 2. user scans requirements grouped by type, or searches by name
[confirmed] 3. user opens a requirement's source to check it
[inferred] 4. user returns to the list with their place kept

**behavior rules**
[confirmed] grouping is by requirement type, subcategory nested one level
[confirmed] search filters the list by requirement name
[suggested] results update as the user types
[confirmed] a requirement's source shows the parsed excerpt and the parser's confidence
[suggested] source is inspected without leaving the list — side drawer is the obvious pattern, but that's design's call
[inferred] long requirement lists stay usable; the mechanism is engineering's call
[needs decision] is needs-attention a tab or a filter?

**data needed**
[confirmed] requirement name
[confirmed] the source excerpt a requirement was parsed from
[confirmed] parser confidence
[confirmed] requirement type and subcategory
[confirmed] a requirement belongs to exactly one application
[needs decision] anything else on a requirement — engineering to confirm; i won't guess field names

**key states**
[inferred] default list
[inferred] empty — no requirements parsed yet
[inferred] loading
[inferred] error on load, with retry
[inferred] search with no results
[needs decision] what a low-confidence parse looks like, if it looks like anything

**open questions**
[needs decision] tab or filter for needs-attention — the spec weighed both and didn't rule
[needs decision] what problem this fixes today, in one line

**done when**
[needs decision] acceptance criteria — the spec has none. one observable check per in-scope item plus the five states above

## what moved

| original | disposition | where it went |
|---|---|---|
| 4 search sentences | merged | one behavior rule |
| 250ms debounce, local state | dropped — implementation detail | replaced by "results update as the user types" |
| table component, virtualization over 100 | dropped — implementation detail | replaced by "long lists stay usable" |
| drawer at 420px, excerpt above confidence | moved | behavior rules, as a need plus a suggested pattern |
| 6 lines of company background | dropped — background noise | the two lines that mattered are now in *why* and *data needed* |
| tab vs filter discussion | moved | open questions, as one [needs decision] |
| localization, bulk resolve, activity feed | moved | out of scope |
| grouping rule | moved | background → behavior rules |

## gaps found — p0 and p1 only

| gap | severity | type | why it matters | suggested fix | needs human? |
|---|---|---|---|---|---|
| tab or filter for needs-attention | p1 · already open | product-specific | it's the list's primary navigation | (a) filter, one list, one count — recommended, (b) tab | yes |
| no acceptance criteria | p1 | best-practice default | nobody can agree when this is done | 5–8 observable checks | no |
| low-confidence parse behavior undefined | p1 | product-specific | confidence is displayed, so it implies a threshold nobody has set | define what counts as low, and what the user does about it | yes |
```

where `5 of 23` comes from, since the number is what justifies the mode: 23 statements
in, and only five help answer anything — requirements come from the parser, a
requirement belongs to one application, search is by name, source shows excerpt and
confidence, grouping is by type with subcategory nested. the other eighteen are
background, the same search rule three more times, two implementation notes, five lines
of undecided discussion, and three future phases.

note what compressing did **not** do. it didn't lower readiness for being long — the
verdict is `needs fixes` on the three gaps, not on the bloat. it didn't rule on tabs vs
filter, because the spec explicitly didn't. and every one of the eight rows in the
simplification table appears again in the ledger, which is the only reason you can
trust the short version.

---

## 12. over-compression

the failure mode of this mode. it looks like excellent editing.

**bad**

```markdown
**behavior rules**
requirements are grouped by type.
search filters by name.
source opens in a drawer showing the excerpt and confidence.
needs-attention is a filter on the main list.
```

four clean lines, and three things went wrong. "subcategory nested one level" was
deleted as too detailed — it's the information architecture of the screen, and it came
from the author. the tab-vs-filter question got *resolved* into a confirmed-looking
statement, which is the exact laundering the tags exist to stop. and the tags are gone,
so a reader can't tell any of this happened.

short and wrong is worse than long and complete. the long version at least still had
the decision in it.

**good**

```markdown
**behavior rules**
[confirmed] grouping is by requirement type, subcategory nested one level
[confirmed] search filters the list by requirement name
[confirmed] source shows the parsed excerpt and the parser's confidence
[needs decision] is needs-attention a tab or a filter? the spec weighed both, didn't rule

## what moved
| original | disposition | where it went |
|---|---|---|
| tab vs filter discussion | moved | open questions |
```

the check that catches it: a behavior rule, a data item, or a state is never *dropped*.
it can be merged, or moved, and either way it shows up in the ledger. if the ledger has
no row for it, it wasn't compressed — it was lost.

---

## 13. a good spec, run through the bloat pass

the pass has to be able to return nothing, or it will find something every time.

the spec has 14 fields listed under *data needed*, an *out of scope* section with 6
items, a *done when* section that restates three behavior rules as observable checks,
and 4 open questions.

**bad**

```markdown
## simplification opportunities
| 14 fields listed | too detailed | most of these don't change the layout | list the 4 that appear on the row |
| done when repeats behavior rules | duplicate | already stated above | remove the repetition |
| 6 items in out of scope | background noise | none of this is being built | trim to the 2 likely to come up |
| 4 open questions | unclear decision | the spec doesn't rule on these | resolve them |
```

every row is wrong, and the last one is dangerous — it reads as an instruction to
decide things the team deliberately hasn't. this is a spec being punished for doing its
job, and it's the same error as severity inflation wearing different clothes.

**good**

```markdown
no simplification opportunities — the spec is dense. the field list, the out of scope
section, and the repeated acceptance criteria are all those sections working correctly.
```

then the audit proceeds normally. `38 of 44 statements carry a decision` never routed
to compress in the first place — density was checked and the spec passed. note that the
4 open questions counted *toward* density, not against it.
