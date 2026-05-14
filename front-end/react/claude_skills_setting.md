## React용 cluade skills 세팅하기



## 1. Anthropic's Frontend Design Skill

This is Anthropic's official skill that gives Claude a design system and philosophy before it touches any code, producing bold aesthetic choices, distinctive typography, purposeful color palettes, and animations that feel intentional rather than decorative.

**How to install (pick one method):**

**Method A — Using the `npx skills` command (recommended):**

```bash
npx skills add anthropics/claude-code --skill frontend-design
```

**Method B — Manual download into your project:**

```bash
mkdir -p .claude/skills/frontend-design
curl -o .claude/skills/frontend-design/SKILL.md \
  https://raw.githubusercontent.com/anthropics/claude-code/main/plugins/frontend-design/skills/frontend-design/SKILL.md
```

**How to use it:**

Claude automatically uses this skill for frontend work — you don't need to do anything special. Just ask Claude Code to build UI and it kicks in. You can also invoke it explicitly:

```
/frontend-design Build a landing page for a personal portfolio site with dark mode
```

Good example prompts to try:

- "Create a dashboard for a music streaming app"
- "Build a landing page for an AI startup"
- "Redesign my hero section with modern UI best practices"

------

## 2. Vercel's React Best Practices Skill

This is a comprehensive performance optimization guide for React and Next.js applications created by Vercel Engineering, with 40+ actionable rules organized by impact level.

**How to install (pick one method):**

**Method A — Using `npx skills`:**

```bash
npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices
```

**Method B — Shorter form:**

```bash
npx skills add vercel-labs/agent-skills
```

This installs all Vercel agent skills at once (React best practices, composition patterns, deployment, etc.).

**How to use it:**

This skill triggers automatically on tasks involving React components, Next.js pages, data fetching, bundle optimization, or performance improvements. So once installed, Claude Code will reference these rules when writing or reviewing React code.

You can also ask for it directly:

- "Review this component for performance issues"
- "Refactor this page to eliminate data fetching waterfalls"
- "Check my bundle imports for unnecessary bloat"

------

## How They Work Together

These two skills complement each other nicely — frontend-design handles the **visual quality** (making things look polished and distinctive), while React best practices handles the **code quality** (making things fast and well-structured). Install both, and your typical workflow looks like:

1. **Install both skills** in your project root
2. **Ask Claude Code to build a component** — e.g., "Build a responsive hero section for my homepage"
3. **Frontend-design** steers the visual output toward something distinctive rather than generic
4. **React best practices** ensures the generated code avoids common performance pitfalls like unnecessary re-renders or barrel import bloat
5. **You review and iterate** — push back on anything you don't like

**One tip for beginners:** each rule can be reviewed and applied independently, and individual rule files include explanations of why each pattern matters alongside before-and-after code examples, so you can learn from the rules themselves as you go. You can browse them on the Vercel GitHub repo at `github.com/vercel-labs/agent-skills`.

For the most current installation instructions, you can also check the official Claude Code docs at `docs.claude.com/en/docs/claude-code/overview`.