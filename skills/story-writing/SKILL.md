---
name: story-writing
description: Write a feature, bug or chore story in the project's bacon-tracker using the team's templates. Produces a story file with title, business value, Gherkin acceptance criteria, notes and labels. Use when the user asks to add, write or draft a story, or when bug-fixing or reviewing surfaces work that needs its own story.
---

# Story writing

The always-on rules for what a story is live in the `stories` rule. This skill is the procedure and the templates.

## Procedure

1. Pick the type: **feature** (new user value), **bug** (defect in an accepted feature), **chore** (necessary, no direct user value).
2. Name the persona. Use the project's existing personas and glossary. Never invent one.
3. Fill the template below for that type. Leave out optional blocks that would be empty.
4. Check INVEST and the ANDs in the acceptance criteria. Split if it fails either.
5. Create the file through the `bacon:bacon-tracker` skill (`/tracker new <type> <title>` or the rake task). Features are `.feature` files, bugs and chores are `.md`.
6. Confirm the ID and stage (new stories start in the icebox).

## What each type needs

**Feature** - who, what and why of the smallest valuable increment, from the user's perspective.
- **Title:** short, descriptive, names the persona
- **Business case:** who wants it, why, and to what end
- **Acceptance criteria:** Gherkin, one scenario per rule
- **Notes:** anything else the developer needs
- **Resources:** mocks, wireframes, flows, links
- **Labels:** epics, build numbers, users

**Bug** - a regression in delivered behaviour.
- **Title:** short and descriptive
- **Description:** what happens now, what should happen
- **Instructions:** steps to reproduce
- **Resources:** screenshots and other evidence

**Chore** - tech debt, dependencies, tooling.
- **Title:** short and descriptive
- **Description:** why it is needed. Does it make the team faster, or is it a dependency that will hurt if left?
- **Resources:** instructions, context, assets

## Templates

### Feature
```
Title: [Persona name] should (not) be able to [overarching action]

Business/User Value: As [persona] I want to [action by user] so that [value or need met]

Acceptance Criteria
GIVEN [necessary context and preconditions for story]
WHEN [action]
THEN [reaction]
AND THEN [reaction]

**DEV NOTES**
[Relevant technical notes that developers may ask you to add to the story during weekly prep meeting (pre-IPM or IPM); sometimes they may add these themselves or add them as tasks]

**DESIGN Notes**
[prototype / design link inserted here; linking to a folder of a feature is good so designers can continue updating designs without anyone having to re-update the links to each design in the stories]

---other items that you may add to a story---

**NEEDS PM**
[Add reason for adding a label needs PM so you can view the story and remember context to unblock the story; usually this is some kind of thing we need to follow up with our client counterpart on]

**NEEDS DESIGN**
[Add reason for adding a label needs design so your designers can view the story and get context to unblock the story]
```

### Bug
```
Title: [Persona name] should (not) be able to [action]

**Currently:**
[What happens now in the regression]

**Expected:**
[What the correct behavior should be]

**STEPS TO REPRODUCE:**
[If needed, add steps to reproduce]

**REFERENCE:**
[Link or attach screenshot(s) to story if relevant]
```

### Chore
```
Title: [Short, descriptive]

**Why:**
[Why it is needed. Faster team, removed risk, or a dependency that will cause problems if left]

**Done when:**
[Observable outcome, one line]

**DEV NOTES**
[Relevant technical notes, links, commands]

**Resources:**
[Instructions, additional context, or other assets that help execute the chore]
```
