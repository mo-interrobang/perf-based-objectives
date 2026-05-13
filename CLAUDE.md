# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A self-contained single-file web tool that helps subject matter experts (SMEs) and instructional designers choose performance-based learning objectives using Bloom's Taxonomy, a Knowledge Domain dimension, and a custom Social/Change Dimension framework.

No build system, no dependencies, no Node.js required. The entire application is `index.html`. Open it directly in a browser.

## Architecture

Everything lives in `index.html`: HTML structure, CSS (in `<style>`), and JavaScript (in `<script>`). There is no bundler, framework, or external dependency.

### Grid structure

The matrix has three axes:

| Axis | Direction | Values |
|------|-----------|--------|
| **Knowledge Domain** (rows, left label) | Top = Abstract → Bottom = Concrete | Metacognitive, Procedural, Conceptual, Factual |
| **Cognitive Process** (columns, bottom label) | Left = Lower Order → Right = Higher Order | Remember, Understand, Apply, Analyze, Evaluate, Create |
| **Social/Change Dimensions** (top header, 1-to-1 with columns) | Personal → Social competence | Self Regard, Self Awareness, Self Control, Social Perception, Social Effectiveness, Social Design |

### Data model

All content is in the `CELLS` object, keyed by `"cognitiveId-knowledgeId"` (e.g., `"remember-factual"`). Each entry has:

```js
{
  competency: 'Optimism',        // social competency label shown on cell face (or null)
  verbs: ['list', 'name', ...],  // action verbs shown on cell face (first 3) and in detail panel
  objectives: ['...'],           // example objectives shown in detail panel
  strategies: ['...'],           // learning strategies shown in detail panel
}
```

`COG` (array) defines the 6 cognitive levels and their paired social dimension label/description. `KNOW` (array) defines the 4 knowledge types and their CSS classes. Both drive the grid render loop.

`WHY` (object, keyed same as `CELLS`) holds plain-English explanations of why a verb maps to that cell — used in the guide result step.

`KNOW_COLORS` maps each knowledge type id to `{ bg, text }` hex values for result badge styling in the guide.

### Rendering

`index.html` builds the grid entirely in JavaScript by appending elements to `#main-grid` (a CSS Grid with `grid-template-columns: 148px repeat(6, 1fr)`). Render order: social header row → 4 knowledge rows (label + 6 cells each) → cognitive process bottom label row.

### Detail panel

Clicking a cell calls `openPanel()`, which injects HTML into `#panel-inner` and adds class `open` to `#detail-panel` (CSS transition slides it in at 348px wide). `closePanel()` reverses this.

### Coaching guide (SME entry flow)

The guide overlay (`#guide-overlay` / `#guide-card`) launches automatically on first load and is re-accessible via the "Guide me ›" header button. It walks SMEs through a 6-step state machine before landing them on the right cell:

| Step | `guideState.step` | Purpose |
|------|-------------------|---------|
| 1 | `goal` | Free-text: business goal or performance gap |
| 2 | `terminal` | Reframe as a learner action (terminal objective) |
| 3 | `steps` | Break into 2–5 enabling steps (dynamic list) |
| 4 | `selectStep` | Pick which step to work on (skipped if only 1 step) |
| 5 | `verb` | Enter a verb; `findVerb()` does exact then prefix match |
| 6 | `result` | Colored badge (cog × knowledge) + WHY explanation + "Explore this cell →" |

State lives in `guideState` object. `renderGuide()` dispatches to step-specific HTML injection. Back navigation and "Skip to grid" are available at every step. A context trail (`.gc-trail`) shows the accumulated goal and terminal objective from step 2 onward.

`findVerb(raw)` searches all `CELLS` verbs: exact match first, then prefix match (minimum 4 chars). Returns `{ key, matchedVerb }` or `null`.

`applyResult(key)` closes the guide, scrolls to the cell, fires a pulse animation (`guide-highlight`), and opens the detail panel.

### Color palette

Defined as CSS custom properties in `:root`:

```css
--ebony: #45503b   --ebony-dk: #2e3628
--grey: #747572    --grey-dk: #5a5958
--lilac: #b098a4   --lilac-dk: #8a7590
--alabaster: #dbd9db
--platinum: #e5ebea
```

Cell background by knowledge row class: `cr-metacognitive` (ebony-dk), `cr-procedural` (ebony), `cr-conceptual` (lilac), `cr-factual` (alabaster). Label column uses matching `kl-*` classes.

## Reference files (do not edit)

`blooms-2.fla` / `blooms-2.html` / `blooms-2.js` — the original Adobe Animate source. Used as reference for content, layout intent, and social competency labels per cell. The `.js` file is compiled canvas output; all human-readable text is in `cjs.Text(...)` calls. The `.psd` and `.pptx` files are design references.

## Adding content to a cell

Edit the matching key in the `CELLS` object in `index.html`. Cells with at least one verb automatically lose the `no-content` opacity. The `objectives` and `strategies` arrays are partially filled; `Knowledge Checks & Assessments` and `Performance Measures & OKRs` sections in the panel are currently `pending` placeholders — add properties to the cell schema and update `openPanel()` to render them when ready.
