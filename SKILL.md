---
name: gemini-design
description: Generate layout and design mockups using Gemini CLI with project design context from .impeccable.md. Creates variations as new routes/pages/components with easy preview and comparison. Use when asked to mock up designs, generate layout variations, or get Gemini design ideas.
user-invokable: true
version: "1.0"
---

Generate design mockups by combining your project's design language (.impeccable.md) with Gemini CLI's layout generation capabilities. Produces multiple variations with a comparison preview.

## Prerequisites

1. **Gemini CLI** must be installed and authenticated. Run `gemini --version` to verify — if it fails, stop and tell the user.
2. **Design context** must exist — check `.impeccable.md` in project root. If missing, run `teach-impeccable` first. Do NOT proceed without design context.

## Workflow

### Step 1: Gather Context

Read all that exist:
- `.impeccable.md` — design language, brand, principles
- `package.json` — framework, dependencies, available libraries
- Existing routes/pages/components relevant to the request
- Design tokens / CSS variables / tailwind config / theme files

Determine the **output format** from the project:
- **Next.js App Router** → `app/mockups/v1/page.tsx`, `v2/page.tsx`, `v3/page.tsx`
- **Next.js Pages** → `pages/mockups/v1.tsx`, `v2.tsx`, `v3.tsx`
- **Remix/SvelteKit** → framework-appropriate route files
- **React SPA** → `src/components/mockups/FeatureV1.tsx`, `V2.tsx`, `V3.tsx`
- **Plain HTML** → `mockups/v1.html`, `v2.html`, `v3.html`
- **Other** → ask the user

### Step 2: Build the Gemini Prompt

Use the [prompt-template](references/prompt-template.md) reference. Write the prompt to a temp file — never inline it in `-p` (escaping issues).

Key rules:
- Always include full `.impeccable.md` content
- Include existing code context (the component being redesigned, or similar components for style reference)
- Include relevant design reference excerpts — pull from `frontend-design` skill references (typography, color, spatial, motion) when they match the task. See [design-references](references/design-references.md).
- Specify exact file format, framework, and available dependencies
- Request a single complete file — no explanations, no markdown wrapping, no import maps
- For large codebases, use `packx` to bundle context. See [context-bundling](references/context-bundling.md).

### Step 3: Generate with Gemini CLI

Write prompt to temp file and pipe via stdin:

```bash
cat /tmp/gemini-prompt-v1.txt | gemini -m gemini-3.1-pro-preview -p "Generate the file exactly as instructed. Output ONLY the file content, no markdown fences." -y --sandbox
```

**Flags**: `-y` (auto-approve file writes), `--sandbox` (safe execution).

### Step 4: Validate Output

After each generation, verify the output:
1. Check the file exists and is non-empty
2. Check it contains valid JSX/HTML (look for opening tags)
3. If Gemini wrapped output in markdown fences (\`\`\`), strip them
4. If the file is truncated or contains errors, re-run with a shorter prompt or split the component

If validation fails twice, tell the user and suggest simplifying the request.

### Step 5: Create Variations

Generate **5 variations** by default. Each variation should differ in **exactly one dimension** — this makes comparison meaningful. See [variation-strategies](references/variation-strategies.md) for the full matrix.

Quick reference — pick one dimension per set:
- **Layout**: grid / stack / asymmetric / sidebar / split-screen
- **Density**: spacious / balanced / compact
- **Hierarchy**: typography-led / color-led / spatial
- **Interaction model**: static / progressive-disclosure / command-palette
- **Visual tone**: minimal / editorial / bold-graphic / organic

Run sequentially with 2s delay between calls to avoid rate limits:

```bash
for v in 1 2 3; do
  cat /tmp/gemini-prompt-v${v}.txt | gemini -m gemini-3.1-pro-preview -p "..." -y --sandbox
  sleep 2
done
```

### Step 6: Create Comparison Preview

Generate a **preview gallery** page that shows all variations with navigation. Use the [preview-gallery](references/preview-gallery.md) reference for framework-specific templates.

The gallery should provide:
- Tab/button navigation between variations
- Labels describing each variation's design direction
- A "pick this one" action for each variation

### Step 7: Iterate

After the user reviews:
- **Pick** → copy winning variation to the target location, clean up mockups directory
- **Combine** → merge elements from multiple variations (Claude does this, not Gemini)
- **Refine** → re-run specific variation with adjusted direction
- **Expand** → generate 3 more variations exploring a new dimension
- **Theme variant** → re-run winner in dark/light mode or responsive variant

## Important Notes

- If Gemini produces generic/templated output, the prompt needs more design context. Enrich with `.impeccable.md` excerpts and specific anti-patterns from the frontend-design skill.
- Use `--include-directories` flag to give Gemini access to additional project directories for file reads.
- The `--output-format json` flag can capture structured output for programmatic processing.
