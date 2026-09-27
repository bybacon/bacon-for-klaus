---
name: bacon-workflow
description: Our mandatory workflow for feature implementation. Use when working on new stories including features, chores and bugs.
---

# Our mandatory workflow

> [!IMPORTANT]  
> No cutting corners!

1. Create a dedicated branch (see the `git` rule)
2. Write the story if it does not exist already (the `bacon:story-writing` skill)
3. Pull the story from the backlog (verify again with the `bacon:story-writing` skill)
4. Refine and ask questions until 95% confidence is reached (work with the `bacon:example-mapping` skill)
5. Update the story document inside the backlog so we have a clear reference for later
6. Take and document impactful decisions (see the `bacon:decision-making` skill)
7. Move the story to **started** (`/tracker start <ID>`)
8. Create the feature and its step definitions **first**, specs **second**, implementation **third** (e.g. the `bacon:ror-recipes` skill)
9. Perform a partial Code Review (see the `bacon:bacon-review` skill)
10. Fix the findings (see the `bacon:bug-fixing` skill)
11. Move the story to **done** (`/tracker done <ID>`)
12. Create the pull request (the `bacon:pull-request` skill)
