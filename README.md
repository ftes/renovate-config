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
| `github>ftes/renovate-config` | every repo: pinned digests, weekly PRs early Monday morning (Berlin), ftes/ci releases merged once green, in hourly waves of repos |
| `github>ftes/renovate-config:elixir` | Elixir repos: toolchain in `.tool-versions` or `mise.toml`, Hex, esbuild and Tailwind |
| `github>ftes/renovate-config:elixir-lib` | Hex packages: wider ranges, so users' versions stay supported |
| `github>ftes/renovate-config:elixir-app` | deployed apps: dev deps by range, CI's Postgres on production's major |

Renovate reads these from `main` on every run, without a PR in the repos that use them: a change
here applies everywhere at once, so CI validates it first.

Each Elixir repo keeps one setting itself, the Erlang major Renovate builds `mix.lock` with, so it
changes in the same commit as the repo's toolchain pin:

```json
{
  "extends": ["github>ftes/renovate-config:elixir-app"],
  "constraints": { "erlang": "29" }
}
```

Elixir is pinned with its OTP suffix (`1.20.5-otp-29`): the versioning requires it and keeps it
as a compatibility marker, so Renovate never moves it. Raise it with `constraints` on an Erlang
major bump; that PR says so.

When production's Postgres moves to a new major (ftes/infra), raise `allowedVersions` in
`elixir-app.json`.
