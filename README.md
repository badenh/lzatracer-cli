# @lzatracer/cli

Compare an AWS Landing Zone Accelerator (LZA) config directory against the LZA UC v1.3.1 baseline — coverage report in terminal, JSON, or HTML.

Part of the [lzatracer](https://lzatracer.net) project — see the site for full methodology, cross-variant comparison, and per-control drilldown.

## Install

Global:

```bash
npm i -g @lzatracer/cli
```

Or run without installing:

```bash
npx @lzatracer/cli compare ./my-lza-config
```

Requires **Node.js 22 or newer** (uses stable ESM + JSON import attributes).

## Quick start

Point `lzatracer compare` at a directory containing your LZA config YAML files:

```bash
lzatracer compare ./my-lza-config
```

The CLI auto-detects the canonical LZA config file set (`accounts-config.yaml`, `network-config.yaml`, `global-config.yaml`, `security-config.yaml`, `organization-config.yaml`, `iam-config.yaml`) and produces a coverage report grouped by capability family (SEC / G / NET / VPC / LOG / TGW / IGW / RAM).

## Output formats

Choose an output format with `--format`:

| Format     | Description                                                                    |
|------------|--------------------------------------------------------------------------------|
| `terminal` | (default) ANSI-color grouped-by-family table. Stdout only. `--out` ignored.    |
| `json`     | Byte-deterministic CoverageBundle JSON. Stdout by default; `--out <file>` optional. |
| `html`     | Self-contained HTML report. `--out <file>` REQUIRED (stdout footgun avoidance). |

Examples:

```bash
# Terminal (default)
lzatracer compare ./my-lza-config

# JSON to stdout (pipe-friendly)
lzatracer compare ./my-lza-config --format json | jq .

# JSON to file
lzatracer compare ./my-lza-config --format json --out report.json

# HTML report (open report.html in a browser)
lzatracer compare ./my-lza-config --format html --out report.html

# No-color terminal (for CI logs)
lzatracer compare ./my-lza-config --no-color
```

Respects `NO_COLOR=1` env var for automatic ANSI-disable.

## Exit codes

| Code | Meaning                                                                   |
|------|---------------------------------------------------------------------------|
| 0    | Clean — no HIGH-severity findings (MEDIUM/LOW may be present)             |
| 1    | HIGH severity — at least one family has measurable > 0 with 0% coverage   |
| 2    | Usage error — `--out` missing with `--format html`, or no LZA config found |

CI usage:

```bash
lzatracer compare ./my-lza-config || echo "coverage gaps"
```

## Baseline

v0.1.0 ships the LZA UC v1.3.1 baseline (frozen at commit `4c4494435a7083b4935766924f3bc622a1880b14`). No network fetch at runtime — the baseline is inlined into the bundle.

Configurable `--baseline <name>` reserved for future expansion (Healthcare LZA, EUSC, etc.). For now, `--baseline uc` is the only value.

## Full methodology

See [lzatracer.net](https://lzatracer.net) for:

- Per-control drilldown with UC fingerprint + variant config evidence + workbook prose
- Cross-variant comparison (LZA UC vs Healthcare LZA vs EUSC vs …)
- Review workflow (curator-reviewed vs auto-inferred verdicts)
- Methodology explainers (fingerprint state model, coverage math, threshold coloring)

## License

[Apache-2.0](./LICENSE). Copyright 2026 Baden Hughes.

## Support

Issue tracker: [github.com/badenh/lzatracer/issues](https://github.com/badenh/lzatracer/issues).

Source (monorepo): [github.com/badenh/lzatracer](https://github.com/badenh/lzatracer). Note that this npm package is published from a separate artifact repo at [github.com/badenh/lzatracer-cli](https://github.com/badenh/lzatracer-cli) — file issues against the main monorepo, not the artifact repo.
