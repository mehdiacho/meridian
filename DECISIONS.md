# Decisions

Newest first. Each entry says what was decided, by whom, and what it costs.

---

## 2026-10-17 — Projects first, day view second

**Decided by:** Mehdi, in the claude.ai "Software Projects" thread ("projects first").

**What the spec says.** `MERIDIAN/07_BUILD_PLAN.md` builds the day first: Phase 0 is the
FastAPI/SQLite skeleton with Pydantic models for the whole data model and the REST/MCP
parity check; Phase 1 is the day, weather and ledger modules plus the React day view;
then a week of use before anything else is written. There is no projects module in
`04_MODULES.md` at all.

**What we are doing instead.** The first thing built is a **projects** module: one
dashboard over every project and the AI tool each one lives in (a claude.ai project
thread, Claude Code, Codex, Cowork, Gemini). The day view comes after it.

**Why.** Sixty-odd projects are spread across several AI tools and nobody, Mehdi
included, can see in one place what is active, what is waiting on him, and which thread
or session holds the context. That is the pain today. The day view solves a problem
that is still hypothetical until the routine is designed (which the spec itself defers
to Phase 2).

**How it is built for now.** Deliberately below the spec's stack: `data/projects.json`
plus a static `index.html`, no build step and no server beyond `python -m http.server`.
This is the module's card and data shape, not its final form.

**What this costs / how it rejoins the spec.**
- The JSON is shaped like a Meridian collection (one document per project, stable `id`,
  enums for `status` and `decision`). When Phase 0 lands, it becomes the `projects`
  collection behind `GET /api/v1/projects` and an MCP `list_projects()`, with the parity
  check applying from the start. The static page becomes the projects card and route in
  the React shell.
- Until then the data is maintained by hand and by agents editing the JSON, so it can go
  stale. The worklog (`X:\ai.tools\worklog`) is the source of truth; this file is a view of it.
- The spec's acceptance test (two weeks of unprompted daily use of the day view) is not
  affected. It still applies to the day view when that is built.
- `04_MODULES.md` and `07_BUILD_PLAN.md` in the spec are **not** edited by this change.
  Adding the projects module to them is an open question for Mehdi.
