# examples

calibration for [SKILL.md](SKILL.md). copy the *shape* and the *tone*, never the content.

---

## 1. a full review

### the spec as received

```markdown
# Tender notes

Users can add notes to a tender so they remember why it matters. Notes show up on the
tender page under the description. Users can edit and delete their notes. We should
also let people @mention teammates in a note and get a notification, and maybe
attach files. Notes are saved automatically.
```

### what comes back

```markdown
**Can design start?** Yes, on the notes box and the tender page. Hold the visibility
rules until decision 1 is answered.

## Decisions needed (3)

- [ ] **1. Who can read a note?**
  Example: Sam adds a note "we can't win this, price too low" to a tender. His colleague
  Dee opens the same tender. Does she see Sam's note?
  a) Only Sam sees it ← I'd pick this, it's the safest to start with
  b) Everyone in the company sees it
  c) Sam chooses each time

- [ ] **2. Are @mentions part of this version?**
  Example: Sam writes "@Dee can you check the deadline?" Does Dee get a notification?
  a) No, later ← I'd pick this, it needs its own notification design
  b) Yes

- [ ] **3. What does "deleting a note" mean?**
  Example: Sam deletes a note by accident an hour later. Can he get it back?
  a) It's gone for good, and we ask "are you sure?" first ← I'd pick this
  b) It's gone, but there's an Undo for a few seconds

**Smaller things, can wait until build:** the maximum note length, and whether an
edited note shows "edited".

## What we're building

**Problem.** Users lose track of why a tender caught their eye.

**Goal.** Write a note on a tender and find it there next time.

**What users can do**
- Add a note on the tender page, under the description
- Edit and delete their own notes
- Notes save automatically as they type

**Not in this version.** @mentions and notifications (decision 2) · file attachments

*Assumed unless you say otherwise: the usual empty, loading and error screens.*

## What I changed
- "@mention" and "attach files" were wishes inside the description. I moved them to
  "not in this version" so they don't sneak into the design.
```

why this is right:

- the top line answers "can we start?" before anything else
- 3 questions, each one question, each with a person and a situation
- nothing in the body that the author didn't write, and nothing decided that the team hasn't
- the "smaller things" line means nothing was dropped, and nothing is a question yet
- about the same length as the input

---

## 2. answers come back

### what the user pastes

```markdown
Decision 1: b, everyone in the company sees notes. Decision 3: b with undo.
On 2, we want mentions, but as a follow-up.
```

### what comes back

```markdown
**Updated.** 3 decisions answered, 1 new question.

**What changed**
- Decision 1 → notes are visible to everyone in the company. Added to the spec.
- Decision 3 → deleting shows "Note deleted · Undo" for a few seconds. Added to the spec.
- Decision 2 → mentions moved to "not in this version", listed as the next step.

## Decisions needed (1)

- [ ] **4. Can someone edit or delete a colleague's note?**
  Example: Dee sees Sam's note, and it's out of date. Can she change or remove it?
  a) No, only the author ← I'd pick this, people trust notes that only the author can change
  b) Yes, anyone

## What we're building
[the full spec again, with the three answers written in and nothing else touched]
```

why this is right:

- the answers went into the spec and left the list, so the list is always what's still open
- the new question is number 4, not 1: numbers never change, because the team already wrote "decision 1" in comments
- one new question, because "everyone can see it" created it, and no more than one level of follow-up
- every untouched line stayed exactly as it was

---

## 3. an idea, not a spec

### what the user gives

```markdown
we need a way for users to compare two tenders side by side
```

### what comes back

```markdown
There isn't enough here to write a spec without guessing, so here are questions instead.

1. What problem does this solve? What is someone doing today that this makes easier?
2. Who is it for, and what do they want to leave with (a decision, a shared link, a printout)?
3. What would they compare? Example: deadline and value, or the full requirements?
4. What's the first thing you'd leave out of version one?

Answer these and I'll turn them into a first draft.
```

why this is right: no spec was written, because writing one would be inventing one.

---

## 4. good and bad

### the question

```markdown
bad   Should the entity's approval pathway be delegated or hierarchical, and is
      escalation asynchronous?

good  **Who approves a tender bid?**
      Example: Sam finishes a bid worth 200,000. Does it go straight to his manager,
      or to whoever the company picked for bids?
      a) His manager ← I'd pick this
      b) A person the company picks
```

### one question or three

```markdown
bad   which plan gets Instant alerts, what do free users see, and is the word right?

good  three separate questions. the two that don't block design go in "smaller things".
```

### the body

```markdown
bad   Notes are visible to all teammates.        ← nobody decided this. it's a guess written as fact

good  Who can read a note? → decision 1          ← the body points to the question, or leaves it out
```

### the language

```markdown
bad   ingestion runs nightly, so instant alerts are batch-delivered at most once per cycle
good  tenders arrive once a night, so "Instant" really means "right after the nightly update"

bad   p1 · product-specific · already open
good  (nothing. the reader doesn't need our labels)
```

### how much to say

```markdown
bad   a paragraph explaining that free users see a disabled option, why disabled beats
      hidden, and three alternatives

good  a) Greyed out with a tooltip ← I'd pick this, people learn the plan has more
      b) Hidden
```

### standard behavior

```markdown
bad   a list of 6 "safe fixes": loading, error, empty, retry…

good  one line: "Assumed unless you say otherwise: the usual empty, loading and error screens."
```

### build detail

```markdown
bad   body: "Each note is stored in the notes table with a soft-delete flag, and requests
      carry an idempotency key so retries don't create duplicates."
      ← nobody using the product sees this, and it buries the rules that matter

good  body: "Deleting a note shows Undo for a few seconds."
      Build notes (end of the spec):
      - notes table uses a soft-delete flag
      - create requests carry an idempotency key, so retries don't duplicate
```

### the spec disagrees with itself

```markdown
bad   goals say "saved views"; the last section says saved views are out of scope.
      → quietly moved to "Not in this version"      ← you just decided for the team

good  a decision: "Are saved views in this version?"
      Example: Ana filters to her own findings and wants to keep that filter for next
      time. Can she save it?
      a) Not in this version ← I'd pick this, the last section already says so
      b) Yes
```

### who can do what

```markdown
bad   "who can dismiss a finding" left out, or assumed to be "everyone"

good  smaller things: "who can dismiss a finding?"  (written as a question, never assumed)
```

### a long spec

```markdown
bad   5,300 words in, 4,600 words out, the same spec with a few lines trimmed

good  about 2,600 words of product spec, plus the decisions, plus a short Build notes list
      the goals, the summary and "done when" said the same things three times. now once
```
