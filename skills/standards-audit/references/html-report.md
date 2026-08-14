# HTML Report Template

**Do not use this file to author HTML/CSS.** [example-report.html](./example-report.html) is the literal file to copy — read it, copy it verbatim, and only edit content nodes (text, table rows, cards, counts) to substitute this audit's data. Never rename a class, restructure a section's DOM, add a CSS rule, or change a spacing/colour value that isn't in the example. This file explains *why* each section exists and *which data* fills it — it is not a spec to regenerate markup from.

If your output and the example ever disagree on a class name, a colour, or a pixel value, your output is wrong — go back to the example and copy it exactly.

## Design intent

A quiet, editorial one-page report — not a dashboard. It opens with a plain-language lede that states the finding, then walks numbered sections from most to least urgent. The reader should grasp the verdict — what was measured, what the target is, where the project sits — without scrolling, and be able to stop reading at any section boundary.

**Tables and ruled rows, not cards.** A report is tabular by nature: any set of homogeneous records (gaps, unknowns, live-verified properties, met controls, tech stack) renders as a table or hairline-ruled rows, never a card grid. The only bordered containers are functional: the tabbed case panel (the tabs need a boundary) and the incomplete-report callout (a warning, not data).

Neutral zinc palette. Colour is reserved for **status meaning** and appears nowhere decoratively: green = Met, amber = Partial, red = Missing, violet = Blocked, grey = Unknown. These hues were validated for adjacent colour-vision-deficiency separation, and every coloured mark also carries a glyph and a word — colour never works alone. Monospace only for the project name, the repo address, and step numbers.

## Every count from one source

The defect this rule exists for: a report once said "Three gaps could enable a breach" and listed four. **Every number in the report — lede, chart segments, legend, filter buttons, section counts, footer — must be derived from the same row list.** Before writing the file, count the rows once and reuse those counts everywhere. If a sentence states a count ("two gaps", "38 missing"), verify it against the rows it summarises. The example is generated from a single data structure precisely so its numbers cannot disagree; preserve that property.

## Structure (reading guide to the example's DOM)

1. **Header** — kicker, mono project `h1`, the **actual repo address as a link** (`.repo-link`), meta right (standards version from `VERSION` + date), chip row: type · solid `Level · {n} {name}` · one mono chip per facet · visibility (`Private repo` / `Public repo`).
2. **Lede** (`.lede`) — two or three sentences a non-engineer understands, in this order: what was measured (all N standards at level n and why that level), current state (meets X of the Y that apply), the sharpest risk, and the rough total effort. Bold only the load-bearing phrases.
3. **Incomplete note** (`.incomplete-note`) — only when a detected out-of-repo service was not reached: name the service, say how many rows are Unknown because of it, and note that Partial grades leaning on it may be off. Omit entirely when the audit is complete.
4. **Hero visuals** (`.hero-viz`, two panels per row):
   - **Coverage bar** — one horizontal stacked bar of the applicable-standard statuses (part-to-whole of one total; segments in Met → Partial → Missing → Blocked → Unknown order, 2px gaps, counts printed on segments wide enough to hold them, full counts always in the legend beneath). N/A is excluded from the bar (it isn't part of the applicable whole) and stated in the note line with the met-%.
   - **Measured against** (`.lscale`) — beside the coverage bar, a four-rung level scale (1 · Personal → 4 · Contracted) with **only the required rung filled dark** — the bar being measured against, visible without reading prose. It highlights a single rung; it never fills up to a level (that would plot maturity on the level scale). Beneath it: one short paragraph — required level = `max(impact, ambition)`, impact is the floor, and which of the four rating questions set it — then a `details.explainer` titled "Level and maturity are not the same thing" explaining that the level is the bar owed and maturity is how far the project currently reaches against it, with the coverage bar as the maturity picture.
   - **The coverage bar is the only maturity chart.** Do not add a second one — no gauges, no meters, and never maturity plotted on the level scale itself (the level names describe what is at stake, not achievement, so a chart implying a Critical project "is at Personal" is wrong by construction). Derived metrics like "levels short" or "average reach" restate the coverage bar in a more confusing unit; per-control distance already lives in the gaps table's Reaches column, where it has context.
   - **What was checked** (`.sources`, full-width row via `.hero-viz.single`) — one row per source (repo, source host, cloud/hosting): a green `✓ checked live` or amber `✕ not reached` pill, the source name, and one line of what that means. This is the prominent checked-vs-not-checked summary; inline caveats in rows still exist but this is where the reader learns it first.
   - **Full-width fallback:** if a `.hero-viz` row would have only one panel, add class `single` so the content spans the full width — never leave an empty column. The same rule applies to any two-column block elsewhere.
5. **01 · Priority** — *not* a re-listing of the worst table rows. Four labelled, hairline-ruled rows (`.priority-rows`) whose visual hierarchy explains itself: **The risk** (red label, the one shared outcome the priority gaps combine into, stated large), **Why together** (why these specific gaps are one failure mode), **The order** (a sequenced arrow list — what unblocks what), **The cost** (plain-language total). The section-sub explicitly says the full table is below. A committed secret with real values (SKILL.md's committed-secrets sweep) always belongs to this section's risk narrative, whatever the level arithmetic says. Omit the section when there are no priority gaps (nothing ≥2 levels below and no blockers).
6. **02 · Next steps** — ranked `<ol class="steps">`, 3–5 items, each with a concrete first action and a plain-language cost ("an hour", "half a day").
7. **03 · All gaps** — every Missing/Partial/Blocked control below the required level, **grouped by category** (`tbody.cat-group` with a `.cat-row` header carrying per-status counts). Above the table, a **filter bar** (`.fbtn` pills: All / Missing / Partial / Blocked, each with its count). Columns: Status (coloured badge with glyph + word) · Standard (name + one-line evidence; `verified live` mark on rows confirmed via a live connection) · Reaches (mini track of n segments + "x of n") · Effort (**Small / Medium / Large**, never S/M/L; the legend "Small — hours. Medium — a day or two. Large — several days." lives in the section-sub).
   - **Row expansion:** every row is a `button.gap-toggle` revealing a `tr.gap-detail` with a two-column `.now-vs` block — **Now** (the verbatim definition of the level actually reached, or "nothing in place"; for Blocked rows this column becomes **Blocked by** with the prerequisite and why implementing first would be worse) and **Required — level n** (the verbatim level-n definition from the section file, with "as level N" resolved to the actual text). Below it, a **Copy fix prompt** button whose `data-prompt` holds a complete instruction a coding agent can act on: the standard, the required definition, the current evidence, and "make the smallest set of changes that satisfies it".
8. **04 · Unverified** — a table (Standard · Category · What to check), one row per Unknown, and a section-sub naming how to enable the missing connection.
9. **04b · Verified against live infrastructure** — when the source host was reached, a table (Property · Finding · Note) of the repo's live properties: visibility, default branch + protection, secret scanning, Dependabot alerts, push protection, fork status, collaborator count, workflow count. Each row is `p-ok` (green ✓) or `p-bad` (red ✕) with a note distinguishing *checked-and-absent* from *not checked*. Extend the same pattern to cloud-side properties when the cloud was reached. Omit the section only when nothing was verified live.
10. **05 · In good standing** — Met rows as item/evidence lines with a category eyebrow. Show the first 5 and put the rest behind the "Show all N" toggle **only when there are more than 5**; with 5 or fewer, no toggle.
11. **06 · Technology detected** — one table (Slot · Detected · Evidence) over a **standardized slot taxonomy that is the same for every stack and level**. Groups and slots (`.cat-row` headers, same style as the gaps table):
      - **Application**: Language · Frontend framework · UI components · Backend runtime · API contract
      - **Data**: Database · Identity & auth · Migrations tooling
      - **Delivery**: Cloud / hosting · CI/CD · Infrastructure as code · Secrets management
      - **Quality**: Linter · Formatter · Unit tests · E2E tests
      - **Operations**: Error tracking · Logging · Feature flags
      - **External services & integrations**: one row per named service (ERP, payments, email, …)
      - **Source control & toolchain**: Source control · Package manager · AI code generation (when detected — the tool and evidence, stated factually; SKILL.md step 4 defines the detection signals)

    **This section reports what's there — only detected technologies appear.** Slots with nothing detected are simply not listed; the standard slot names and group order exist so reports stay comparable across projects, not to track absence — what's missing is the gaps table's job. Each row names the technology plainly (no audit commentary) with its mono **evidence path**. Below the table, one line naming the stack facets applied. No diagrams, no flows, no status colour anywhere in this section. **No vague entries** anywhere: name the actual technology/SDK/service precisely or omit it — never "email provider" or "address service".
12. **Deferred / N/A** (`section.plain`, side by side) — "Not yet expected at level n" (controls first defined above the required level; say "None" when empty rather than omitting, so the reader knows nothing is hidden) and "Not applicable" with each failed precondition stated.
13. **07 · The case for improvement** — the investment case as one card with Business/Engineering tabs (Business default). **Formatted prose, never a wall of text**: Business is 2–3 short paragraphs; Engineering is a short intro + ordered list of first actions + a closing why-now sentence. Content rules per SKILL.md.
14. **08 · Assumptions** — bullet list of what the findings are contingent on: how the level was inferred, which services weren't reached and what that clouds, what was sampled rather than exhaustively verified, anything taken on trust.
15. **Footer** — total standards · applicable at level n · standards version; right side: linked project name · date.

## No banner, no grade

There is deliberately no colour-coded banner and no letter grade. Severity is communicated by what leads: the lede's bolded risk sentence and, when present, section 01. Never invent a score — the coverage bar, the meter and prose carry the finding.

## Adapting to an evolving standard

The template is data-shaped, not audit-shaped. When the standard changes:

- New categories, controls or facets → clone the existing structural blocks (`tbody.cat-group`, `tr.gap-row` + `tr.gap-detail`, chips) with new content. Never invent new structures for new data of an existing kind.
- A new *status* would need a new colour token — that is a template change, made here in the reference (with the colour set re-validated for CVD separation), never improvised in one report.
- Sections whose data is empty follow their stated rule: omit (Priority, incomplete note, 04b when nothing verified) or state "None" (deferred). When omission empties one column of a two-column row, apply the full-width fallback.

## Styling

Copy the `<style>` block character-for-character; the token list below is only so you recognise it when reading:

- Zinc neutrals: `--fg` `--fg-secondary` `--muted` `--faint` `--border` `--border-light` `--border-dashed` `--bg` `--surface` `--secondary` (oklch, as in the example).
- Status: `--met #15803d` · `--partial #ca8a04` (text variant `--partial-text #854d0e`) · `--missing #c92a2a` · `--blocked #7048c8` · `--unknown` = muted grey; each with a pale `-bg` tint for badges. The chromatic four pass adjacent CVD ΔE and normal-vision floors as a set; amber's sub-3:1 surface contrast is relieved by the always-present labels and counts. Red belongs to Missing and the required-level marker only.
- No gradients, no animation beyond the row chevron, no web fonts, no shadows except the selected tab. Borders and whitespace do the structural work. Tabular-nums on figures. `text-wrap: pretty` on prose.
- `@media (max-width: 720px)` — two-column grids collapse to one; props grid to two; the gaps table scrolls inside `.table-scroll`, never the page.
- `@media print` — hide buttons, filters, tabs and hints; force-reveal all gap details and both case panels so nothing is lost on paper.

## Interactions

Copy the `<script>` block verbatim — one IIFE, null-checked, and the report must be fully readable with JS disabled (hidden content is revealed by print, never required for the verdict). Four handlers: gap-row expansion (aria-expanded + hidden), copy-fix-prompt (clipboard API with execCommand fallback and a "Copied" flash), status filter (hides rows and any category group left empty; collapses hidden rows' details), and case tabs (Business default).

## Content rules

- Detail rows mirror the markdown data exactly — same rows, same grouping; no editorialising in one output that isn't in the other.
- Quotes in the now/required columns are verbatim from the section files ("as level N" resolved); do not paraphrase level definitions.
- Fix prompts are generated from the same fields as the row (name, category, required definition, evidence) so they can never contradict it.
- Investment-case pitches: Business ≤ 150 words per SKILL.md, no tool names; Engineering names controls, tools and first actions.
