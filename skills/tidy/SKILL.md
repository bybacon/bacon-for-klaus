---
name: tidy
description: Housekeeping sweep. Checks for outdated docs, misplaced files, stale tracker state, and stale branches. Use when something feels out of date or after a batch of stories land.
---

# Tidy

## Docs audit
- Check overview docs (`docs/overviews/project_overview.md`, the latest `docs/overviews/testing-overview-YYYY-MM-DD.md`, or equivalent) against the current codebase - screens, flows, data models, and known limitations should reflect what is actually in the code
- Verify version numbers in docs match the project files (build number, marketing version, etc.)
- Check the changelog - is the `[Unreleased]` section accurate? Are fixed bugs and shipped features recorded?
- Flag anything that is stale, missing, or contradicted by the code - do not silently skip

## Tracker hygiene
- Verify all stories in `3_started` (or equivalent in-flight stage) have been moved to `4_done` if the work is merged
- Verify backlog links actually point to existing files - remove or fix broken links
- Check that the backlog is stack-ranked and contains only actionable, current work - move anything that no longer applies to the icebox or delete it
- Flag any story that has been in `3_started` for a long time without a corresponding PR
- Run the tracker lint action

## Branch cleanup
- List branches that have been merged into the main branch and have not been deleted
- List branches that appear stale (no commits in a long while, no open PR)
- Do not delete branches - report them and ask for confirmation first

## Conventions check
- Scan for files that are in the wrong directory according to the project's conventions (e.g. specs not in `spec/`, views with business logic, interactors without a `Fake`)
- Check for hardcoded strings, force-unwraps, or other violations noted in the project's `donts.md` or equivalent that may have slipped in
- Note but do not fix - violations found here become chore stories (see `story-writing`)

## Bacon plugin
- Shared skills and rules come from the `bacon` plugin ([bybacon/bacon-for-klaus](https://github.com/bybacon/bacon-for-klaus)), not from files in this repo
- Verify `.claude/settings.json` has the `bybacon` marketplace under `extraKnownMarketplaces` and `"bacon@bybacon": true` under `enabledPlugins`
- Flag any copy of a plugin skill or rule inside `.claude/skills/` or `.claude/rules/` - it duplicates the plugin and drifts
- Flag symlinks in `.claude/` pointing outside the repo - they only resolve on one machine
- `.claude/commands/tracker.md` is a real file installed by `tracker-init --command` from the bacon-tracker gem - if it is missing or older than the gem copy, reinstall it
- Project-specific skills and rules stay as real files in `.claude/` - leave them alone
- Read your skills and rules to refresh context

## Report
- Summarise findings in sections matching the checks above
- Use ✅ for clean, ⚠️ for needs attention, ❌ for broken
- End with a recommended priority order for addressing findings
