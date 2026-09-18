---
name: generate-ux-patterns
description: Generate alternative UX interaction patterns for a feature or workflow. Use when deciding how a feature should behave, how users move through a task, how information is presented, or when exploring multiple interaction models before implementation.
---

# UX Pattern Generator

Generate meaningfully different interaction patterns for the same product
capability.

This is not about visual style. The question is: **what interaction model should
this feature use?**

A feature can usually be built through several valid patterns. Generate the
alternatives before choosing one.

## Core method

1. State the user's goal in one sentence.
2. Identify the primary interaction verb (below).
3. Gather the decision drivers (below) — they eliminate most candidates before you generate anything.
4. Select 3–5 patterns that are genuinely different interaction models.
5. Give each its tradeoffs and the conditions where it wins.
6. Recommend one, with the driver that decides it.

Unlike visual directions, UX patterns are usually decidable. Volume, frequency,
reversibility, and device generally pick the winner. Say which one did, and say
what would change the answer.

## Decision drivers

Ask these before generating. They do more work than the pattern catalog.

- **Volume** — 5 items or 5,000? Decides list vs. table vs. paged review.
- **Frequency** — daily habit or once a quarter? Habitual use rewards keyboard and density; rare use rewards guidance and explicit labels.
- **Attention per item** — does each item need real thought, or is the decision near-automatic? Decides sequential vs. bulk.
- **Reversibility** — can a mistake be undone? Irreversible actions earn friction; reversible ones should get undo instead of a confirmation dialog.
- **Expertise** — first-time user or power user? Decides guided vs. direct.
- **Device and input** — mobile touch, desktop mouse, keyboard-heavy? Eliminates whole families (no hover affordances on touch, no dense data grids on phones).
- **Interruption** — is the task completed in one sitting or resumed later? Resumable tasks need persisted state and a way back in.
- **Comparison need** — must users see items against each other, or one at a time?

## Interaction verbs → candidate patterns

Start from the verb; it narrows the catalog fast.

- **Viewing** — feed, list, table, card grid, timeline, calendar, dashboard
- **Finding** — search, faceted filters, filter chips, filter drawer, saved views, sort, grouping
- **Choosing** — single/multi select, combobox, autocomplete, picker, segmented control
- **Entering** — single field, form, composer, quick create, template-based create, import
- **Editing** — inline edit, autosave editor, explicit save form, drawer, structured builder
- **Reviewing** — guided review (one at a time), batch review, triage inbox, approval queue, review table, diff view
- **Organizing** — tags, categories, folders, collections, favorites, pinning, grouping, drag and drop
- **Navigating** — sidebar, icon rail, top nav, bottom nav, tabs, breadcrumbs, command palette
- **Progressing** — stepper, wizard, checklist, pipeline, kanban, progress indicator
- **Comparing** — side-by-side, comparison table, diff view, before/after
- **Acting** — primary button, toolbar, context menu, overflow menu, FAB, swipe actions, bulk actions

Detail presentation cuts across all of them: modal, dialog, drawer/bottom sheet,
inspector, master–detail, dedicated page, expandable row, accordion.

Feedback and confirmation likewise: toast, inline validation, banner, alert,
empty state, skeleton loading, undo, soft delete, confirmation dialog,
destructive confirmation (typed, checkbox, or second-stage).

Assume the standard behavior of each pattern is known. Only spell one out when
the variant matters to the decision.

## Patterns worth arguing about

A few defaults are reached for reflexively and are often wrong:

- **Modal for everything.** Modals block context and break deep linking. If the user needs the underlying content while working, use a drawer, inspector, or page.
- **Confirmation dialogs on reversible actions.** Undo is better for anything recoverable — it costs nothing on the happy path. Reserve confirmation for irreversible operations.
- **Infinite scroll on task collections.** Fine for browsing, bad for anything the user must finish or return to. No sense of completion, no stable position.
- **Wizards for short tasks.** A stepper on a 3-field form adds ceremony without reducing complexity. Wizards earn their weight when later steps genuinely depend on earlier answers.
- **Tables as the default collection.** Dense and comparable, but heavy on mobile and poor for items whose value is visual or narrative.
- **Toast as the only confirmation of something important.** Transient and easily missed; if the user needs proof it happened, show state, not a toast.
- **Hover-revealed actions.** Invisible on touch and to keyboard users.

Where the product has an established convention, consistency usually beats the
theoretically better pattern. Say so when that is the reason.

## Constraints

Before generating, identify what is fixed:

- patterns already used elsewhere in the product (consistency has real value)
- platform conventions the users expect
- accessibility requirements — keyboard path, focus management, screen reader behavior
- data shape that rules patterns in or out (no kanban without discrete states, no calendar without dates)
- existing components or design system inventory

Do not invent product capability to make a pattern viable. If a pattern requires
a capability the product lacks, say that explicitly as its cost.

## Patterns combine

Real features stack several patterns across layers. Name the combination, not a
single pattern.

Email inbox: sidebar navigation · list collection · search + facets ·
master–detail · toolbar + context menu · triage workflow

Weekly planning: stepper workflow · grouped list · approve/reject selection ·
date picker · progress indicator

## Output format

### [Pattern Name]

**Core pattern** — the primary interaction model.

**Supporting patterns** — the secondary ones it needs to work.

**How it works** — the actual flow, step by step: what the user sees, what they
do, what happens next, how they finish or resume.

**Strengths** — what this handles well.

**Tradeoffs** — what becomes harder, and what it rules out.

**Best when** — the driver conditions where this one wins.

### Comparison

| Option | Core model | Attention per item | Volume fit | Device fit |
|---|---|---|---|---|
| Sequential Review | guided, one at a time | high | low–mid | keyboard/mobile |
| Triage Inbox | dense list + bulk | low | high | desktop |
| Review Table | data grid + inline edit | mid | high | desktop |

### Recommendation

One option, the driver that decides it, and what would change the answer.

## Worked example

Feature: review 30 incoming items.

- **Sequential Review** — guided review, one at a time, keyboard actions. Wins when each decision needs real attention.
- **Triage Inbox** — dense list, multi-select, bulk actions. Wins when decisions are repetitive and similar.
- **Card Deck** — single-focus cards, swipe or keyboard. Wins when each item is understood at a glance and the context is mobile.
- **Review Table** — data grid, inline editing, bulk approval. Wins when users need to compare items against each other before deciding.

These are four interaction models, not four skins.

## Diversity check

Before presenting:

- Would these produce **different code**, not just different CSS?
- Does each handle the volume in the brief, or is one obviously broken at scale?
- Have I included at least one option that is not the category's default?
- Does each have a keyboard path and a mobile story, or is that a stated tradeoff?
- Am I offering variants of one model (list vs. denser list) rather than different models?

## Principle

Keep the layers separate:

- **Capability** — what the product does
- **UX pattern** — how the user interacts with it
- **Visual design** — how that interaction looks
- **Implementation** — how the system works underneath

Example: *review the upcoming week* (capability) · *multi-step guided review*
(UX) · *quiet editorial interface* (visual) · *state machine with a persisted
draft session* (implementation).

Do not collapse these layers. This skill works at the UX layer only — visual
direction is a separate exercise.
