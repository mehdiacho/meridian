# MERIDIAN (app)

Code for MERIDIAN, the daily record with pluggable feeds. The spec lives beside this repo
in `X:\Ventures\02_AI_BLACKWALL\software\MERIDIAN` (README, CLAUDE.md, 00–08, designs/).

The first module is **projects**: one dashboard over every project, what state it is in,
what Mehdi decided about it, which AI tool holds its context, where the code is, and what
it is waiting on. The day view comes after it. See [DECISIONS.md](DECISIONS.md).

## Open the dashboard

Browsers won't let a page read a JSON file when it's opened by double-click, so serve the folder:

```
cd X:\Ventures\02_AI_BLACKWALL\software\MERIDIAN-app
python -m http.server 8000
```

Then open <http://localhost:8000>. Edit `data/projects.json` and reload; there is no build step.

## Data: `data/projects.json`

| Field | Meaning |
|---|---|
| `id`, `name`, `summary` | Stable slug, display name, what it is |
| `status` | `active` · `pending` · `paused` · `idea` · `done` · `shelved` · `chucked` |
| `decision` | Mehdi's 2026-10-16 call: `push` (push now) · `research` · `backup` · `keep` · `low` (low priority) · `shelve` · `chuck` · `merged` · `null` (undecided) |
| `ai` | `kind` (`claude-project-thread`, `claude-code`, `codex`, `cowork`, `gemini`, `unknown`), `name` (thread name or session id), `also` (other places it has been worked on) |
| `where` | `path` (local folder), `repo` (`owner/name` on GitHub), `branch`, `notes` |
| `lastActivity` | Date of the newest worklog entry or commit known |
| `next` | Next steps, in order |
| `blockedOn` | `mehdi` · `claude` · `null` |
| `ask` | When blocked on Mehdi: the exact thing he needs to do or decide |
| `mergedInto` | For merged projects, the `id` they merged into |

Top-level `homelab` lists NAS services with `ok`/`warn` status. `waitingOnMehdi` holds
asks that don't belong to one project. Every project with `blockedOn: "mehdi"` is added to
the waiting list automatically.

**Source of truth** is the worklog at `X:\ai.tools\worklog` (which also records which AI
each project lives in), then `X:\Projects\PROJECTS-INVENTORY.md`. When they disagree with
this file, they win; update this file.
