# Upasana Personal OS

[License](https://creativecommons.org/licenses/by-nc-sa/4.0/)

My AI-assisted task system: goals, backlog, schedule, and prioritized tasks—all in markdown your agent can read and act on.

Dump notes into `BACKLOG.md`, process them into `Tasks/`, and use natural-language prompts for quick focus, time-blocked daily plans, and weekly check-ins. Personal data (`Tasks/`, most of `Knowledge/`, `BACKLOG.md`, `GOALS.md`, `schedule.md`) stays local and gitignored—see [.gitignore](.gitignore).

**Based on** [PersonalOS](https://github.com/amanaiproduct/personal-os) by Aman Khan. This fork adds daily planning, schedule-aware scheduling, and custom Knowledge playbooks while keeping the upstream `core/` reusable layer.


| Section                      | Status |
| ---------------------------- | ------ |
| Task Management              | Yes    |
| Goal-driven Prioritization   | Yes    |
| Knowledge Base               | Yes    |
| Backlog Processing           | Yes    |
| Daily Planning (time blocks) | Yes    |
| Help Menu (7 prompts)        | Yes    |
| Schedule-aware Planning      | Yes    |
| Session Evals                | Yes    |
| Integrations (optional)      | Yes    |
| MCP Server (optional)        | Yes    |


---

## Quick Start

### 1. Clone and enter the repo

```bash
git clone <your-fork-url> upasana-personal-os
cd upasana-personal-os
```

### 2. Run setup (~2 minutes)

```bash
./setup.sh
```

Setup will:

- Create `Tasks/` and `Knowledge/`
- Copy templates (`AGENTS.md`, `.gitignore`, `BACKLOG.md`) if missing
- Ask about your role, vision, and priorities
- Generate your personal `GOALS.md`
- Optionally install the [amans-skills](https://github.com/amanaiproduct/amans-skills) pack

**Agent-guided setup:** You can also skip bash and have your AI walk through setup interactively—the instructions at the top of [setup.sh](setup.sh) describe the same steps (directories, five questions, `GOALS.md`).

**Note:** Python 3.10+ is only required for the optional MCP server. Markdown-only usage works without Python.

### 3. Add your schedule

```bash
cp schedule.example.md schedule.md
# Edit weekly hours, overrides, and planning notes
```

`schedule.md` is gitignored. Your agent uses it for time-blocked plans and replans.

### 4. Start talking to your agent

```
Read AGENTS.md and help me get organized
```

Or type `menu` to see numbered workflows without running one yet.

---

## How It Works

1. **Brain dump** — Drop notes into `BACKLOG.md` (no structure needed).
2. **Process** — Say `Process my backlog` to turn notes into tasks in `Tasks/`.
3. **Prioritize** — Tasks align with `GOALS.md` (P0–P3, categories, status).
4. **Focus** — Say `What should I work on today?` for up to three focus tasks (quick focus).
5. **Plan** — With `schedule.md` configured, say `Plan my day` for a time-blocked plan.

Reference material lives in `Knowledge/` and links from task frontmatter via `resource_refs`.

---

## Help Menu

Say `**menu`**, `**help**`, or `**what can you do?**` (standalone) to list workflows—nothing runs until you pick a number or phrase.


| #   | Say this                         | What you get                       |
| --- | -------------------------------- | ---------------------------------- |
| 1   | `What should I work on today?`   | Up to 3 focus tasks (quick focus)  |
| 2   | `Plan my day`                    | Time-blocked day plan              |
| 3   | `Process my backlog`             | Triage `BACKLOG.md` into tasks     |
| 4   | `What's next today?`             | Next batch after finishing a block |
| 5   | `Replan my day — I lost 2 hours` | Adjust remaining hours             |
| 6   | `Weekly check-in`                | P0/P1 progress vs goals            |
| 7   | `Show me my P0 and P1 tasks`     | Urgent list only (no full plan)    |


Other useful phrases:

- `Mark [task] as done` — Complete work and update task status.
- `I'm blocked on [task] because [reason]` — Flag blockers.

Full agent behavior is defined in [AGENTS.md](AGENTS.md).

### Priorities


| Priority | Meaning       | Suggested limit |
| -------- | ------------- | --------------- |
| **P0**   | Do today      | max 3           |
| **P1**   | This week     | max 7           |
| **P2**   | Scheduled     | —               |
| **P3**   | Someday/maybe | —               |


---

## Directory Structure

```
upasana-personal-os/
├── AGENTS.md              # Agent workflows (help menu, planning, backlog)
├── GOALS.md               # Your goals (gitignored)
├── schedule.md            # Your hours/exceptions (gitignored)
├── schedule.example.md    # Schedule template (committed)
├── BACKLOG.md             # Inbox (gitignored)
├── Tasks/                 # Task markdown files (gitignored)
├── Knowledge/             # Reference docs (mostly gitignored)
├── setup.sh
├── core/
│   ├── mcp/server.py      # Optional MCP server
│   ├── evals/             # Session evaluations
│   ├── integrations/    # Granola and future integrations
│   └── templates/
└── examples/              # Workflows and tutorials
```

**Committed in repo:** `AGENTS.md`, `schedule.example.md`, `core/`, `examples/`, templates, and shared Knowledge such as `Knowledge/pm-interview-rubrics/` (this fork tracks interview rubrics there).

**Local only (gitignored):** `GOALS.md`, `schedule.md`, `BACKLOG.md`, `Tasks/*.md`, and most other `Knowledge/*.md` files.

---

## Daily Workflow


| When                    | Prompt                                                                           |
| ----------------------- | -------------------------------------------------------------------------------- |
| **Morning**             | `menu` → 1 or 2, or `What should I work on today?` / `Plan my day`               |
| **During the day**      | Dump to `BACKLOG.md`; `Process my backlog` when ready                            |
| **After a focus block** | `What's next today?`                                                             |
| **Disruption**          | Update Day-of Exceptions in `schedule.md`, then `Replan my day — I lost 2 hours` |
| **End of week**         | `Weekly check-in` + `Process my backlog`                                         |


Save specs, notes, and playbooks under `Knowledge/` and link them from tasks. See [examples/workflows/README.md](examples/workflows/README.md) for longer workflow write-ups.

---

## Optional Capabilities

### MCP server

For structured task tools (`list_tasks`, `create_task`, `process_backlog_with_dedup`, `get_task_summary`, and more), run the MCP server described in [core/README.md](core/README.md):

- Python 3.10+
- Dependencies in [core/requirements.txt](core/requirements.txt)
- Entry point: [core/mcp/server.py](core/mcp/server.py)

Markdown-only workflows in Cursor or Claude Code do not require MCP.

### Integrations

Optional extensions live under [core/integrations/](core/integrations/):


| Integration                                           | Setup prompt                 |
| ----------------------------------------------------- | ---------------------------- |
| [Granola](core/integrations/granola/) (meeting notes) | `Set up Granola integration` |


See [core/integrations/README.md](core/integrations/README.md) for details.

### Skill packs

[setup.sh](setup.sh) can clone [amans-skills](https://github.com/amanaiproduct/amans-skills) (Excalidraw, design-for-agents, and more). Install manually anytime from that repo.

### Custom skills

Project-specific skills can live in `.claude/skills/` and auto-trigger when relevant (see [AGENTS.md](AGENTS.md)).

---

## Learn More


| Doc                                        | Contents                                     |
| ------------------------------------------ | -------------------------------------------- |
| [examples/README.md](examples/README.md)   | Workflows and tutorials                      |
| [Knowledge/README.md](Knowledge/README.md) | What belongs in Knowledge                    |
| [core/README.md](core/README.md)           | MCP tools, configuration, contributor detail |


**Upstream:** [github.com/amanaiproduct/personal-os](https://github.com/amanaiproduct/personal-os)

---

## Contributing

The `core/` directory is the reusable system layer. When changing shared behavior:

- Do not commit personal tasks, goals, backlog, or schedule
- Keep changes generic and documented
- Follow patterns in existing `core/` code and templates

---

## License

This work is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).

**Attribution:** This repository is a fork of PersonalOS. Original work copyright © 2025 Aman Khan ([amanaiproduct/personal-os](https://github.com/amanaiproduct/personal-os)). Fork modifications are offered under the same license.

You may view, use, modify, and share this repo with attribution for non-commercial purposes. Commercial sale of the work is not permitted; internal use for work and business is allowed under the license terms.

Full legal text: [https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode)