---
name: generate-design-directions
description: Generate meaningfully different visual design directions for an application or interface. Use when exploring a new UI, redesigning an existing product, creating alternative concepts, or when the user asks for different looks, styles, directions, themes, or visual approaches. Produces distinct design concepts by varying a small set of independent design axes rather than making superficial color or spacing changes.
---

# Design Direction Generator

Generate distinct visual directions for an application before implementation.

The goal is not several slightly different versions of the same design. Each
direction should feel like it came from a different designer with a different
visual thesis, while still serving the same product and users.

## Ground the directions in the subject first

Before picking any axis, identify what the product actually is: its domain, its
users, the materials and vocabulary of the world it belongs to. This is where
distinctive choices come from. A scheduling tool for machinists and a scheduling
tool for florists should diverge before a single axis is chosen — same product
category, entirely different visual vernacular available.

If the brief does not say, propose a concrete subject, audience, and primary job,
and confirm before generating.

Directions that could be swapped onto any product in the category are themes, not
directions.

## Core method

1. Identify the product, primary user, primary interaction, and subject-matter vernacular.
2. Choose 2–4 design axes that will define each direction. Not all of them.
3. Deliberately choose contrasting combinations between directions.
4. Give each direction a short memorable name.
5. State the visual thesis in one or two sentences.
6. Translate the thesis into concrete interface decisions.
7. Keep product functionality constant unless the concept specifically requires a structural change.

A strong direction has one dominant idea supported by two or three secondary
choices. Avoid changing every variable at once.

## Generate more than you present

The first two or three ideas in any ideation are the accessible ones — category
conventions and things already seen. Generate **six** directions internally,
then discard the two that most resemble what any similar product would get, and
present the rest. Note in one line what was discarded and why, so the user can
ask for it back.

---

## Design axes

Pick 2–4 per direction. Listing a value on every axis produces mush; the unpicked
axes stay at whatever the dominant idea implies.

**Tone** — how the interface emotionally presents itself.
playful · austere · warm · clinical · quiet · bold · serious · luxurious ·
utilitarian · restrained

**Density** — how much information occupies the visible interface.
Spectrum: spacious/minimal → balanced → dense/maximal
Consider whitespace, row height, metadata visibility, simultaneous actions,
progressive disclosure.

**Layout paradigm** — the fundamental spatial model.
card grid · editorial column · single-focus workspace · dashboard/panels ·
split view · master-detail · timeline · feed · canvas · table/ledger · kanban ·
calendar/agenda · asymmetric composition

Changing the layout paradigm should noticeably change how the product feels to
use. It is not rearranging cards.

**Color strategy** — how color participates.
monochrome + one accent · grayscale only · muted/tonal · duotone · multicolor ·
semantic · high contrast · dark-first · warm neutrals · saturated accent blocks

Specify the *role* of color, not hex values. "Mostly grayscale, with color only
for urgency, selection, and primary actions."

**Typography voice** — the personality carried by type.
geometric sans · grotesk · humanist sans · humanist serif · editorial serif ·
mono/technical · expressive display · condensed · oversized · typewriter/archival

Consider heading/body contrast, scale, weight, capitalization, tracking, and how
numbers and metadata are set.

**Structure** — how visibly the interface exposes its organization.
Spectrum: grid-strict/ruled → balanced → organic/whitespace-driven
Ruled: visible borders, dividers, table structures, hard alignment, labeled regions.
Organic: whitespace, proximity grouping, fewer containers, less visible chrome.

**Imagery** — visual content beyond interface elements.
photography · illustration · diagrams · abstract geometry · texture ·
data visualization · type-only · none

For application interfaces, explicitly consider whether imagery should be absent.

**Formality / era** — the visual tradition being referenced.
contemporary product UI · corporate · editorial · handmade · archival ·
scientific · industrial · retro computing · brutalist · Swiss/International ·
modernist · terminal-inspired · print-publication · luxury

Use these as influences, not costumes.

### Optional secondary axes

Use only when they materially change the design.

- **Geometry** — sharp · slightly rounded · heavily rounded · pill-based · irregular
- **Depth** — flat · borders only · subtle elevation · layered surfaces · translucent
- **Motion** — static · restrained transitions · responsive micro-interactions · playful
- **Navigation model** — persistent sidebar · compact rail · top nav · command palette · contextual · mostly hidden
- **Interaction character** — mouse-first · keyboard-first · touch-first · command-driven · direct manipulation · form-driven

---

## Defaults to avoid

Distinct axes still produce generic-looking output if every direction arrives
carrying the same chrome. These are the current tells of generated design — all
legitimate for *some* brief, but they show up regardless of subject, which is
what makes them defaults rather than choices:

- ALL-CAPS tracked-out eyebrow labels above headings
- meta strings joined with middle dots (`A · B · C`)
- `→` appended to link and button text
- labels built as `WORD — fragment` with a spaced em dash
- tinted near-black (#0B0B0B, #111) standing in for black
- monospace used for small data labels regardless of whether the data is technical
- one border-radius on everything regardless of hierarchy
- the same soft grey shadow (rgba(0,0,0,.1)) under every card
- warm cream background (~#F4F1EA) + high-contrast serif + terracotta accent (~#D97757)
- near-black background with a single acid-green or vermilion accent
- accenting a single word in a headline with color, italic, or weight
- numbered markers (01/02/03) on content that is not actually a sequence
- fade-and-slide-up entrances on every section, hover transitions on every card

Where the brief specifies one of these, follow the brief — its words always win.
Where the brief leaves an axis free, don't spend that freedom on a default.

---

## Product constraints

Visual exploration must respect the underlying product. Before generating,
identify:

- primary job of the application
- primary screen and most frequent action
- important information hierarchy
- device targets
- constraints that stay fixed

Do not redesign the product to make the visual concepts look different.

### Redesigning an existing application

Separate what is fixed from what is open.

Usually fixed: information architecture, product terminology, data model,
navigation destinations, workflows, required controls.

Usually open: hierarchy, layout, density, typography, color, containers,
interaction and navigation presentation.

Preserve product logic unless explicitly asked to reconsider it.

---

## Output format

Default to **3 presented directions** (from 6 generated).

### [Direction Name]

Short, memorable, descriptive — "Quiet Ledger", "Paper Agenda", "Modern
Terminal", "Precision Grid". Never "Design A", "Modern Version", "Concept 2".

**Thesis** — one or two sentences on the central visual idea.

**Design DNA** — only the 2–4 axes that materially define this direction.

**Interface consequences** — how the choices change the actual interface:
page composition, navigation, rows/cards, borders, spacing, typography, color,
buttons, forms, status indicators, empty states. Enough that another designer or
coding agent could build it without asking what a given screen should look like.

**Signature detail** — one recognizable element that makes the concept memorable.
"Dates appear as oversized margin markers anchoring each day."

### Comparison

After the directions, a short table naming what is structurally different:

| Direction | Dominant idea | Layout | Density | Structure |
|---|---|---|---|---|
| Quiet Ledger | precision | ledger | dense | ruled |
| Editorial Agenda | reading | column | spacious | whitespace |
| Command Center | awareness | dashboard | dense | panels |

Do not rank the directions unless asked.

---

## Worked contrast

Bad — themes, not directions:

- Option A: blue cards with rounded corners
- Option B: purple cards with rounded corners
- Option C: green cards with rounded corners

Better — same application, different visual thesis:

- **Quiet Ledger** — quiet · dense · ledger layout · monochrome + one accent · strict ruled structure
- **Editorial Agenda** — warm · spacious · editorial column · serif + sans · whitespace-driven
- **Personal Command Center** — bold · dense · dashboard panels · semantic multicolor · geometric sans

---

## Diversity check

Before presenting, compare the set:

- Would someone recognize these as different concepts **in grayscale**?
- Would they still feel different if all used the same brand color?
- Do they use different hierarchy and composition strategies, not just different skins?
- Is there a clear thesis behind each, statable in one sentence?
- Did I change more than border radius, color, and shadow?
- **Did any direction reach for something on the defaults-to-avoid list?**
- **Could these be lifted onto a different product in the same category unchanged?** If yes, the subject matter isn't doing any work.

If the set fails any of these, increase the conceptual distance and regenerate.

---

## Principle

Do not generate themes. Generate design systems with opinions.

A useful direction lets another designer or coding agent read the description and
know how the entire interface should behave visually.
