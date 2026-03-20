# Design Reference Integration

The `frontend-design` skill has detailed reference docs that should be selectively included in Gemini prompts based on the task.

## When to Include Each Reference

| Task involves | Include this reference |
|---|---|
| Font choices, type scale, headings | typography |
| Color palette, dark mode, theming | color-and-contrast |
| Layout, grid, spacing, padding | spatial-design |
| Animations, transitions, hover effects | motion-design |
| Forms, buttons, modals, loading states | interaction-design |

## How to Include

Read the relevant reference file from `~/.agents/skills/frontend-design/reference/` and include the **key rules** section (not the full file). Extract 10-15 most relevant lines.

Example for a dashboard mockup (needs spatial + typography):

```
## Typography Guidelines (from project design system)

- Use a modular type scale: 12 / 14 / 16 / 20 / 24 / 32 / 40 / 48px
- Body text: 16px/1.5 line-height minimum
- Headings: use weight contrast (600-700) rather than just size
- Don't use more than 2 font families
- Use fluid sizing: clamp(1rem, 0.5rem + 1vw, 1.25rem)

## Spatial Guidelines (from project design system)

- Base unit: 4px. Scale: 4, 8, 12, 16, 24, 32, 48, 64
- Group related items with tight spacing (8-12px)
- Separate sections with generous spacing (32-48px)
- Don't use the same spacing everywhere — create rhythm
- Use container queries for component-level responsiveness
```

## Reference File Paths

```
~/.agents/skills/frontend-design/reference/typography.md
~/.agents/skills/frontend-design/reference/color-and-contrast.md
~/.agents/skills/frontend-design/reference/spatial-design.md
~/.agents/skills/frontend-design/reference/motion-design.md
~/.agents/skills/frontend-design/reference/interaction-design.md
~/.agents/skills/frontend-design/reference/responsive-design.md
~/.agents/skills/frontend-design/reference/ux-writing.md
```

## Anti-Patterns to Always Include

Always include these anti-patterns in every Gemini prompt — they prevent "AI slop":

```
## Anti-Patterns (NEVER USE THESE)
- Card grids with icon + heading + text repeated identically
- Glassmorphism/blur effects used decoratively
- Pure black (#000) or pure white (#fff) — tint your neutrals
- Cyan-on-dark, purple-to-blue gradients, neon accents
- Gradient text on headings or metrics
- Bounce/elastic easing curves
- Same spacing everywhere — vary it for rhythm
- Center-aligning everything
- Rounded rectangles with generic drop shadows
- Dark mode with glowing accents as a substitute for actual design decisions
- Big icons with rounded corners above every heading
- Monospace typography as lazy shorthand for "technical" vibes
- Modals for things that could be inline or in a panel
```
