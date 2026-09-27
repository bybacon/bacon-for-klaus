---
name: ror-release-feature
description: Feature release guideline. Use when pushing a feature to the `main` branch.
---

# Ruby on Rails - Release Feature

## Release creation

- Make sure credentials are created for each environment, e.g. `VISUAL=nano EDITOR=nano bin/rails credentials:edit --environment=production`
- Bump the current app version:
- ```
    bundle exec rake version:write[version]   # set custom version in the x.x.x-? format
    bundle exec rake version:patch            # increment the patch x.x.x+1 (keeps any flags on)
    bundle exec rake version:minor            # increment minor and reset patch x.x+1.0 (keeps any flags on)
    bundle exec rake version:major            # increment major and reset others x+1.0.0 (keeps any flags on)
    bundle exec rake version:dev              # set the dev flag on x.x.x-dev
    bundle exec rake version:beta            # set the beta flag on x.x.x-beta
    bundle exec rake version:rc               # set or increment the rc flag x.x.x-rcX
    bundle exec rake version:release          # removes any flags from the current version
```
- Update the `Changelog.md` file
- Run rubocop: `bundle exec rubocop` / `bundle exec rubocop -a` / `bundle exec rubocop -A`
- Squash the commits
- Push your code: `bundle exec rake push` - This will run specs and acceptance tests first then push only if nothing failed.
- Create a PR
- After the PR is reviewed and merged, a GitHub Action builds and pushes the image to GHCR and deploys to staging automatically
