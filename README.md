# Repo Wrapped Release Recap

Append an AI-narrated **"what shipped" recap** to every GitHub Release — a
short changelog grouped by area (Features / Fixes / …), each commit narrated
in plain English. One workflow file, nothing to configure, no tokens to
create.

Powered by the public recap endpoint at
[repowrapped.com](https://repowrapped.com) — try it without the Action:

```
curl https://repowrapped.com/gh/react/react/recap.md
```

## Usage

```yaml
# .github/workflows/release-recap.yml
name: Release recap
on:
  release:
    types: [published]
permissions:
  contents: write
jobs:
  recap:
    runs-on: ubuntu-latest
    steps:
      - uses: PouriaH/repo-wrapped-action@v1
```

That's it. When you publish a release, the Action fetches a recap of the
commits between your previous tag and this one and **appends** it to the
release notes.

## Guarantees

- **Appends, never replaces** your release body.
- **No-ops cleanly on any failure** (rate limit, unknown repo, no tag range,
  endpoint down) — it exits 0 with a `::notice::`, so it can never break your
  release pipeline.
- **All writes use the workflow's own `github.token`** in _your_ repo, under
  the permissions _you_ granted (`contents: write`). Repo Wrapped receives no
  token and holds no write access to anything.
- **Public repos only.** The recap endpoint is anonymous and serves public
  repositories; a private repo simply no-ops.
- **No fresh AI on your release path.** Commit narrations come from Repo
  Wrapped's cache; commits without one fall back to their raw title, so the
  recap is always complete and always fast.

## Options

None in v1 — the endpoint defaults do the right thing on a
`release: published` event (`to` = the released tag, `from` = the tag before
it). To preview a specific range:

```
https://repowrapped.com/gh/{owner}/{repo}/recap.md?from=v1.2.0&to=v1.3.0
```

## License

MIT
