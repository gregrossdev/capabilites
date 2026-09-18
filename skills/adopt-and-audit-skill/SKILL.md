---
name: adopt-and-audit-skill
description: Adapt an existing third-party or public skill into a personalized version, then re-audit that personalized skill on a recurring basis to keep it accurate and lean. Use when the user shares a skill they found and wants it fitted to their stack and workflow, when they say a skill is stale, bloated, or not firing, or when they ask for a skill review, audit, or cleanup. Not for creating a skill from scratch.
---

# Adopt and Audit

Two modes on the same artifact.

**Adopt** — a skill exists somewhere (a repo, a gist, a colleague, a public
catalog). Fit it to this user's actual stack, conventions, and workflow.

**Audit** — a personalized skill already exists. Re-check it against what has
been learned since, and cut whatever no longer earns its place.

Start in Adopt if the skill came from outside. Start in Audit if it is already
theirs. When unsure, ask which.

> Not for building a skill from nothing. For that, use `skill-creator`, which
> also owns eval harnesses, trigger-description optimization, and packaging.
> This skill is narrower: it fits an existing skill to one person and keeps it
> from rotting. If an audit turns up a triggering problem that needs measuring
> rather than reasoning about, hand off to `skill-creator` rather than
> reimplementing it here.

---

## Mode 1: Adopt

### Step 1 — Read it as-is, before changing anything

Summarize back, in three or four lines: what it does, when it claims to fire,
what it assumes about the environment, and what it actually enforces versus what
it merely describes.

Name the assumptions explicitly. Most adoption failures are a skill quietly
assuming a language, a framework, a directory layout, a CLI, or a team process
the user does not have.

### Step 2 — Separate the method from the furniture

Every skill is a mix of:

- **Method** — judgment, sequencing, the checks that make it work. The reason to adopt it.
- **Furniture** — the author's stack, naming, file paths, tool invocations, house conventions.
- **Padding** — restatements of things the model already knows.

Keep the method. Replace the furniture. Delete the padding, which is usually the
largest share.

The test for padding: would the model do this correctly without being told? If
yes, cut it. Skills are for what the model gets wrong or does inconsistently, not
for what it already does well.

### Step 3 — Fit it to the user

Gather only what changes the skill:

- their stack and tooling, where the skill names specific tools
- their conventions, where it names conventions
- where the outputs go and in what form
- what they will actually invoke it on
- anything in the original that is plainly wrong for them

Use what is already known about the user's projects and preferences before
asking. Ask only for the gaps.

### Step 4 — Rewrite, don't patch

Write the personalized version as one coherent document rather than the original
with edits layered on. A patched skill reads like two authors arguing.

Preserve the original's name unless it is genuinely misleading. Note provenance
in one line at the bottom — what it was adapted from and when — so a later audit
knows what to diff against.

### Step 5 — Set the budget

Before finishing, state a target length and hold it: **under 200 lines for a
single-purpose skill**, with a hard stop around 500. Record the number in the
provenance line. Every future audit is measured against it.

---

## Mode 2: Audit

An audit is not a rewrite. It is a pass with a strong bias toward removal.

### The five questions

1. **Does it still fire?** Has the user reached for this skill since the last audit? If they have been doing the task manually instead, the description is the problem, not the body.

2. **What did they correct?** Every time the user overrode the skill's output in real use is a signal. Corrections that recur are the only strong evidence that something should change.

3. **What is now wrong?** Tool flags move, versions change, conventions shift, the project moves on. Check anything the skill asserts about the outside world.

4. **What never got used?** Sections that have never fired in real work are the first candidates for deletion. Unused specificity is worse than absence — it is context cost plus a false sense of coverage.

5. **What did the model already know?** Re-run the padding test against the whole file. Skills accumulate explanation of the obvious over time.

### The removal rule

**An audit that only adds has failed.**

Each audit must either cut something or state plainly that nothing needed
cutting, with a reason. If the file grew, say by how much and what bought the
growth.

When adding and cutting in the same pass, present the cut first.

### Evidence bar for additions

Add a rule only when there is a concrete instance of the skill getting something
wrong. Not a hypothetical, not a completeness impulse, not "it would also be good
if."

- One occurrence → mention it, do not encode it
- Twice → add it, in the shortest form that works
- Anticipated but never observed → do not add it

Most one-off misses are better handled in the moment than written into a
permanent instruction.

### Simplicity checks

- Is any rule here restating another rule from a different angle? Merge them.
- Can any paragraph become a sentence?
- Do the examples earn their length, or would one do the work of three?
- Is there a list of options where naming the category would suffice?
- Would someone reading this cold know what to do, or only what to consider?

### Output of an audit

Keep it short. A table of changes, then the updated file.

| Change | Type | Why |
|---|---|---|
| Removed section X | cut | never fired in 3 months |
| Fixed flag in Y | correction | CLI changed in v2 |
| Added rule Z | addition | same mistake twice, Sept 4 and Sept 11 |

Then: lines before → lines after, against the budget.

State plainly if the skill is drifting toward needing a rewrite rather than
another audit — that is a real outcome, not a failure.

---

## Anti-bloat, stated once

Repeated auditing has a natural failure mode: each pass adds a rule, nothing is
ever removed, and after six months the skill is a 900-line document nobody reads
and the model skims.

The defenses, in order of importance:

1. A stated line budget, checked every audit
2. The removal rule — every pass cuts or justifies not cutting
3. The evidence bar — twice before it is written down
4. The padding test — if the model already does it right, it does not belong here

A skill that has gotten shorter and still works is a better outcome than one that
has gotten more thorough.

---

## Provenance line

At the bottom of every personalized skill:

```
<!-- adapted from <source> on <date> | budget: <n> lines | last audit: <date> -->
```

This is what makes the next audit cheap. Without it, every audit restarts from
zero.
