---
name: spec-audit
description: >-
  Reviews a product spec before design starts and returns a short, plain-language
  version of it: whether design can start, the few decisions the team still has to
  make (each with an example and a recommended answer), and a clean spec that contains
  only what is decided. Catches gaps that would force design to invent, and bloat that
  hides what matters. Also updates a spec when decisions come back from comments or a
  call, and shortens a spec that has grown too long. Never silently invents
  product-specific rules. Use when a spec is about to go into design, when decisions
  were made and the spec needs to match, when a rough idea needs turning into a spec,
  or when a spec is too long to act on. Works for components, screens, flows and
  features. Triggers on: spec audit, audit this spec, is this spec ready, design-ready,
  spec review, fill the gaps in this spec, write a spec, update the spec, compress this
  spec, spec is too long, trim the spec, simplify the spec, cut this down.
argument-hint: "[spec link, pasted spec, or rough idea] [optional: decisions or comments to fold in]"
---

# spec audit

Make a spec ready for design, and easy for anyone to read.

The reader is a teammate who joined yesterday. They have not been in the meetings, they are not technical, and they have five minutes. If they have to ask someone to explain it, the output failed.

One rule sits under everything: **you can propose, but you must not silently decide.** A product rule the team hasn't chosen is a question, never a fact.

## Inputs

1. **The spec.** A Linear issue, Notion page, doc link, pasted text or file. Fetch it in full with whatever integration is available. If there isn't one, ask for the text. Never work from a paraphrase or from memory.
2. **What it describes:** a component, screen, flow or feature. Ask if it isn't clear.
3. Optional: the parent spec, decisions or comments from a call or thread, design system docs.

If nothing was given, ask. Don't start.

## What to do

Pick by looking at the input. Don't ask which one, and don't announce the choice.

- **Comments or decisions came with it** ("we decided", pasted Linear comments, meeting notes). Update the spec. Check this first, so a good spec plus one new decision never gets re-audited and the decision lost.
- **There's hardly a spec** (no problem and no user goal, or most of it missing). Don't write one. Say plainly there isn't enough to work from, ask the problem and the goal first, then only enough to sketch what's in, what's out and the main flow. Never turn an idea into a full spec in one go, because that is where guessing happens.
- **Everything else.** Review it and return the short version. If the spec is long, shortening it is part of the job.

### Updating a spec

1. Start with what changed, in a few lines, one per decision, so the reader can confirm you understood.
2. Write each answer into the spec where it belongs, and remove that decision from the list.
3. Touch only the lines the answers affect. Everything else stays exactly as it was.
4. If an answer opens a new question, ask it, once. Don't chain more than one level.
5. If the spec you were given is still the raw original and hasn't been through this skill, clean it while you fold the answers in, as in a normal review. Give the spec back with one heading per section, not the original's layout.

## How to review

Read the spec against these. They are for you. Don't turn them into headings.

- **Why.** What problem, for whom, why now. (If this is part of a bigger spec that already says it, skip it.)
- **Goal.** One sentence: what does the user get done?
- **In and out.** Could a designer point at the screen and say "that exists"? What's the small idea most likely to get added halfway through?
- **Main flow.** Where the user comes from, what they do, where they end up.
- **Rules.** What appears when, what each action does. This is where specs are weakest, so spend the most time here.
- **Data.** Every field, status, limit and fallback. What does it look like with real, ugly, incomplete data?
- **Screens that matter.** Only the ones that change the design.
- **Done.** How would two people disagree about whether it's finished?

Then look for three more things:

- **Contradictions.** The same word meaning two things, two rules that can't both be true, something both in and out of scope. A spec that contradicts itself is worse than a silent one, because both readers think they're right.
- **Vague words.** "Manage", "handle", "review", "improve", "process". Each one hides a decision.
- **Bloat.** Repeated rules, how-it's-built detail nobody using it would notice, exact sizes or numbers stated where the design should choose, future ideas written as requirements, company background that changes nothing. Keep numbers that came from the business ("filed within 15 working days"). Keep long lists of data fields, long "not in this version" lists and long open questions: those sections are meant to be long.

## Is it a decision?

Some gaps you fill, some you ask. Check this table first.

| Case | What to do |
|---|---|
| Button copy, loading, obvious format checks (an email looks like an email) | Assume it |
| Empty and error screens: how they look | Assume it |
| What *triggers* an empty or error screen | Ask |
| Rules about business limits (must be under budget, must match a customer) | Ask |
| Who can approve, delete, pay, export or share | Ask, always |
| Who can do something small and reversible (reorder their own list) | Assume it |
| Default sort or grouping | Assume it, unless the items have a priority (risk, deadline, amount). Then ask |
| Deleted for good, or kept and hidden | Ask, always |
| Confirming before something destructive | Assume it |
| Exact wording where legal or sales language applies | Ask |
| Any field name, status or data source the spec doesn't mention | Ask. Never invent |
| Whether an agent's action needs a human to approve it | Ask |

If it isn't in the table: does the answer follow necessarily from what's already in the spec? Assume it. Does it depend on business rules, permissions, data, legal, pricing or strategy? Ask.

**Check the assumptions once more.** For everything you were about to assume, ask: does it touch money, permissions, legal, or a promise a customer will see? If yes, it's a decision, however standard it feels. "Save a draft automatically" is an assumption. "Save automatically, over someone else's edits" is a decision. When in doubt, ask: if a wrong guess would be costly to undo or embarrassing in front of a customer, ask.

**If the spec already lists a question as open, it's a decision.** Never fill it in on your own, and give the team credit for spotting it.

## What comes back

Use this shape. Normal capitalisation, plain words, short sentences. No labels from this skill (no severity codes, no "product-specific", no tags).

```markdown
**Can design start?** [Yes / Partly / Not yet.] [One sentence: what can start, what waits for which decision.]

## Decisions needed (N)

- [ ] **1. [One question.]**
  Example: [A named person in a real situation.]
  a) [Option] ← I'd pick this, [one-line reason]
  b) [Option]

**Smaller things, can wait until build:** [short phrases, comma-separated, at most about 8. Group similar ones. No explanations.]

## What we're building
[the spec: only what is decided]

*Assumed unless you say otherwise: [one line.]*

## Build notes
[only if the spec has how-it's-built detail. one line each.]

## What I changed
[only if something was cut or moved that a reader might look for. one line each. max 5.]
```

**Can design start.** "Yes" means a designer could draw the main screen today. "Partly" means some screens can start and some wait. "Not yet" means the main screen would be a guess. Always say what waits for which decision.

**Decisions.**
- A short checklist with numbers, so the team can say "decision 2" in a comment and tick it when it's settled.
- Each one is **a single question.** If it's really two questions, split them.
- Never point at "the original spec", "the prototype" or "later sections". The reader won't have them, and this output replaces the original. Say what the spec says ("another part of this spec says recheck is out"). Only "What I changed" may mention the original.
- Each one has an **example with a person in it** ("Ana is on the free plan and has 3 saved searches. She clicks Save a 4th time. What happens?"). This is what makes an abstract rule easy to answer.
- 2 or 3 options, and the one you'd pick with the reason in a few words. A recommendation turns a meeting into a one-line reply.
- **At most 5.** Rank by design impact: first the ones that change which screens exist or what's on the main ones (a button, a modal field, a page, what a number means). Then the ones that change a rule. Edge cases (a huge result, a bounced email, a deleted user) go in the "smaller things" line unless they change the layout. Rank by impact, not by whether the spec already listed the question. Nothing is lost: every gap is either a decision or in that line.
- Ask only what the spec is silent or unclear on. If the spec states the behavior, it's decided, even if you would design it differently. Never invent a question to look useful. Zero decisions is a fine answer.
- **Permissions never slip.** Who can approve, delete, dismiss, edit for everyone, export or share is always a decision. If it doesn't fit in the 5, it goes in the "smaller things" line, grouped into one item: "who can do what: dismiss, undo, assign, manage company checks". It is never assumed and never left out.
- **One decision can cover a group** when every item is the same yes-or-no question. Example: five things the goals list but a later section rules out become one decision, "Which of these are in this version?", with the items as options to tick. That is still one question.
- Two rules that collide deserve the top slot. Example: alerts are "instant" but the data only arrives once a night. That decides what the feature promises.
- **Numbers never change.** The team writes "decision 2" in comments. When you update, answered decisions leave the list, and new ones take the next number (if 1 to 5 were asked, the next is 6). Never renumber.

**The spec.**
- Contains only what the author wrote (in clearer words) plus what the team has decided. Nothing you added, nothing guessed. Anything you would add goes in a decision, or in the one "assumed" line.
- Where an undecided thing would sit, write "→ decision 2" at the end of the line it affects, or leave it out. Never attach a pointer to a line it isn't about. Don't scatter question marks through the body. Each open question lives in exactly one place, the list.
- If the spec already reads well (nothing vague, repeated or contradictory), don't give it back. Say "The spec reads well as is" and give only the decisions and the smaller things. Rewrite only what needs it.
- Use only the headings the spec needs. A simple component needs three, a feature maybe six. Typical ones: Problem, Goal, What users can do, Rules (plain sentences), Not in this version, How we'll know it worked. Leave out empty ones.
- Keep the author's words and product terms. If a line has nothing wrong with it, leave it alone. You may merge bullets that say the same thing and tighten long ones.
- Keep every requirement, data field and rule that a user or designer would see, once. Merge anything the spec says in several places (goals, summary, "how it works" and "done when" often repeat each other). Future scope moves to "Not in this version" only if the spec itself says it is out. Only repeated lines and background may be dropped.
- **How it's built goes to Build notes, not the body.** Data tables, model or provider choices, retries, idempotency, storage and access plumbing, viewer internals, performance and logging rules: things nobody using the product would see. Nothing is deleted. It moves.
- **If the spec contradicts itself about scope**, for example a goal that a later section rules out, that is a decision. Never settle it by moving the item to "Not in this version" yourself.
- Keep the same scope. If the original had 7 things in scope, the new version has 7, or a decision saying why not.

**Build notes.** A short list at the very end, one line per item, in the author's technical words (this is the one place technical words are fine). Keep the exact names, numbers and limits. Skip the section if the spec has none. It exists so engineering detail is kept but nobody reading for the product has to wade through it.

**Assumed line.** One line, usually loading, empty and error screens. Add anything else you assumed that would change the design if wrong (for example "desktop only"). Nothing about standard software behavior gets its own list.

**What I changed.** Skip it when there is nothing a reader would go looking for. Otherwise one line for each real cut or move, especially anything that left the body ("moved to not in this version"). Merged duplicates don't need a line.

## Cut, then cut again

Before you send, run this on the output:

1. Cut it in half. For every line: if the reader wouldn't miss it, delete it.
2. Then cut what's left that only exists to show your work: mode explanations, counts, labels, repeated reasons.
3. Length:
   - **Spec under about 1,500 words:** the spec body is shorter than the original. Decisions come on top, and the whole output stays close to the original's length. (Exception: input under about 200 words.)
   - **Spec over about 1,500 words:** the spec body, on its own, is about **half** the original. Decisions and Build notes come on top of that. Get there by merging what's said twice, moving build detail to Build notes, tightening long bullets and turning long explanations into one line. Never by dropping something a user would see.
   If you're over, cut again.

The output should read in about five minutes. Nothing important is trimmed: a rule, a data field, a decision, a not-in-scope item survives every cut. Only words go.

## Words

- Write like you're explaining to a friend who doesn't work here. Short sentences, everyday words.
- Every question has a person in it.
- No labels from this skill. No "P0", "gap", "best practice default", "product-specific".
- Swap technical words for what the user sees. "The nightly update" instead of "ingestion". "What the user sees" instead of "state". "Cut off with …" instead of "clamp". "Loading, empty and error screens" instead of "states".
- This applies to the whole spec body, not only the questions. Keep names people see in the product (a button, a tab, a status like "Needs review"). Rewrite technical explanations in everyday words ("saved so it can't change later", not "immutable snapshot"). If a technical word is one the reader needs (an API, a plan name), keep it and say what it means once.
- One idea per sentence. If a sentence has an "and" and a "but", split it.

## Guardrails

- **Never fill product-specific gaps as facts.** If it depends on business rules, permissions, data, legal, customer expectations or strategy, ask.
- **Never invent a field name, status value or data source.** If the flow needs data the spec doesn't describe, ask what exists.
- **No new scope.** Clarifying an existing item is fine. Adding a new one is not.
- **Never delete a requirement.** Move it.
- **Never edit the source.** Give the output in the conversation for the human to paste. Don't update the Linear issue or Notion page, even if you can.
- **Don't grade the author.** Review the spec.
- **Don't inflate.** If the spec is in good shape, say "Yes, design can start" with a decision or two, or none. Don't invent problems to look useful.

## Before you send

1. The first line answers "can design start?"
2. Every decision is one question, has a person in the example, and has a recommended option.
3. At most 5 decisions, ranked by design impact, numbers never reused. Each open question appears once.
4. The spec body has nothing the author didn't write or the team didn't decide.
5. Nothing in the spec was invented: no field, status or data source.
6. No requirement, data field or rule was lost.
7. Nothing you assumed touches money, permissions, legal or a customer promise.
8. No jargon from this skill or from engineering.
9. Not longer than the input, and about half of it if the spec was long.
10. Build details are in Build notes, not the body. Nothing about who-can-do-what was assumed, and no scope contradiction was settled by moving the item.

If you're unsure how short or how plain to go, look at [EXAMPLES.md](EXAMPLES.md).
