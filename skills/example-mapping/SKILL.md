---
name: example-mapping
description: Run an Example Mapping session on a story to clarify and confirm its acceptance criteria before it is pulled into development. Produces agreed rules, concrete examples, and a list of open questions, and updates the story file with them. Use when a story is about to move from backlog to started, or whenever a story's scope feels unclear.
---

# Example Mapping

## Goal

Turn a story into a **shared, concrete understanding of its scope** in one short, structured conversation.
The output is a map of four things: the **story**, the **rules** (acceptance criteria), the **examples** that illustrate each rule, and the **questions** nobody can answer yet.
The session is done when the user is confident enough to pull the story into development, or when it becomes clear the story must be sliced or sent back for homework.

---

## The four cards

Example Mapping was designed for index cards on a table. In conversation, keep the same four categories and label them explicitly:

| Card | Colour | What goes on it |
|---|---|---|
| **Story** | yellow | The story under discussion. Exactly one. |
| **Rule** | blue | An acceptance criterion or business rule. One rule per card. |
| **Example** | green | A concrete scenario that illustrates one rule: specific people, specific data, specific outcome. |
| **Question** | red | Something nobody in the conversation can answer now. Capture it and move on. |

---

## Discovery

Before starting the conversation, gather:

1. **The story** - read the story file in `<tracker>/` (`<tracker>` is the tracker root - the directory holding `backlog.md` and `.next-id`, `tracker/` by default or `docs/tracker/` in older projects. See the `bacon:bacon-tracker` skill). Extract the title, the business value, and any acceptance criteria already written.

2. **Existing rules** - anything already stated as GIVEN / WHEN / THEN in the story becomes a first blue card. Do not re-ask what the story already answers.

3. **The codebase** - for every question that can be answered by reading code, read the code instead of asking. Look at models, routes, existing feature files and specs around the same area.

4. **Glossary and personas** - use the project's existing terms and personas in every rule and example. Sharpen fuzzy language against the glossary.

5. **Neighbouring stories** - check `<tracker>/features/4_done/` and the backlog for stories that touch the same screens or data, so rules do not contradict what already exists.

---

## Running the session

1. **Place the story.** Restate it in one sentence. Confirm with the user that this is the story being mapped.

2. **Place the known rules.** List the rules you already have, one per line, labelled `Rule 1`, `Rule 2`, …

3. **Work rule by rule.** For each rule:
   - Propose one or two examples in plain language, with concrete names and data.
   - Ask the user to confirm, correct, or add an example.
   - If an example exposes a rule that is not yet written, add a new rule.
   - If an example cannot be resolved, write it down as a question and move on.

4. **Ask one question at a time.** Wait for the answer before asking the next. For every question, give a recommended answer so the user can just say "yes".

5. **Read the map out loud.** After each round, show the current map: rules, examples under each rule, open questions.

6. **Call it.** Stop when one of the exit conditions below is met.

---

## Reading the map

The shape of the map tells you what to do next:

- **Many red (question) cards** - the story still has too much uncertainty. Stop mapping and send the open questions to the product owner as homework. Do not pull the story.
- **Many blue (rule) cards** - the story is too big. Propose a slice: pick the rules that form the smallest valuable story, keep them, and write the rest as a new story in the icebox (see the `bacon:story-writing` skill).
- **One rule with many examples** - there are probably several rules hiding in it. Tease them apart.
- **A rule with no example** - the rule is not understood yet. Find one example before moving on.
- **Few blues, one or two greens each, no reds** - the story is ready.

---

## Exit conditions

A well-sized, well-understood story maps in **about 25 minutes**. Stop when:

- The user says confidence is at **95 % or higher** and every rule has at least one example, or
- The map shows the story must be **sliced**, or
- The map shows the story must go back for **homework**.

Ask the user for a thumb-vote: **"Ready to pull this into development?"** Remaining minor questions are fine if the user says they can be resolved while building. Let the user decide.

---

## Delivery

- **Update the story file** in the backlog with the outcome:
  - Each **rule** becomes an acceptance criterion in GIVEN / WHEN / THEN form.
  - Each **example** becomes a `Scenario:` under its rule, or a bullet in the notes if it is not worth a scenario.
  - Each open **question** goes into a **NEEDS PM** or **NEEDS DESIGN** block with the recommended answer.
- If the story was sliced, create the new story via the `bacon:bacon-tracker` skill and cross-reference both.
- Print a short summary: the number of rules, examples and open questions, and the verdict (ready / slice / homework).
- Do **not** move the story to started - that is step 7 of the workflow and the user's call.

---
