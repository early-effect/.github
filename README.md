# early-effect org workflows

Reusable GitHub Actions for the [early-effect](https://github.com/early-effect) organization.

## Workflows

| Workflow | Trigger | Purpose |
|----------|---------|---------|
| [`specular-docs.yml`](.github/workflows/specular-docs.yml) | `workflow_call` | Build a Specular site (`sbt <project>/specularSite`) and deploy to GitHub Pages project site |
| [`hub-site.yml`](.github/workflows/hub-site.yml) | `workflow_dispatch` | Rebuild [earlyeffect.rocks](https://www.earlyeffect.rocks) from published `metadata.json` URLs |

## Project docs (caller)

In each library repo, add a thin workflow on `v*` tags:

```yaml
name: Docs

on:
  push:
    tags:
      - 'v*'

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    uses: early-effect/.github/.github/workflows/specular-docs.yml@main
    with:
      sbt-project: docs
```

Enable **GitHub Pages** → Source: **GitHub Actions** on the repo.

Docs land at `https://early-effect.github.io/<repo>/`.

## Org hub

The live hub is [`early-effect.github.io`](https://github.com/early-effect/early-effect.github.io)
([earlyeffect.rocks](https://www.earlyeffect.rocks)).

1. Add a `metadata.json` URL to `catalog-urls.txt` in that repo
2. Run **Actions → Hub site → Run workflow** on `early-effect.github.io`

(A stub `hub-site` workflow here prints the link so the org `.github` repo stays the docs index.)
