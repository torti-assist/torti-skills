---
name: llm-quota-monitor
description: "Monitor LLM token usage per model, enforce quotas, and suggest optimal model based on remaining quota."
---

# LLM Quota Monitor

This skill tracks token consumption for each LLM model used by OpenClaw, compares against configurable quotas (daily, hourly, monthly), and recommends the model with the most remaining quota for upcoming calls.

## How It Works

1. **Configuration**: Define quotas per model in `config.yaml` (see below).
2. **Collection**: The skill reads the current session's token usage via `session_status` and accumulates it over time.
3. **Storage**: Usage logs are stored in the memory wiki under `LLM Quota Monitor/` as daily JSON files.
4. **Reporting**: When quotas are near exhaustion (configurable threshold, default 80%), the skill posts a warning to the configured support channel (e.g., #support).
5. **Recommendation**: The skill can be invoked to suggest the best model to use next based on remaining quota.

## Workflow

### Setup
1. Copy the example config to your OpenClaw config directory:
   ```bash
   cp skills/llm-quota-monitor/config/example.yaml ~/.openclaw/config/llm-quota-monitor.yaml
   ```
2. Edit the config to set quotas per model and notification channel.

### Usage
- **Automatic**: The skill runs via a cron job (every 15 minutes) to update usage and check quotas.
- **Manual**: Run `llm-quota-monitor check` to see current usage and remaining quota.
- **Manual**: Run `llm-quota-monitor suggest` to get the model with the most remaining quota.

## Configuration

Create `~/.openclaw/config/llm-quota-monitor.yaml` with:

```yaml
# Quotas are in tokens
quotas:
  openrouter/nvidia/nemotron-3-super-120b-a12b:free:
    daily: 500000
    hourly: 50000
    monthly: 10000000
  google/gemini-3.1-flash-lite-preview:
    daily: 1000000
    hourly: 100000
    monthly: 20000000
  github-copilot/claude-haiku-4.5:
    daily: 800000
    hourly: 80000
    monthly: 15000000
  # Add other models as needed

# Threshold percentage at which to warn (0.0-1.0)
warning_threshold: 0.8

# Where to send quota warnings (Discord channel ID or name)
notification_channel: "#support"

# How often to run the check (cron expression, default every 15 minutes)
check_cron: "*/15 * * * *"
```

## Data Storage

Usage is stored in the memory wiki at:
`LLM Quota Monitor/YYYY-MM-DD.json`

Each file contains:
```json
{
  "date": "2026-05-30",
  "usage": {
    "openrouter/nvidia/nemotron-3-super-120b-a12b:free": 124578,
    "google/gemini-3.1-flash-lite-preview": 0,
    ...
  }
}
```

## Automation

A cron job is automatically installed (via `agency-operator` or manual setup) that runs the skill's check script on the schedule defined in config.

## Scripts

- `check.py`: Collects current session usage, updates daily logs, checks quotas, sends warnings.
- `suggest.py`: Reads logs and quotas, returns model with highest remaining quota.
- `status.py`: Prints a formatted table of usage vs quotas.

## References

- See `scripts/README.md` for detailed script usage.
- See `assets/example.yaml` for a full configuration example.

## Extending

To add a new model, add it to the quotas config and ensure the model name matches exactly what appears in `session_status` output.