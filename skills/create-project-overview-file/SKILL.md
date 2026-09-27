---
name: create-project-overview-file
description: Generate or update the living application overview at docs/overviews/project_overview.md. Produces a Markdown document with the architecture summary, screen inventory, user flow maps, UI states, component inventory, and data relationships. Use when a major version release is imminent, before onboarding, design or planning work, or when a PR changes screens, flows or data.
---

# Create Project Overview File

## Goal

Produce a single, up-to-date Markdown document that describes the application **as it is in the code right now**, so that onboarding, design, planning and testing all start from the same accurate picture.
The document is a **map of the product** - not a changelog, not a story list, not a design pitch.
A new developer, designer or tester should be able to read it top to bottom and know what the app does, every screen it has, how a user moves between them, what each screen looks like in every state, and what data flows where.
The code is the source of truth. Where a story, a design or the previous overview disagrees with the code, the overview follows the code and notes the discrepancy.

---

## Discovery

Before generating anything, gather:

1. **Previous overview** - read `docs/overviews/project_overview.md` if it exists. It is the baseline you are updating, not a template to trust. Every claim in it must be re-verified against the code.

2. **Routes and entry points** - read the routing layer (e.g. `config/routes.rb`) and any authentication or authorisation layer. Extract:
   - Every reachable path and which persona can reach it
   - Public vs. authenticated vs. admin-only surfaces
   - Redirect rules and where a user lands after login, logout, sign-up and errors

3. **Screens** - read every view, template, component and mailer. For each, note:
   - Its name and the route that renders it
   - Key elements, data shown, and actions available
   - Which layout, navigation pattern and shared components it uses

4. **States** - for each screen, find the empty, loading, error and populated states in the code (conditionals, flash messages, error partials, guards). Note admin vs. regular user differences.

5. **Data model** - read the schema and models (e.g. `db/schema.rb`, `app/models/`). Extract entities, relationships, what is persistent vs. session or temporary, and what is updated in real time (jobs, broadcasts, polling).

6. **Done stories** - read `<tracker>/features/4_done/` (`<tracker>` is the tracker root - the directory holding `backlog.md` and `.next-id`, `tracker/`) and cross-check that every done feature is visible somewhere in the screen inventory or flow maps. A done feature with no screen or flow is a discrepancy to flag.

7. **Recent changes** - read `CHANGELOG.md` since the last overview update to know which sections need the closest look.

8. **Accepted decisions** - read `docs/decisions/accepted/` for constraints the overview must reflect (e.g. an accepted decision that a screen is intentionally read-only).

9. **Glossary and personas** - use the project's existing glossary and persona definitions. Never invent a new name for something the project already names.

Ask the user: **"Which release is this overview for, and is there anything that was deliberately left out of the code that I should not describe?"** before generating.

---

## Output format

Update the **single living file**:
`docs/overviews/project_overview.md`

Do **not** create dated copies - history lives in git. Bump the version and date in the header on every update.

Use this structure:

```markdown
# Project Overview - [App name] · Version x.x.x
> As of: YYYY-MM-DD
> Prepared by: Klaus + Alex

---

## What this app does
[Two or three sentences in plain language: what it does, who it is for, the core value.]

## Personas
| Persona | Who they are | What they can do |
|---|---|---|

---

## Architecture at a glance
[Stack, major subsystems, external services, background jobs, mail. One paragraph plus a short list. No implementation detail.]

---

## Screen inventory

One subsection per screen, grouped by persona or area, in the order a user would meet them.

### [Screen name] · `[route]`
- **Who sees it:** [persona(s)]
- **Purpose:** [one sentence]
- **Shows:** [data displayed]
- **Actions:** [buttons, links, forms and what they do]
- **States:** empty · loading · error · populated - [what each looks like; note admin differences]

---

## User flows

One subsection per flow. Each flow lists entry points, the step sequence, branches and conditionals, and where it ends.

### [Flow name]
- **Entry:** [where the user starts]
- **Steps:** 1. … 2. … 3. …
- **Branches:** [condition → outcome]
- **Ends at:** [screen or state]
- **Dead ends:** [anywhere a user can get stuck, or "none"]

---

## Component inventory
| Component | Where used | Notes |
|---|---|---|
[Navigation patterns, card types, form elements, modal styles, notification and flash types.]

---

## Data relationships
- **Entities:** [list with one-line descriptions]
- **Relationships:** [what belongs to what]
- **Flows between screens:** [what data a screen produces that another consumes]
- **Persistent vs. temporary:** [what is stored vs. session-only or derived]
- **Real-time:** [what updates without a page reload, and how]

---

## Discrepancies

| Where | What the code does | What the story / design / previous overview said |
|---|---|---|

---

## Known limitations

- **[area]** - [what does not exist or does not work yet, with the story ID if there is one]
```

---

## Discrepancy identification

Record a Discrepancy when:
- A done feature file describes behaviour the code does not have, or the other way round
- The previous overview describes a screen, flow or state that no longer exists or has changed
- A design link in a story shows something the implemented screen does not
- Two screens name the same thing differently

Never silently fix the code, the story or the design to make the discrepancy go away. Report it and let the user decide.

---

## Known limitation identification

Record a Known Limitation when:
- A flow has a dead end the code does not guard against
- A state (empty, loading, error) has no dedicated handling in the code
- A feature exists in the backlog or icebox and the overview would otherwise imply it is present

Always say what a user would experience, not just what is missing.

---

## Delivery

- Write the file to `docs/overviews/project_overview.md`.
- Print a one-paragraph summary: which screens, flows or data relationships changed since the previous overview, what discrepancies were found, and what was intentionally left out.
- Link to the overview in the release PR description (see the `bacon:pull-request` skill).
- Do **not** move any feature or tracker files and do **not** change application code - this skill is read-only with respect to the pipeline and the codebase.
