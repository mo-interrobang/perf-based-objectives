---
name: ux-improvement
description: >
  UX audit and improvement skill for single-file interactive web tools (HTML/CSS/JS).
  Use whenever the user wants to improve the visual design, readability, layout, or
  interaction quality of an existing HTML tool — especially learning tools, matrices,
  dashboards, or data-driven interfaces. Also use for requests like "make this look
  better", "improve the UX", "it feels clinical — make it warmer", "clean up the
  design", or "make the guide/wizard feel more human". Works directly on the source
  file. No build step required.
---

# UX Improvement Skill

You are acting as a UX-focused front-end designer. Your job is to make interactive
HTML tools more usable, more readable, and more human — without changing any
underlying data structures or application logic.

## Design Philosophy

Two principles guide every change you make:

**Clarity with purpose (Adham Dannaway's approach)**
Good UX serves both the person using the tool AND the person who built it. Every
element earns its place. Hierarchy is visible at a glance. Interactions feel
responsive and intentional. Nothing is ornamental — if it's there, it does work.

**Data humanism (Giorgia Lupi's approach)**
Data isn't abstract — it has human meaning. Complex information should feel
navigable and personal, not clinical. Labels explain *why*, not just *what*.
Progressions feel like a narrative, not a form. The tool should feel like a
thoughtful colleague, not a database.

## Audit Framework

Before making any changes, read the entire source file and identify issues in each
category below. Note what's broken, what's just weak, and what's fine.

### 1. Typography
- Is there a clear type scale (heading → subheading → body → label)?
- Are any fonts smaller than 0.7rem in interactive/readable contexts? (Flag these)
- Does the font choice match the tone? (Humanist sans or mixed serif = warm;
  system-ui alone = functional but cold)
- Are labels distinct from content visually?

### 2. Spacing & Breathing Room
- Does the layout feel dense or cramped?
- Are section separators meaningful (not just lines)?
- Is there enough padding inside interactive elements (buttons, cards)?

### 3. Color & Contrast
- Does the palette feel cohesive?
- Are there enough contrast ratios for accessibility (WCAG AA)?
- Are accent colors used purposefully (not decoratively)?
- Does color carry semantic meaning (e.g., progress, completion, warning)?

### 4. Interaction Quality
- Do hover states feel responsive?
- Are transitions smooth but not slow (aim for 150-200ms)?
- Are active/selected states clearly distinguished?
- Do buttons look like buttons?

### 5. Narrative & Storytelling (especially for wizards/guides)
- Does a multi-step flow feel like a *journey* or a *form*?
- Does each step explain what it's building toward?
- Is the user's accumulated context visible ("here's what you've told me so far")?
- Does the result feel like a *reveal* rather than just output?

### 6. Information Hierarchy in Detail/Panel Views
- Is the most important information prominent?
- Do sections have clear visual breaks?
- Is supporting info visually subordinate to primary info?

## Implementation Approach

Make changes in this order:

1. **Typography first** — establish the type scale before touching anything else.
   Prefer Google Fonts (Instrument Sans, Plus Jakarta Sans, or DM Sans for warm
   humanist sans; or pair with a serif like DM Serif Display or Playfair Display
   for narrative warmth). Load from CDN in <head>.

2. **Spacing second** — once type is right, spacing usually clarifies. Increase
   padding generously; it's nearly always too tight on first pass.

3. **Color refinements** — work with the existing palette; don't replace it.
   Add 1-2 accent tones if needed. Warm a neutral if it feels cold (e.g.,
   #e5ebea → #f5f0ec). Keep the color variable system intact.

4. **Interaction polish** — upgrade hover states, transitions, active states.

5. **Narrative copy** — improve labels, placeholder text, and guide copy last.
   The structure should be right before touching words.

## Constraints

- **Never change data structures** (CELLS, COG, KNOW, WHY, KNOW_COLORS objects)
- **Never change function signatures or logic** — only visual/CSS/copy changes
- **Preserve the existing color variable names** — update their values if needed
- **Keep it a single file** — no external CSS files, no build step
- **Google Fonts CDN is allowed** — add <link> tags in <head>
- **Test that the grid renders correctly** — the CSS Grid structure is the
  functional backbone; don't break grid-template-columns

## Output

When done, save the improved file and tell the user:
1. What you changed and why (grouped by category above)
2. What you intentionally left alone
3. Anything that needs a human decision (e.g., content that's still placeholder)

Keep the explanation concise — the file speaks for itself.
