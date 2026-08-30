# shared-workflows

Reusable GitHub Actions workflows for jtrotter83 repos. A reusable workflow must
live on the default branch of this repo to be callable, so changes here roll out
to all callers once merged to `main`.

## ci-ts.yml — TypeScript / web repos

```yaml
jobs:
  ci:
    uses: jtrotter83/shared-workflows/.github/workflows/ci-ts.yml@main
    with:
      node-version: "22"
      enable-pages-preview: true
      coverage-threshold: 80
```

Runs: Prettier check → ESLint → `tsc --noEmit` → Vitest with line-coverage
threshold → build → `npm audit --audit-level=high` (fails on high/critical).

**Inputs**

| Input                  | Type    | Default | Description                   |
| ---------------------- | ------- | ------- | ----------------------------- |
| `node-version`         | string  | `"22"`  | Node.js version               |
| `enable-pages-preview` | boolean | `false` | Deploy a Pages preview on PRs |
| `coverage-threshold`   | number  | `80`    | Minimum line coverage (%)     |

Note: the repo needs `@vitest/coverage-v8` in devDependencies for coverage.
Pages preview deploys to a `pr-<number>` environment with base path
`/pr-<number>/` and requires Pages to be enabled in repo settings.

## ci-luau.yml — Roblox repos

```yaml
jobs:
  ci:
    uses: jtrotter83/shared-workflows/.github/workflows/ci-luau.yml@main
    with:
      source-dir: src
```

Runs: StyLua format check → Selene lint → Rojo build smoke test (`rojo build -o
build.rbxl` validates the project JSON and tree structure).

**Inputs**

| Input        | Type   | Default | Description                        |
| ------------ | ------ | ------- | ---------------------------------- |
| `source-dir` | string | `"src"` | Directory checked by StyLua/Selene |

## pr-title.yml — conventional commits PR titles

```yaml
jobs:
  pr-title:
    uses: jtrotter83/shared-workflows/.github/workflows/pr-title.yml@main
```

Validates PR titles against
`(feat|fix|docs|style|refactor|test|chore|ci|perf|hotfix)(scope)?!: description`.
Has no inputs.
