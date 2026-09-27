---
name: pull-request
description: Create the pull request for a finished story. Runs the pre-PR checklist (story done, version bumped, changelog, overview docs, manual-testing impact, green CI, squashed and rebased) and writes the PR description from the template. Use when a story is done and ready to go back onto main.
---

# Pull request

## Before creating the PR
- Story moved to done and removed from `backlog.md`
- Version number bumped
- Changelog updated
- Project overview doc updated (or confirmed no screen/ flow/ data changes)
- Testing overview: the story's feature file records the behaviour. Update the repo's dated overview in `docs/overviews/` only if the PR changes a surface automation cannot reach (real mail, real dispatches, real DNS, deployed-host behaviour, browser rendering, real secrets) - otherwise say so
- All acceptance tests, specs and linters are green
- Commits squashed
- Feature branch rebased with `main`

## PR Format template

```
Story: [story# story-title](paste Story link here)

- [ ] Yes, I have moved the story to done and updated backlog.md.
- [ ] Yes, I have bumped the version number.
- [ ] Yes, I have updated the changelog.
- [ ] Yes, I have updated the project overview doc (or no screen/flow/data changes).
- [ ] Yes, I have checked the manual-testing impact (feature file records the behaviour; the dated overview changed only if this PR touches a surface automation cannot reach).

Changes proposed in this pull request:

- [item 1 - replace me]
- [item 2 - replace me]

What I have learned working on this feature:
[If you don't put anything here you are doing it wrong!]

- [item 1 - replace me]
- [item 2 - replace me]

Screenshots:
[If you made some visual changes to the application please upload screenshots here, or remove this section]
```
