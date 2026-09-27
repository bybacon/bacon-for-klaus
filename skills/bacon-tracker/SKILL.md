---
name: bacon-tracker
description: Use this skill whenever the user mentions stories, bugs, chores, the backlog, icebox, bacon-tracker, or wants to create/ move/ list/ manage work items. Also use when the user says things like "add a story", "what's next", "start a bug", "commit to backlog", "mark done", or any variant of managing tasks in a tracker. This skill governs all interactions with the bacon-tracker system.
version: 3.1.0
---

# Bacon Tracker

bacon-tracker manages stories, bugs, and chores as plain files in a git repo. Work moves through four stages: **icebox → backlog → started → done**.

It is a released Ruby gem (Ruby >= 3.3, MIT): [rubygems.org/gems/bacon-tracker](https://rubygems.org/gems/bacon-tracker), source at [github.com/bybacon/bacon-tracker](https://github.com/bybacon/bacon-tracker). When a rule here and the gem's docs disagree, the gem wins: [story format](https://github.com/bybacon/bacon-tracker/blob/main/docs/story-format.md), [lint checks](https://github.com/bybacon/bacon-tracker/blob/main/docs/linting.md), [vocabulary](https://github.com/bybacon/bacon-tracker/blob/main/docs/vocabulary.md), [flow](https://github.com/bybacon/bacon-tracker/blob/main/docs/flow.md).

## Setup

If the project has no tracker yet (no `backlog.md` / `.next-id`), set one up with the gem rather than creating directories by hand:

```bash
gem install bacon-tracker
tracker-init --path . --namespace APP --title 'My App' --command --yes
```

`tracker-init` writes the tracker, `docs/decisions/`, and registers the project in a `dashboard.md` with a central `Rakefile` beside it. `--command` installs `/tracker`. Ask the user for the namespace (2–8 chars, letter first) and whether they want a central tracker home or a per-project `Rakefile` (`require "bacon_tracker/tasks"` + `BaconTracker.configure` in the project) before running it.

## How to act on the tracker

Never move story files or edit `.next-id` by hand. Two tools do it safely:

1. **`/tracker`** - the Claude Code command shipped in the gem ([canonical copy](https://github.com/bybacon/bacon-tracker/blob/main/lib/bacon_tracker/commands/tracker.md)), installed at `.claude/commands/tracker.md` next to the dashboard by `tracker-init --command`. It knows every subcommand (`new`, `commit`, `start`, `done`, `edit`, `delete`, `show`, `list`, `next`, `lint`, `status`) and the ID-allocation guards. If it is missing, tell the user to run `tracker-init --command`.
2. **Rake tasks** - when the command is not available or the user prefers the shell. In a central tracker home, run from the directory holding `dashboard.md` and prefix `NS=<namespace>`; in a per-project setup, run from the project without `NS=`. Always quote the task: zsh treats `[ ]` as a glob.

```bash
NS=APP rake "story:feature[My title,size=M,assignee=AB]"   # create in features/1_icebox
NS=APP rake "story:bug[My title]"                          # create in bugs/1_icebox
NS=APP rake "story:chore[My title]"                        # create in chores/1_icebox
NS=APP rake "story:commit[APP-001]"                        # icebox → backlog (adds to backlog.md)
NS=APP rake "story:start[APP-001]"                         # backlog → started
NS=APP rake "story:done[APP-001]"                          # → done (removes from backlog.md)
NS=APP rake "story:edit[APP-001,size=M,assignee=AB]"       # size/assignee/blocked_by/linked_to/title/body; empty value clears
NS=APP rake story:next                                     # top backlog item
NS=APP rake story:lint                                     # check stages, ids, backlog.md, relationships
```

Both allocate IDs under a file lock and floor `.next-id` above the highest ID on disk, so a stale counter never reissues an existing ID. A rake `body=` is one shell line. Use `/tracker` or edit the file for multi-line bodies and subtasks.

The board (`tracker-dashboard --dashboard dashboard.md`, or `rake story:server` per project) serves `http://localhost:4567`. It is a view over the same files. Suggest it when the user wants to see or reorder the backlog, but you act through `/tracker` or rake.

## Vocabulary - use these words

- The work item is a **story**. Never a card, ticket, issue, or task. A **card** is only the board's rendering of a story.
- **type** - `feature`, `bug`, or `chore`. The directory is the plural.
- **stage** - the directory: `1_icebox`, `2_backlog`, `3_started`, `4_done`. The directory is authoritative; `status:` in the frontmatter only mirrors it.
- **commit / start / done** - the workflow verbs. `commit` means icebox → backlog, not a git commit. Say which when both are in play.
- **subtask** - a `- [ ]` line in a story body. There is no hierarchy: no epics, no parents.
- **blocked_by / linked_to** - you write these; `blocks` and `linked_from` are derived.
- Stories are **moved**, never assigned or transitioned. Nothing is closed or resolved. It is **done**, and done is append-only.
- There are no sprints, points, priorities, or epics. Position in `backlog.md` is the priority.

## Layout

The tracker root is the directory holding `backlog.md` and `.next-id`: `tracker/` at the repo root by convention (BT-ADR-0016), or `docs/tracker/` in older projects. Every story ID is prefixed with the project namespace, e.g. `APP-003`.

```
tracker/
  features/  bugs/  chores/     ← each with 1_icebox/ 2_backlog/ 3_started/ 4_done/
  backlog.md                    ← stack-ranked list of backlog IDs, nothing else
  .next-id                      ← next ID to assign
```

Decisions are **not** stories. They live in `docs/decisions/` with their own `.next-id`, are driven by the gem's `decision:*` rake tasks, and are handled by the `bacon:decision-making` skill.

## Workflow guidance

- **Icebox vs backlog**: icebox = might do; backlog = will do next, in priority order. Committing to the backlog is a commitment. Keep it short (3–7 items).
- **Started**: pull from the top of the backlog. Flag if more than 2 stories are started at once.
- **Done**: permanent. Never delete done stories.
- **backlog.md** contains only backlog story lines in priority order. No done items, no notes, no icebox references.
- After any move, confirm the new stage. Run `story:lint` (or `/tracker lint`) when something looks off; it exits non-zero on real breakage, so it also works as a CI gate.
