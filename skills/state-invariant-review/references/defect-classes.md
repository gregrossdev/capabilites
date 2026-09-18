# Defect classes, with the question that exposes each

Worked examples use a weekly planning app whose task states are `waiting`, `on` (in the current week), `next` (committed to the following week), and `done`.

## 1 · Impossible or unreachable states

**Question:** for every combination the model permits, which flow produces it? For every state, which flow enters it?

A model with a `state` enum and three independent modifier fields permits more combinations than the flows produce. Most are harmless; the dangerous ones are combinations that contradict the state they sit in.

```text
state: done      + scheduledFor: <future date>   → finished work claiming a future slot
state: waiting   + day: "Tuesday"                → not in any week, yet carrying a day
state: next      + slipCount: 3                  → a slip counter on work that was never live
```

Look hardest at fields that are *cleared* by some transitions and not others. A field cleared in three of four exits is a defect in the fourth.

**Also check reachability backwards:** a state no flow enters is either dead code or a missing transition.

## 2 · Conflicting rules

**Question:** for each transition, how many places describe it, and do they agree?

The highest-yield comparison is the **state table against the flow that performs the transition**. They are written at different moments and drift without either being edited.

```text
State table:  "next — committed to next week, with a day or without one"
Push flow:    "choose no day and it goes back to waiting"
```

Both are plausible. Only one can be the rule. The finding is not "the flow is wrong" — it is that two sources disagree, and the product decision has never actually been made.

Second-highest yield: **a rule stated in the summary versus the same rule stated in the detail**. Summaries simplify, and the simplification becomes false.

## 3 · Missing transitions

**Question:** for each state, what are its exits? For each event, what happens in every state it can occur in?

Three shapes:

- **Dead end** — a state with no exit under any flow.
- **Undefined event** — an event defined for some states and silent for others. *"Closing the week promotes committed work"* — and the undated committed work?
- **Unruled edge** — null, empty, zero, first, last, simultaneous, repeated. Ask each one explicitly; they are almost never written down.

A missing transition and a conflicting rule are frequently **the same defect seen from two ends**: one source permits a case the other never handles. Report it once, name both ends.

## 4 · Transitions with more than one owner

**Question:** which flow causes this change — and if the answer is more than one, do they have identical side effects?

Two owners are only a defect when the side effects differ. Test it:

```text
Route A: item sheet → "Take it on"     → state: on          (day preserved)
Route B: plan queue → pick a day       → state: on, day set (slip history cleared)
```

Same destination state, different resulting record. Whichever route the user happens to take changes their data. Either unify the side effects or make one route the only one.

Ownership defects hide behind convenience features — the shortcut added later that duplicates a transition the canonical flow already owned.

## 5 · Unenforced invariants

**Question:** for every sentence in the document containing *always*, *never*, *only*, or *must* — which code enforces it, and what path violates it?

This is the highest-value class, because an invariant is the thing everyone reasons from. When it is false, every downstream decision inherits the error.

```text
Asserted:  "Only the planning screen puts work into a week."
Reality:   three paths do — the planner, the push action, and the item sheet.
```

The correction is rarely to remove the paths. It is to state the invariant that is actually true and still useful:

```text
"The planner is the only place a week is filled in bulk.
 A single item may be moved into or out of a week from its own controls.
 Nothing enters a week as a side effect."
```

Note the third clause. A restated invariant needs the line that makes it falsifiable in future, or it is a description rather than a rule.

## 6 · Vocabulary drift

**Question:** does each concept have exactly one name, and each name exactly one meaning?

```text
"pushed"  /  "deferred"  /  "next week"     — one concept, three names
"waiting" — in the document: not yet in a week
          — in one flow:     in a week but undated
```

Drift is upstream of every other class: two names let two rules diverge without anyone noticing they are about the same thing. Fix it first, then re-run classes 1–5 — several findings usually dissolve.

Include code identifiers in the check. A field called `nextDay` holding the sentinel string `"No day yet"` in one path and `null` in another is drift inside a single model.
