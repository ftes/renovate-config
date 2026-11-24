# renovate-config

Shared [Renovate](https://docs.renovatebot.com) presets for ftes's repos. A repo's `renovate.json`
extends one of them and keeps only what is its own:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>ftes/renovate-config:elixir-app"]
}
```

| Preset | For |
|---|---|
| `github>ftes/renovate-config` | every repo: pinned digests, weekly PRs on Monday night, ftes/ci releases merged once green |
| `github>ftes/renovate-config:elixir` | Elixir repos: toolchain in `.tool-versions` or `mise.toml`, Hex, esbuild and Tailwind |
| `github>ftes/renovate-config:elixir-lib` | Hex packages: wider ranges, so users' versions stay supported |
| `github>ftes/renovate-config:elixir-app` | deployed apps: dev deps by range, CI's Postgres on production's major |

Renovate reads these from `main` on every run, without a PR in the repos that use them: a change
here applies everywhere at once, so CI validates it first.

When production's Postgres moves to a new major (ftes/infra), raise `allowedVersions` in
`elixir-app.json`. When the repos move to a new Erlang major, raise `constraints` in
`elixir.json` (a repo can override it while it catches up).
