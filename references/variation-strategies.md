# Variation Strategies

Each variation set should explore **one dimension** so the user can make a meaningful comparison. Don't randomize multiple things — isolate the variable.

## Variation Dimensions

### Layout Structure
- **V1**: Traditional top-nav + main content area
- **V2**: Sidebar navigation with content area (Linear-style)
- **V3**: Command-palette-driven with minimal persistent chrome (Raycast-style)

### Visual Density
- **V1**: Spacious — generous padding, one focal element per viewport section, dramatic whitespace
- **V2**: Balanced — standard padding, multiple visible elements, readable rhythm
- **V3**: Compact — tight spacing, data-rich tables/lists, power-user optimized

### Hierarchy Approach
- **V1**: Typography-led — size and weight create all hierarchy, minimal color
- **V2**: Color-led — color blocks, accents, and tints guide attention
- **V3**: Spatial — position, grouping, and negative space create importance

### Interaction Model
- **V1**: Static overview — all information visible, scroll-based
- **V2**: Progressive disclosure — sections expand, details on hover/click
- **V3**: Tab/panel navigation — content organized into switchable views

### Visual Tone
- **V1**: Minimal/refined — restrained palette, thin type, geometric
- **V2**: Editorial/magazine — bold typography, asymmetric layouts, dramatic scale
- **V3**: Organic/warm — rounded shapes, warm palette, handcraft feel

### Content Organization
- **V1**: Card grid — modular, scannable, each card is self-contained
- **V2**: List/feed — vertical stream, timeline-like, chronological
- **V3**: Dashboard panels — resizable sections, data-forward, mixed content types

### Theme Variants (use as a second pass)
- **Light** → Dark (or reverse)
- **Desktop** → Mobile (responsive variant)
- **Default** → High contrast (accessibility variant)

## Choosing the Right Dimension

Match the dimension to the user's need:

| User says | Best dimension |
|-----------|---------------|
| "I don't know what layout to use" | Layout Structure |
| "It feels too cramped/empty" | Visual Density |
| "Everything looks the same importance" | Hierarchy Approach |
| "Too much info on screen" | Interaction Model |
| "It looks generic" | Visual Tone |
| "I have a lot of different content types" | Content Organization |

## Variation Naming

Name variations descriptively, not just V1/V2/V3:

```
app/mockups/spacious-editorial/page.tsx
app/mockups/compact-sidebar/page.tsx
app/mockups/minimal-command-palette/page.tsx
```

Or with numeric prefix for ordering:

```
app/mockups/01-spacious-editorial/page.tsx
app/mockups/02-compact-sidebar/page.tsx
app/mockups/03-minimal-command-palette/page.tsx
```
