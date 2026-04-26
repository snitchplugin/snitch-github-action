# Snitch GitHub Action

Security review on every pull request. Free with GitHub Models, or bring your own AI provider key. Sticky PR comment with findings, SARIF upload to GitHub Code Scanning, optional merge gate.

## Quick start

```yaml
name: Snitch Security Scan

on:
  pull_request:
    types: [opened, synchronize, reopened]

permissions:
  contents: read
  pull-requests: write
  security-events: write
  statuses: write
  models: read   # free GitHub Models inference via the auto-issued GITHUB_TOKEN

jobs:
  snitch:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: snitchplugin/snitch-github-action@v1
        with:
          snitch-license-key: ${{ secrets.SNITCH_LICENSE_KEY }}
```

## Subscribe

Subscriptions and license keys live at https://snitchplugin.com/action.

## Inputs

See `action.yml`. The only required input is `snitch-license-key`. The default AI path uses GitHub Models via the runner's `GITHUB_TOKEN` (free tier ~50 req/day per account). To pick a specific model or use a paid provider, supply one of:

- `openrouter-api-key` — pay-per-token across every model
- `anthropic-api-key`
- `openai-api-key`
- `google-api-key`
- `copilot-token` — Copilot subscription token (overrides the auto-issued GITHUB_TOKEN)

Optional:

- `model` — provider-relative model id or alias (`sonnet`, `opus`, `gpt-4o`, `gemini`)
- `provider` — force a specific provider when multiple keys are present
- `trigger-mode` — `smart` | `always` | `manual` (default `smart`)
- `fail-on` — `critical` | `high` | `medium` | `low` | `none` (default `high`)
- `categories` — comma-separated category numbers, e.g. `1,2,5`
- `paths` — comma-separated glob filters
- `sarif-output-path` — where to write the SARIF report (default `SECURITY_AUDIT_REPORT.sarif`)

## Outputs

- `findings-count`, `critical-count`, `high-count`
- `report-url` — link to the sticky PR comment
- `scan-id` — Snitch scan id for dashboard correlation
- `provider-used`, `model-used`

## Per-repo configuration

Drop a `.snitch.yml` at the root of any repo to override defaults: trigger mode, fail-on threshold, path filters, category list, etc. Per-repo config wins over Action inputs.

## Privacy

The Action runs on your runner. Your source code never enters Snitch infrastructure. The only fields the Action sends to snitchplugin.com are:

- License-key validation
- Methodology download (the 68-category ruleset)
- Per-scan counters: file count, finding counts, duration

No repo name, PR number, or branch is sent or stored.

The AI inference path (GitHub Models, OpenRouter, etc.) does see the prompt; that's an unavoidable trade for any cloud-based AI scan. Snitch's own servers do not.

## License

Business Source License 1.1. Production use requires a current Snitch subscription. Reading and evaluation are permitted without one. Converts to Apache 2.0 four years after each release. See `LICENSE` (in the published mirror) or https://snitchplugin.com.

## Support

eric.waters@snitchplugin.com
