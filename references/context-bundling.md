# Context Bundling for Large Codebases

When the project is large, use `packx` to bundle relevant code context before building the Gemini prompt.

## When to Bundle

- The component/page being redesigned imports from many files
- You need to show Gemini the design system (tokens, shared components, utilities)
- The project has a complex directory structure

## How to Bundle

```bash
# Bundle specific directories relevant to design
packx --limit 49k --include "src/components/**" --include "src/styles/**" --output /tmp/design-context.txt

# Bundle a specific feature area
packx --limit 49k --include "src/app/dashboard/**" --output /tmp/feature-context.txt
```

## Using Bundled Context in Prompts

Write the full prompt (including bundled context) to a temp file, then pipe to Gemini:

```bash
# Write prompt to temp file
cat > /tmp/gemini-prompt.txt << 'PROMPT_EOF'
You are a senior UI/UX designer...

## Existing Codebase Context

[PACKX OUTPUT GOES HERE]

## Design System

[.impeccable.md CONTENT]

## Task

[WHAT TO BUILD]
PROMPT_EOF

# Send to Gemini
cat /tmp/gemini-prompt.txt | gemini -m gemini-3.1-pro-preview -p "Follow the instructions from stdin." -y
```

## Context Priority

When hitting context limits, prioritize in this order:
1. `.impeccable.md` (design language — always include)
2. The specific file being redesigned
3. Shared design tokens / CSS variables
4. Related components that should match style
5. Layout/routing patterns
