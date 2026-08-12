# HTML Report Template

**Do not use this file to author HTML/CSS.** [example-report.html](./example-report.html) is the literal file to copy — read it, copy it verbatim, and only edit content nodes (text, table rows, card lists) to substitute this audit's data. Never rename a class, restructure a section's DOM, add a CSS rule, or change a spacing/colour value that isn't in the example. This file only explains *why* each section exists and *which data* fills it, so you substitute the right content in the right place — it is not a spec to regenerate markup from.

If your output and the example ever disagree on a class name, a colour, or a pixel value, your output is wrong — go back to the example and copy it exactly.

## Design intent

A quiet, editorial one-page report — not a dashboard. No banner colour, no grade, no gauge. It opens with a plain-language lede that states the finding, then walks numbered sections from most to least urgent. The reader should grasp the verdict in the first two sentences and be able to stop reading at any section boundary.

Neutral zinc palette, generous whitespace, one accent (red) reserved exclusively for breach-risk. Monospace only for the project name, effort codes, and step numbers.

## Structure

The skeleton below is a reading guide to the example's DOM, not markup to type out — copy the actual tags, classes, and nesting from [example-report.html](./example-report.html) instead of reconstructing them from the placeholders shown here.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Standards Audit — {project}</title>
  <style>/* see Styling */</style>
</head>
<body>
<div class="wrap">

  <header class="report-header">      <!-- kicker, monospace {project} h1, meta (standards version + date), chip row -->
    <!-- chips: {type} · solid "Target stage · {stage}" · one mono chip per detected facet -->
  </header>

  <div class="lede-section">          <!-- plain-language finding + stat strip -->
    <p class="lede">…{met} of the {applicable} standards … {plain risk sentence} … {rough effort}…</p>
    <!-- completeness: when GitHub and/or cloud/hosting weren't reached, add a plain note here
         stating the report is INCOMPLETE, which services weren't reached, and that their Unknown
         rows are unverified. Omit when every detected out-of-repo service was connected. -->
    <div class="stats">               <!-- Met / Partial / Missing / Unknown; Unknown num is .dim -->
    <div class="stats-note">…met-% of applicable · N not yet expected · N N/A…</div>
  </div>

  <section>01 · Priority</section>     <!-- red h2 stating the worst finding; 2-col grid of breach items. OMIT whole section when no breach-risk gaps -->
  <section>02 · Next steps</section>   <!-- ranked <ol class="steps">, 3–5 items, each with a rough cost -->
  <section>03 · All gaps</section>     <!-- .table-card table: Status | Standard(+evidence+category) | Risk | Effort -->
  <section>04 · Unverified</section>   <!-- 3-col grid of dashed .unknown-card, one per Unknown item -->
  <!-- Optional 04b · Verified against live infra: 3-col grid of solid .verified-card, one per row that a live connection confirmed. OMIT when no rows were verified live -->
  <section>05 · In good standing</section> <!-- Met rows, first 5 shown, rest .extra behind a toggle -->

  <section class="plain">              <!-- deferred + N/A, de-emphasised, side by side -->
    <!-- "Not yet expected at {stage}" with short/more spans behind a toggle · "Not applicable" -->
  </section>

  <section>06 · Why now</section>      <!-- .case-card with Business/Engineering tabs (Business default). OMIT when no actionable gaps -->

  <footer>…{total} standards evaluated · {applicable} apply at {stage} · standards v{VERSION} … {project} · {date}</footer>

</div>
<script>/* see Interactions */</script>
</body>
</html>
```

## No banner, no grade

There is deliberately no colour-coded banner and no letter/level grade. Severity is communicated by *what leads*: if there is a breach-risk gap, section 01 leads with it in red; if not, omit section 01 entirely and let the lede carry a calmer finding. Never invent a score — state stage, coverage, and the finding in prose.

## Section rules

- **Lede** — one or two sentences a non-engineer understands: how many standards are met of those that apply at the target stage, the sharpest risk, and the rough total effort. The stat strip and note must reconcile with the markdown coverage line.
- **01 · Priority** — only breach-risk gaps (`Causes breach` at or below target stage). Red `h2` naming the worst outcome; grid items each show name, one-line evidence, and a `Missing · {Category} · Effort {S/M/L}` tag. Omit the section when there are none.
- **02 · Next steps** — the ranked recommendation list from step 5 (risk severity desc, then effort asc), 3–5 items, each with a plain-language cost ("hours", "half a day").
- **03 · All gaps** — every `Missing`/`Partial` row at or below target stage, same rows and order as the markdown report. Status badge (Missing = outlined dark, Partial = filled grey), evidence line, category, risk with a dot (red for breach), effort in mono.
- **04 · Unverified** — one dashed card per `Unknown` row with the exact out-of-repo check.
- **04b · Verified against live infrastructure** (optional) — only when a live connection (step 2) turned previously-`Unknown` rows into `Met`/`Partial`. One solid `.verified-card` per such row, using the **same vertical stack** as `.unknown-card` (name over one-line finding); a `✓` is appended to the name via CSS. These rows are already folded into the counts above — this section just shows the reader what was checked live. Do **not** lay the card out as name-beside-detail columns. Omit the whole section when nothing was verified live.
- **05 · In good standing** — `Met` rows as two-column item/evidence lines; show the first 5, hide the rest as `.extra` behind a "Show all N" toggle.
- **Deferred / N/A** (`section.plain`) — de-emphasised. "Not yet expected at {stage}" lists Prod-only items with a short→more toggle; "Not applicable" states each row and its reason.
- **06 · Why now** — the investment case (SKILL.md step 8) as one card with Business/Engineering tabs; Business selected by default. Omit when there are no actionable gaps.

## Styling

Do not retype the `<style>` block from memory or from the token list below — copy the example's `<style>` block character-for-character. The list below is only so you recognize the tokens when reading the example; it is not a spec to write CSS from.

- `--fg: oklch(0.141 0.005 285.823)` · `--fg-secondary: oklch(0.442 0.017 285.786)` · `--muted: oklch(0.552 0.016 285.938)` · `--faint: oklch(0.705 0.015 286.067)`
- `--border: oklch(0.92 0.004 286.32)` · `--border-light: oklch(0.94 0.004 286.32)` · `--border-dashed: oklch(0.871 0.006 286.286)`
- `--bg: oklch(0.985 0 0)` · `--surface: oklch(1 0 0)` · `--secondary: oklch(0.967 0.001 286.375)`
- `--destructive: oklch(0.577 0.245 27.325)` — red, used **only** for breach-risk (priority h2, breach risk dots/labels)
- `--mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`; body font `ui-sans-serif, system-ui, -apple-system, "Segoe UI", …`

Rules: no gradients, no animation, no web fonts, no shadows except the subtle one on the selected tab. Borders and whitespace do the structural work. Tabular-nums on stat figures. `text-wrap: pretty` on prose.

- `@media (max-width: 720px)` — stats collapse to 2 columns; priority/unknown/deferred grids collapse to 1.
- `@media print` — hide buttons and tabs; force-reveal `.met-row.extra`, the deferred "more" text, and both case panels so nothing is lost on paper.

## Interactions

Copy the `<script>` block from the example verbatim — do not rewrite the toggle/tab logic. It is one IIFE, wrapped in null-checks; the report must be fully readable with JS disabled (progressive enhancement — hidden content is revealed by print/JS, never required to understand the verdict). Three handlers:

- **Met toggle** — toggles `.open` on the met list (CSS reveals `.extra` rows); button text swaps "Show all N" ⇄ "Show fewer"; keep `aria-expanded` in sync.
- **Deferred toggle** — toggles `.deferred-open` (swaps short/more spans); "Show more" ⇄ "Show fewer".
- **Case tabs** — Business/Engineering; toggle `aria-selected` and each panel's `hidden`. Business default.

## Content rules

- Detail tables mirror the markdown report exactly — same rows, same ordering; no editorialising in one output that isn't in the other.
- Every count in the lede, stat strip, and note must reconcile with the markdown coverage line.
- The met list shows the first 5; the rest go behind the toggle, not dropped.
- Investment-case pitches follow SKILL.md step 8 (Business ≤ 150 words, no tool names; Engineering names rows/tools/first action).
