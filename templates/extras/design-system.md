# Design System: [PROJECT NAME]

**Last updated:** [YYYY-MM-DD]
**Owner:** [NAME / TEAM]
**Design tool:** [Figma / None — code is the source of truth]
**CSS framework:** [Tailwind CSS / CSS Modules / Styled Components / etc.]

This document is the visual language reference for [Project Name]. All new UI work should reference this system before introducing new colors, fonts, or component patterns.

---

## Brand Identity

**Product name:** [PROJECT NAME]

**Tagline:** [Short, memorable phrase that captures what the product does or stands for.]

**Brand tone:**
- [Adjective — e.g., Confident but approachable]
- [Adjective — e.g., Technical without being cold]
- [Adjective — e.g., Direct and efficient — no fluff]

**What we sound like:** [One to two sentences describing the voice in copy, UI labels, and error messages. E.g., "We write like a knowledgeable colleague, not a corporate manual. Short sentences. Active voice. We don't over-explain."]

**What we don't sound like:** [E.g., "We avoid startup clichés ('revolutionize', 'disrupt'), exclamation points in errors, and passive corporate voice."]

---

## Color Palette

### Primary

| Name | Hex | Usage |
|------|-----|-------|
| Primary | `[#1A56DB]` | Main CTAs, links, active states |
| Primary Dark | `[#1742B0]` | Hover state for primary |
| Primary Light | `[#EBF0FF]` | Backgrounds, badges, subtle highlights |

### Secondary

| Name | Hex | Usage |
|------|-----|-------|
| Secondary | `[#7E3AF2]` | [Accents, secondary actions, badges] |
| Secondary Dark | `[#6429CE]` | [Hover state for secondary] |
| Secondary Light | `[#F5F3FF]` | [Subtle secondary backgrounds] |

### Neutral

| Name | Hex | Usage |
|------|-----|-------|
| Gray 900 | `[#111827]` | Body text, headings |
| Gray 700 | `[#374151]` | Secondary text, labels |
| Gray 500 | `[#6B7280]` | Placeholder text, captions |
| Gray 300 | `[#D1D5DB]` | Borders, dividers |
| Gray 100 | `[#F3F4F6]` | Page backgrounds, table stripes |
| White | `[#FFFFFF]` | Card backgrounds, modal backgrounds |

### Semantic

| Name | Hex | Usage |
|------|-----|-------|
| Success | `[#057A55]` | Confirmations, success states |
| Success Light | `[#DEF7EC]` | Success backgrounds, alert fills |
| Warning | `[#C27803]` | Warnings, caution states |
| Warning Light | `[#FDF6B2]` | Warning backgrounds |
| Error | `[#C81E1E]` | Error messages, destructive actions |
| Error Light | `[#FDE8E8]` | Error backgrounds |
| Info | `[#1C64F2]` | Informational messages |
| Info Light | `[#E1EFFE]` | Info backgrounds |

### Accent (optional)

| Name | Hex | Usage |
|------|-----|-------|
| Accent | `[#FF5A1F]` | [E.g., highlights, new feature callouts] |

---

## Typography

### Font families

| Role | Family | Fallback | Source |
|------|--------|----------|--------|
| Heading | `[Inter]` | `sans-serif` | [Google Fonts / Bundled / System] |
| Body | `[Inter]` | `sans-serif` | [Google Fonts / Bundled / System] |
| Mono | `[JetBrains Mono]` | `monospace` | [Google Fonts / Bundled / System] |

### Type scale

| Name | Size | Line height | Weight | Usage |
|------|------|-------------|--------|-------|
| Display | `[3rem / 48px]` | `[1.2]` | `[700]` | [Hero headlines only] |
| H1 | `[2rem / 32px]` | `[1.25]` | `[700]` | [Page titles] |
| H2 | `[1.5rem / 24px]` | `[1.3]` | `[600]` | [Section headings] |
| H3 | `[1.25rem / 20px]` | `[1.4]` | `[600]` | [Card titles, subsections] |
| H4 | `[1rem / 16px]` | `[1.5]` | `[600]` | [Labels, small headings] |
| Body LG | `[1.125rem / 18px]` | `[1.6]` | `[400]` | [Long-form text, onboarding] |
| Body | `[1rem / 16px]` | `[1.5]` | `[400]` | [Default body text] |
| Body SM | `[0.875rem / 14px]` | `[1.5]` | `[400]` | [Secondary text, table rows] |
| Caption | `[0.75rem / 12px]` | `[1.4]` | `[400]` | [Timestamps, footnotes] |
| Mono | `[0.875rem / 14px]` | `[1.6]` | `[400]` | [Code, IDs, keys] |

### Type usage rules

- [Limit heading levels to three per page. Do not skip levels (H1 → H3).]
- [Use Body for most UI text. Use Body SM for tables and secondary information.]
- [Use Mono for any value the user might copy — IDs, API keys, code snippets.]
- [Do not use more than two font weights on the same page outside of headings.]

---

## Spacing and Layout

### Spacing scale

Using a base unit of `4px`. Tailwind class in brackets.

| Token | Value | Tailwind |
|-------|-------|----------|
| 1 | 4px | `p-1` |
| 2 | 8px | `p-2` |
| 3 | 12px | `p-3` |
| 4 | 16px | `p-4` |
| 5 | 20px | `p-5` |
| 6 | 24px | `p-6` |
| 8 | 32px | `p-8` |
| 10 | 40px | `p-10` |
| 12 | 48px | `p-12` |
| 16 | 64px | `p-16` |
| 20 | 80px | `p-20` |
| 24 | 96px | `p-24` |

### Grid and layout

| Breakpoint | Min width | Max content width | Columns |
|------------|-----------|------------------|---------|
| Mobile | 0px | 100% | 4 |
| Tablet (md) | 768px | 720px | 8 |
| Desktop (lg) | 1024px | 960px | 12 |
| Wide (xl) | 1280px | 1200px | 12 |

**Page padding:** `16px` mobile / `24px` tablet / `32px` desktop.

**Section spacing:** `48px` mobile / `80px` desktop between major page sections.

**Card padding:** `16px` mobile / `24px` desktop.

---

## Component Patterns

### Buttons

| Variant | Use case | Background | Text | Border |
|---------|----------|-----------|------|--------|
| Primary | Main CTA | Primary (`[#1A56DB]`) | White | None |
| Secondary | Secondary action | White | Primary | Primary, 1px |
| Danger | Destructive action | Error (`[#C81E1E]`) | White | None |
| Ghost | Tertiary action | Transparent | Gray 700 | None |
| Link | Inline action | — | Primary | None (underline on hover) |

**Button sizes:** SM (`h-8, px-3, text-sm`), MD (`h-10, px-4, text-sm`) default, LG (`h-12, px-6, text-base`).

**States:** All buttons have hover, focus (ring), active, disabled, and loading states.

**Rules:**
- [One primary button per section — do not stack two primary buttons side by side.]
- [Use Danger variant only for irreversible destructive actions (delete, remove). Always confirm before executing.]
- [Loading state shows a spinner and disables the button. Label stays the same.]

---

### Cards

- Background: White. Border: Gray 300, 1px. Border radius: `[8px / rounded-lg]`. Shadow: `[shadow-sm]`.
- Padding: `[16px]` default / `[24px]` for prominent cards.
- Cards are not interactive by default. If clickable, use hover state: `[hover:shadow-md, cursor-pointer]`.
- Do not nest cards more than one level deep.

---

### Forms

- Label: Body SM, Gray 700, 4px below label to input.
- Input: `[h-10, px-3, border border-gray-300, rounded-md, text-gray-900, bg-white]`. Focus: `[ring-2, ring-primary]`.
- Helper text: Caption, Gray 500, 4px above input bottom.
- Error message: Caption, Error color (`[#C81E1E]`), below input. Always include an icon.
- Required fields: Marked with a red asterisk (`*`) after the label.
- Field width: Match the expected length of the value (e.g., phone number fields are shorter than address fields). Default to full-width within the form container.

---

### Navigation

**Top navigation:**
- Height: `[64px]`. Background: White. Border-bottom: Gray 300, 1px.
- Logo left-aligned. Primary nav links center or right. User avatar/menu far right.
- Active nav item: Primary color text and/or underline. Font weight: 600.

**Sidebar navigation (if applicable):**
- Width: `[240px]`. Background: Gray 900 or White (pick one, be consistent).
- Nav items: Body SM, `[h-9, px-3, rounded-md]`. Active: Primary Light background, Primary text.

**Mobile navigation:**
- Collapse to hamburger menu below `[768px]`. Drawer slides from left.

---

## Iconography

**Icon set:** [Heroicons / Lucide / Phosphor / Custom]

**Sizes:** SM `[16px]`, MD `[20px]` default, LG `[24px]`.

**Usage rules:**
- [Icons always accompany text labels in buttons unless space is critically limited.]
- [Standalone icon buttons must have a tooltip or `aria-label`.]
- [Do not mix icon libraries on the same page.]
- [Use filled icons for active/selected states and outline icons for default states (if the icon set supports both).]

---

## Imagery Style

**Photography:** [E.g., Real product screenshots, not staged stock photos. Light, clean backgrounds. No generic business imagery.]

**Illustrations:** [E.g., Flat, geometric, limited color palette matching brand. Used only for empty states and onboarding.]

**Empty states:** [Each empty state includes an illustration (optional), a heading explaining what's empty, and a CTA to take the first action.]

**Avatars:** [Circular, `[32px]` default. Fallback: initials in a colored circle based on name hash.]

---

---

## Prompt: How to generate this document

```
I need to document the design system for [PROJECT NAME] based on the existing UI.

Here is the project context:
- CSS framework: [e.g., Tailwind CSS v3]
- UI component library (if any): [e.g., shadcn/ui, Radix, Headless UI, or none]
- Design tool (if any): [e.g., Figma, or none — code is source of truth]
- Brand personality: [describe in a few adjectives]
- Target users: [describe]

To help you generate this, I'll provide one or more of the following:
[PASTE ONE OR MORE OF THESE]
- Screenshots of the current UI (attach images)
- Tailwind config file contents
- Main CSS or globals file
- A list of the main colors and fonts already in use
- A description of the overall visual style

Please generate a complete design system document with these sections:
1. Brand identity — name, tagline, tone (3 adjectives), voice description, what we don't sound like
2. Color palette — primary, secondary, neutral, semantic, accent tables with hex values and usage descriptions
3. Typography — font families, full type scale table (size, line height, weight, usage), and 3-4 usage rules
4. Spacing and layout — spacing scale table, breakpoints, grid, page/section/card padding
5. Component patterns — buttons (variants, sizes, states, rules), cards, forms, navigation
6. Iconography — icon set, sizes, usage rules
7. Imagery style — photography, illustrations, empty states, avatars

Infer hex values and sizes from whatever I provide. Flag anything you're unsure about so I can verify.
```

---

## Prompt: How to update this document

```
I have an existing design system document for [PROJECT NAME] that needs to be updated to reflect UI changes.

Here is the current design system:
[PASTE DOCUMENT HERE]

Here is what has changed in the UI:
[Describe changes — e.g., we changed the primary color, added a new button variant, updated the font, added a sidebar, changed card styles, added a new icon set]

To help you verify the current state, I'll provide:
[PASTE ANY OF THESE]
- Updated screenshots
- Updated Tailwind config
- New component code

Please:
1. Update all color, typography, spacing, or component entries that have changed
2. Add new component patterns for any new UI patterns not previously documented
3. Remove or mark as deprecated any patterns that are no longer used
4. Flag any inconsistencies — e.g., two different card border radiuses in use, mixed icon libraries
5. Update the "Last updated" date

Keep the same document structure. Do not rewrite sections that haven't changed.
```
