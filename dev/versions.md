# Model Knowledge vs. Library Versions

## Training Cutoff Context

| Date              | Event                                  |
| ----------------- | -------------------------------------- |
| August 2025       | Claude Sonnet 4.6 training data cutoff |
| February 17, 2026 | Claude Sonnet 4.6 released publicly    |

The gap between cutoff and release means the model shipped ~6 months after its knowledge stopped.
Any library that reached its current major version after August 2025 has **no training data** in Sonnet 4.6.

---

## Solid Knowledge (released before August 2025)

These versions were part of the training data. The model can reason about them from memory without needing to fetch docs.

| Library                  | Known Version     | Notes                         |
| ------------------------ | ----------------- | ----------------------------- |
| React                    | 19.0.0            | React 19 released Nov 2024    |
| react-dom                | 19.0.0            | Same release                  |
| react-router-dom         | 7.0–7.x (early)   | v7 released Dec 2024          |
| Tailwind CSS             | 4.0               | v4 released Jan 2025          |
| @tailwindcss/vite        | 4.0               | Same release                  |
| TypeScript               | 5.7.x             | 5.7 released Nov 2024         |
| Vite                     | 6.x               | v6 released Nov 2024          |
| Vitest                   | 3.x (early)       | v3 released early 2025        |
| @vitejs/plugin-react     | 4.x               | Within v6 Vite era            |
| Radix UI                 | 1.x               | Stable throughout 2024–2025   |
| lucide-react             | 0.4xx             | Pre-1.0 API                   |
| @tanstack/react-query    | 5.x               | v5 released Oct 2023          |
| @tanstack/react-form     | 0.x–1.x (early)   | v1 released 2024              |
| @playwright/test         | 1.x (up to ~1.45) | Minor updates throughout 2025 |
| tailwind-merge           | 2.x–3.x           | Stable                        |
| class-variance-authority | 0.7.x             | Stable                        |

---

## No Training Data (released after August 2025)

The model has zero parametric knowledge of these versions. When working with them, always use WebSearch or the context7 MCP to fetch current docs first.

| Library              | Unknown Version  | Released       | Gap                      |
| -------------------- | ---------------- | -------------- | ------------------------ |
| TypeScript           | **6.0**          | March 23, 2026 | 7 months post-cutoff     |
| Vite                 | **7.0 and 8.0**  | 2026           | 6–7 months post-cutoff   |
| @vitejs/plugin-react | **5.x and 6.x**  | 2026           | Follows Vite major       |
| Vitest               | **4.x**          | Early 2026     | 6+ months post-cutoff    |
| @tailwindcss/vite    | **4.3+** (minor) | 2026           | Minor; API likely stable |
| lucide-react         | **1.x**          | 2026           | 0→1 stability bump       |

---

## Currently Installed (post-update, June 2026)

These are the versions actually in the project after the June 2026 update session.
Minor/patch updates within safe major versions were applied; major unknowns (TS 6, Vite 8, Vitest 4) were intentionally skipped.

| Package                   | Installed | Knowledge                          |
| ------------------------- | --------- | ---------------------------------- |
| react / react-dom         | 19.2.7    | Solid (19.x patches, same API)     |
| react-router-dom          | 7.18.1    | Solid (same v7 API)                |
| tailwindcss               | 4.3.2     | Solid (same v4 API)                |
| @tailwindcss/vite         | 4.3.2     | Likely solid (minor changes)       |
| vite                      | 6.4.3     | Solid (v6, patched for security)   |
| vitest                    | 3.2.6     | Solid (v3, patched for security)   |
| typescript                | 5.7.3     | Solid                              |
| @playwright/test          | 1.61.1    | Mostly solid (minor updates)       |
| @anthropic-ai/claude-code | 2.1.196   | devDep only; no impact on app code |

---

## Recommendation

When adding TanStack Query, TanStack Form, or MSW (planned per CLAUDE.md), use context7 to pull
their current docs into context before writing integration code — those libraries move fast and
minor API surface changes between 2025 and 2026 are likely.
