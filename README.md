# 🦭 PromptSeal Action

**GitHub Action that fails your build when prompt/model behavior regresses.**

Runs [`promptseal ci`](https://github.com/ArsinShaabani/promptseal) against your eval
suite: executes every case, diffs against your sealed baseline, writes a markdown
report to the PR summary, and exits non-zero on regressions.

Persian docs: **[README.fa.md](README.fa.md)**

## Quick start

```yaml
name: PromptSeal
on: [pull_request]

jobs:
  seal:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ArsinShaabani/promptseal-action@v1
        with:
          provider: openai:gpt-4o-mini
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

Your repo needs a `promptseal.yaml` + `cases/` suite and a sealed baseline
(see the [tutorial](https://github.com/ArsinShaabani/promptseal/blob/main/TUTORIAL.md)).

## Inputs

| Input | Default | Description |
|---|---|---|
| `provider` | *(config default)* | Provider spec, e.g. `openai:gpt-4o` or `ollama:llama3.1:8b` |
| `min_pass_rate` | *(config)* | e.g. `0.95` — pass rate below this fails the build |
| `install_from` | `git` | `pypi` after the PyPI release, or `git` for latest main |
| `python_version` | `3.12` | Runner Python version |
| `working_directory` | `.` | Where `promptseal.yaml` lives (monorepos) |

Pass your provider key through `env` (`OPENAI_API_KEY`, `OPENROUTER_API_KEY`, …).
Ollama/vLLM self-hosted runners need no key.

## What the PR looks like

The action posts a step summary:

```markdown
## 🦭 PromptSeal report
| Metric | Value |
|---|---|
| Provider | `openai:gpt-4o` |
| Pass rate | **93%** (14/15) |

**Verdict vs baseline:** ❌ REGRESSION (1 regressions, 2 improvements)

| Case | Baseline | Candidate |
|---|---|---|
| ❌ pii-guard | pass | **fail** |
```

## How baselines work

Seal locally on `main` after every reviewed change:

```bash
promptseal seal -p openai:gpt-4o
```

Then PRs are measured against that behavior. Experimental suites can relax the gate:

```yaml
- uses: ArsinShaabani/promptseal-action@v1
  with:
    min_pass_rate: '0.95'
```

## License

MIT © [Arsin Shaabani](https://github.com/ArsinShaabani)
