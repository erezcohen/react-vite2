# Claude Code Automation Recommendations

**Codebase Profile:**

- React 19 + TypeScript + Vite
- shadcn/ui (Radix UI + Tailwind CSS v4)
- Vitest + Playwright testing
- Prettier + ESLint configured
- GitHub CLI authenticated

---

## MCP Servers

**context7** — already installed.

**Playwright MCP** — you have Playwright e2e tests; this lets Claude drive the real browser to debug failing tests or verify UI behavior interactively.

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

**GitHub MCP** — you're authenticated with `gh`; this gives Claude direct access to issues, PRs, and Actions without shell roundtrips.

```bash
claude mcp add github -- npx -y @modelcontextprotocol/server-github
# requires GITHUB_PERSONAL_ACCESS_TOKEN env var
```

---

## Hooks

**Auto-format on Edit** — you have Prettier configured; this eliminates the manual `npm run format` step.

Add to `.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npm run format"
          }
        ]
      }
    ]
  }
}
```

**TypeScript check on Edit** — catches type errors immediately rather than at the end of a task.

```json
{
  "matcher": "Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "npm run typecheck"
    }
  ]
}
```

---

## Skills

**new-component** — you follow a consistent shadcn/ui pattern; a skill that scaffolds a new component with the right imports, Tailwind structure, and a test file saves repeated explanation.

Create `.claude/skills/new-component/SKILL.md` describing your component conventions, file locations, and test template.

**pr-check** — a skill that runs lint + typecheck + tests then summarizes what's ready to merge.

---

## Subagents

**ui-reviewer** — frontend-heavy app; a subagent that checks accessibility, Tailwind class consistency, and mobile responsiveness in parallel with your main work.

Create `.claude/agents/ui-reviewer.md`:

```markdown
---
name: ui-reviewer
description: Reviews React components for accessibility, responsive design, and Tailwind consistency
---

Review the changed components for: ARIA attributes, keyboard navigation, mobile-first Tailwind classes, and dark mode support. Report issues only — no fixes.
```

---

## Plugins

**commit-commands** — structured git commit workflow with conventional commit format. Install via:

```
/plugin commit-commands
```
