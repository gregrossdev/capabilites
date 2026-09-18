---
name: state-invariant-review
description: Review a specification, flow document, or feature against its implementation for state-machine and requirements consistency. Hunts impossible states, conflicting rules, missing transitions, transitions owned by more than one flow, unenforced invariants, and vocabulary drift. Use when a spec has grown detailed enough that the useful review is consistency rather than visual critique, when two documents describe the same behaviour differently, before implementing a model change, or when a reviewer says "these two rules contradict each other".
---

# State and Invariant Review

A specification is a claim about which states can exist and how they change. Review it as a machine, not as prose. The defects this finds are invisible in a screenshot and survive every visual review: they live in the gaps between documents, and between a document and the code.

## Use this skill when

- A flow document, PRD, or state model is detailed enough that visual critique has stopped paying
- Two sources describe the same behaviour differently, or you suspect they do
- A model change is about to be implemented and you want the blast radius first
- A reviewer reports a contradiction and you need to know whether there are more
- A feature has accreted flows over time and nobody can state its invariants

**Do not use this skill for** visual hierarchy, spacing, accessibility, or copy tone. Those are a design review. Reviewing both at once produces a list where the serious defects are buried among the cosmetic ones.

## Core workflow

### 1. Establish the state model deterministically

Read the implementation before the document. Extract, from code:

- every state value the model can hold, by name
- every field that modifies a state's meaning (flags, nullable dates, counters)
- every function that writes state

If there is no implementation yet, extract from the document — and say so in the report, because every finding then becomes a claim about intent rather than a verified defect.

**Never infer a state the source does not name.** If the document describes a condition that has no representation in the model, that is itself a finding.

### 2. Build the transition matrix

Every state × every state. For each non-empty cell record:

- the trigger (user action, automatic rule, or scheduled event)
- the flow that owns it
- the side effects on other fields

Include self-transitions. They hide the most defects — ageing, counters, retries.

### 3. Assign ownership

Every non-empty cell must name **exactly one** flow. Zero owners and two owners are both defects, and they are different defects.

### 4. Hunt the six classes

Work through them in this order. Later classes depend on the matrix being complete.

**1 · Impossible or unreachable states.** A combination the model permits but no flow produces. A state with no entry transition. A field combination that contradicts the state it sits in (`state: done` with a future scheduled date; `state: waiting` carrying a day).

**2 · Conflicting rules.** Two places defining one transition differently. The highest-yield pair to compare is *the state table* against *the flow that performs the transition* — they are written at different times and drift silently.

**3 · Missing transitions.** A state with no exit. An event with no defined outcome for some state it can occur in. An edge case with no rule: null, empty, first, last, zero, simultaneous, repeated.

**4 · Transitions with more than one owner.** Two flows that cause the same change, usually with different side effects. Ask which one runs when both apply. If the answer is "whichever the user reaches first", the side effects must be identical or it is a defect.

**5 · Unenforced invariants.** Every rule the document asserts as always true. For each, find the code that enforces it, or find the path that violates it. An invariant nothing enforces is a comment.

**6 · Vocabulary drift.** One concept with two names, or one name with two meanings, across documents or between document and code. This is how classes 1 and 2 get introduced in the first place.

### 5. Verify before reporting

For every candidate finding, confirm it against the implementation — run it, read the function, trace the path. Report `CONFIRMED` for defects you reproduced and `CLAIMED` for those you could only derive from the document. Do not mix them without labels.

### 6. Report

Lead with the transition matrix, then findings grouped by class and ranked by blast radius:

1. An invariant the build violates — the rule was load-bearing and is false
2. A conflicting rule — two behaviours, one of which is wrong for every user
3. A transition with two owners — behaviour depends on the route taken
4. An impossible state — corrupt data becomes reachable
5. A missing transition — a dead end or an undefined edge
6. Vocabulary drift — the seed of all of the above

For each finding give: **where it is** (both sources, cited), **what breaks** (a concrete sequence that produces the wrong state), and **the smaller correction** — which of the two sources should change, and why that one.

## Rules

- **The implementation is the source of truth for what is; the document is the source of truth for what was intended.** When they disagree, that is the finding — do not silently pick one.
- **Every finding cites two locations.** A finding with one citation is an opinion.
- **Prefer the correction that removes a rule** over the one that adds one.
- **State the invariants you verified as holding,** not only the ones that failed. A reviewer needs to know what was checked.
- **Propose no new features.** If a missing transition needs a product decision, name the decision and stop.
- **One defect may have several symptoms.** Group them. Five failing screens caused by one wrong token is one finding, not five.

## Anti-patterns

- Reporting every matrix cell as a finding
- Accepting the document's own summary of its rules without tracing the code
- Calling it a conflict when two statements are the same rule at different altitudes
- Reviewing tone, naming aesthetics, or layout under cover of consistency
- Producing a matrix but no ownership column, which hides classes 3 and 4
- Declaring an invariant verified because the code contains a comment asserting it

## Example

```text
User: "Here's the flow document and the build. Review it for consistency."

Expected behaviour:
1. Extract states and modifiers from the code, not the document.
2. Build the full transition matrix with owners.
3. Find: the state table says a state may be undated, while the flow that
   creates it converts undated to a different state — class 2.
4. Find: the close routine therefore has no rule for the undated case — class 3,
   and the same defect as above seen from the other end. Report once.
5. Find: the document asserts "only X puts work into a week"; three paths do —
   class 5. Propose the restated invariant that is actually true.
6. Verify each by running the sequence; label CONFIRMED.
7. Report matrix, then findings by blast radius, each with the smaller fix.
```

## Before finishing

- Is every state in the matrix, including ones the document does not mention?
- Does every non-empty cell name exactly one owning flow?
- Does every finding cite both the document and the implementation?
- Is every finding labelled CONFIRMED or CLAIMED?
- Have you grouped symptoms of one defect into one finding?
- Have you listed the invariants that held, not only those that failed?
- Have you stayed out of visual design entirely?

## References

- `references/defect-classes.md` — each class with worked examples and the question that exposes it
