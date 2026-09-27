# Bacon for Klaus

The bacon way of building software with Klaus, packaged as a plugin: a story-driven BDD workflow on top of [bacon-tracker](https://github.com/bybacon/bacon-tracker), example mapping, ADRs, structured reviews and bug fixing, and Ruby on Rails conventions.

Klaus is the name Claude goes by in commits and conversations.

## What's inside

| Skill | Use it to |
|---|---|
| `bacon:bacon-workflow` | Take a story from backlog to pull request, step by step |
| `bacon:bacon-tracker` | Create, move and list stories, bugs and chores |
| `bacon:story-writing` | Write a feature, bug or chore story from the templates |
| `bacon:example-mapping` | Clarify a story's rules and examples before starting it |
| `bacon:decision-making` | Record architecture decisions (ADRs) |
| `bacon:bacon-review` | Review code and turn findings into chores |
| `bacon:bug-fixing` | Reproduce, hypothesise, regression-test and fix a bug |
| `bacon:pull-request` | Run the pre-PR checklist and write the description |
| `bacon:tidy` | Sweep docs, tracker, branches and conventions |
| `bacon:create-project-overview-file` | Keep `docs/overviews/project_overview.md` current |
| `bacon:create-tester-overview-file` | Write a manual testing guide for a session |
| `bacon:ror-recipes` | Follow Rails walkthroughs: features, generators, i18n, upgrades |
| `bacon:ror-release-feature` | Release a Rails feature to `main` |

The files in `rules/` (conventions, don'ts, git, stories, Ruby on Rails) are loaded into every session by a `SessionStart` hook.

## Use it in a project

Commit this to the project's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "bybacon": { "source": { "source": "github", "repo": "bybacon/bacon-for-klaus" } }
  },
  "enabledPlugins": { "bacon@bybacon": true }
}
```

Once per machine:

1. Trust the project folder when Claude Code asks, then run `/plugin install bacon@bybacon` if it is not installed yet.
2. Turn on updates: `/plugin` → **Marketplaces** → `bybacon` → **Enable auto-update**. Without it, you stay on the commit you installed.

Or install it for all your projects: `claude plugin marketplace add bybacon/bacon-for-klaus` and `claude plugin install bacon@bybacon`.

Project-specific skills and rules stay in the project's own `.claude/` as usual.

## Develop

```bash
claude --plugin-dir /path/to/bacon-for-klaus   # try local changes without pushing
claude plugin validate .                       # check the manifests
```

There is no version field: Claude Code versions the plugin by commit, so everything merged to `main` ships. Add a dated entry to `CHANGELOG.md` with each change.

## License

MIT, see [LICENSE](LICENSE).
