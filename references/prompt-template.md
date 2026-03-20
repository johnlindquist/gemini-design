# Gemini Prompt Construction

Always write prompts to a temp file and pipe via stdin. Never inline complex prompts in `-p`.

## Template

```
You are an elite UI designer who writes production-ready [FRAMEWORK] code. You have a strong opinion about aesthetics and never produce generic, templated interfaces.

## Design System (FOLLOW EXACTLY)

[PASTE FULL .impeccable.md CONTENT]

## Anti-Patterns (AVOID THESE)

These are hallmarks of AI-generated design slop. Never use:
- Card grids with icon + heading + text repeated identically
- Glassmorphism/blur effects used decoratively
- Pure black (#000) or pure white (#fff) — always tint
- Cyan-on-dark, purple-to-blue gradients, neon accents
- Gradient text on headings or metrics
- Bounce/elastic easing curves
- Identical spacing everywhere — create rhythm through variation
- Centering everything — left-aligned text with asymmetric layouts feels more designed

## Design Reference

[INCLUDE RELEVANT EXCERPT from frontend-design references — e.g., typography scale, color strategy, spacing system]

## Existing Code Context

[PASTE RELEVANT EXISTING CODE — the component being redesigned, or similar components that establish the visual language]

## Task

[SPECIFIC DESCRIPTION — what to build, what content it shows, what interactions it supports]

## Variation Direction

[FOR THIS SPECIFIC VARIATION: exactly what design dimension to explore and how]

## Output Rules

- Output ONLY the complete file content — no markdown fences, no explanations, no comments about the code
- Use only these dependencies: [LIST FROM package.json]
- File will be saved to: [TARGET_PATH]
- [FRAMEWORK CONVENTIONS: e.g., "export default function Page()" for Next.js App Router]
- Include sample data inline as const — do not import from external files
- All styles inline or via the project's existing styling approach (tailwind/CSS modules/styled-components)
```

## Prompt Quality Checklist

Before sending to Gemini, verify:
- [ ] `.impeccable.md` content is included in full
- [ ] Anti-patterns section is present
- [ ] At least one code context file is included (even for blank-slate designs, include a similar existing component)
- [ ] Content is specific — real data shapes, real labels, real metrics
- [ ] Variation direction is concrete and singular (one dimension)
- [ ] Output rules specify exact framework conventions

## Content Specificity

Bad: "Create a dashboard"
Good: "Create a project dashboard for a SaaS analytics tool showing: a metrics bar with 4 KPIs (MRR $42.1k trending up 12%, churn 2.3% trending down, MAU 8,421 trending up, NPS 72 stable), a sparkline chart area showing revenue over 12 months, and a recent activity feed with 5 items showing user avatar, action text, and relative timestamp"

Bad: "Create a settings page"
Good: "Create an account settings page with: a profile section (avatar upload, name, email, timezone dropdown), a notification preferences section (email digest frequency, push notification toggles for 4 categories), and a danger zone section (export data button, delete account with confirmation)"

## Stdin Pipe Pattern

```bash
# Write prompt to temp file
cat > /tmp/gemini-prompt-v1.txt << 'PROMPT_EOF'
[full prompt here]
PROMPT_EOF

# Pipe to Gemini — the -p flag appends to stdin
cat /tmp/gemini-prompt-v1.txt | gemini -m gemini-3.1-pro-preview \
  -p "Generate the file exactly as instructed above. Output ONLY the raw file content." \
  -y --sandbox
```

## Handling Gemini Output Issues

If Gemini wraps output in markdown fences:
```bash
# Strip leading/trailing markdown fences from generated file
sed -i '' '1{/^```/d}' /path/to/output.tsx
sed -i '' '${/^```/d}' /path/to/output.tsx
```

If Gemini adds explanatory text before/after the code, manually extract the code block or re-run with stronger "output ONLY the file" instructions.
