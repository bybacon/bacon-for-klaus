---
name: create-tester-overview-file
description: Generate or update the manual testing guide at docs/overviews/testing-overview-YYYY-MM-DD.md. Produces a Markdown document with feature overviews, scenario checklists, known gaps, and space for tester notes. Use when preparing a testing session, onboarding a new tester, or reviewing coverage before a release.
---

# Create Tester Overview File

## Goal

Produce a self-contained Markdown document a dedicated tester can work through without developer help.
The document is a **persona-structured walkthrough** - not grouped by story ID or feature.
Each session groups steps by who is logged in to minimise login switches.
A tester should be able to follow it top to bottom as if they were real users discovering the app for the first time.
The document must tell them **what to do and in what order**, **what to expect**, **what is fragile**, and give them room to **record their findings**.

---

## Discovery

Before generating anything, gather:

1. **Done features** - read every `.feature` file under `<tracker>/features/4_done/` (`<tracker>` is the tracker root - the directory holding `backlog.md` and `.next-id`, `tracker/`; see the `bacon:bacon-tracker` skill). Extract:
   - Feature name and ID (from the `# id:` header)
   - Goal and context comments
   - All `Scenario:` titles and their Gherkin steps
   - The personas involved in each scenario

2. **Done bugs and chores** - read `<tracker>/bugs/4_done/` and `<tracker>/chores/4_done/`. Note bug fixes that hint at fragile areas worth extra attention.

3. **Backlog/ icebox** - read `<tracker>/features/2_backlog/` and `<tracker>/features/1_icebox/`. Note what is not yet done so you can mark it as a Known Gap.

4. **Recent changes** - read `CHANGELOG.md` (at least the last 5 releases). Surface:
   - New additions that may not yet have a done feature file
   - Bug fixes that hint at fragile areas

5. **Open gaps in test coverage** - check `spec/` and `features/` for any `skip`, `pending`, or `@wip` tags.

6. **Persona sessions** - before writing, map the complete test into sessions grouped by persona. Goal: minimise login switches. Typical order: Admin → Place Owner (setup) → Place Owner (events) → Place Owner (team) → Staff → Guest → Place Owner (management) → Place Owner (security) → Admin (management). Some sessions may need the Place Owner to dip back in to respond to Guest or Staff actions - mark these clearly.

Ask the user: **"Who is the testing session for and what is the target environment (local / staging / production)?"** before generating.

---

## Personas and email addresses

Use the `+` alias pattern for test emails so all addresses deliver to one inbox, e.g.:

| Persona | Default email alias |
|---|---|
| Klaus | `testing+klaus@ourtestdomain.com` |
| Horst | `testing+horst@ourtestdomain.com` |

Reference these concrete addresses in every step that involves an email or a login. Never say "use a test email address you can access."

---

## Output format

Create a **new dated file** for each test session:
`docs/overviews/testing-overview-YYYY-MM-DD.md`

Do **not** overwrite previous dated files - each is a permanent record of that session.

Use this structure:

```markdown
# Testing Overview - YYYY-MM-DD · Version x.x.x
> Environment: [local | staging | production]
> Prepared for: [tester name or "External tester"]
> Prepared by: Klaus + Alex

---

## How to use this document
...

## Personas & Credentials
| Name | Role | Login email | Access |
...

## Test Tiers
| Tier | When to run | Time |
...

---

## Smoke Test (~10 min)
...

---

## Release Test - vX.X.X (~N min)
One section per release since the last complete test.
Each section covers only the new stories and bugs in that release.
...

---

## Complete Test (~3–4 hours)

Work through Sessions A → N in order.

---

## Session A - [Persona]: [Theme] · [Start here note if applicable]

[One sentence setting the scene.]

- [ ] [Plain-English action]
  - Expected: [what success looks like]

#### Notes
> _Add your observations here. Use [ ] for issues you find._

---

## Known Gaps

| Area | Notes |
|---|---|
| [feature or path] | [why it cannot be tested] |

---

## Fragile Areas

- **[area]** - [why fragile; which bug was fixed or which spec is pending]

---

## Session Log

| # | Session | What I tested | Result | Notes |
|---|---|---|---|---|
| A | [Persona: Theme] | | | |
```

---

## Session ordering rules

Session ordering is crucial:
- Order personas and their stories to enable future tests, not block them (e.g. a Admin creates invitation codes before the Users can sign up)
- If one Persona needs to respond to another Persona action mid-session (e.g. promote a waitlist, approve OOO), batch these responses into a dedicated management session rather than interleaving them with the current session

---

## Known gap identification

Mark something as a Known Gap when:
- The feature exists in the icebox or backlog but has no done file
- The UI path to reach a feature is unknown (e.g. "Feature view not discoverable")
- The test requires manual staging console intervention (e.g. backdating `created_at`)

---

## Fragile area identification

Mark something as a Fragile Area when:
- A done bug file exists in `<tracker>/bugs/4_done/` for that area
- A spec has a `skip`, `pending`, or `@wip` tag
- The previous testing session's notes flagged unexpected behaviour in that area

Always include what to look for, not just what was fixed.

---

## Delivery

- Write the dated file to `docs/overviews/testing-overview-YYYY-MM-DD.md`.
- Print a one-paragraph summary: what changed from the previous overview, what sessions are new, and what was intentionally left out.
- Do **not** move any feature or tracker files - this skill is read-only with respect to the pipeline.
