# Changelog

All notable changes to this project will be documented here.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

The plugin has no version number on purpose: Claude Code versions it by commit, so every commit on `main` reaches every project with auto-update on. Entries are grouped by date.

## 2026-09-27

First public release, moved from a local `_prefs` folder that projects symlinked into.

### Added

- Plugin `bacon` in marketplace `bybacon`, served from this repo.
- Skills: `bacon-workflow`, `bacon-tracker`, `bacon-review`, `bug-fixing`, `story-writing`, `example-mapping`, `decision-making`, `pull-request`, `tidy`, `create-project-overview-file`, `create-tester-overview-file`, `ror-recipes`, `ror-release-feature`.
- Rules `conventions`, `donts`, `git`, `stories` and `ruby-on-rails`, loaded into every session by a `SessionStart` hook.

### Changed

- Skills refer to each other as `bacon:<skill>` and to rules by name, instead of `bacon-skills/…` and `bacon-rules/…` paths.
- `ruby-on-rails` is always loaded. Path scoping is not available to plugin rules.
- `tidy` checks the plugin setup instead of `_prefs` symlinks.
