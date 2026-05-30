# Installing the Quota Monitor Cron Job

The skill includes a template for a cron job that runs the quota check every 15 minutes (configurable).

## Manual Installation

1. Copy the config file to your OpenClaw config directory:
   ```bash
   cp skills/llm-quota-monitor/config/example.yaml ~/.openclaw/config/llm-quota-monitor.yaml
   ```
2. Edit the config to set your desired quotas and notification channel.

3. Add a cron job via the OpenClaw gateway (or using `openclaw cron add` if available). Example payload:
   ```json
   {
     "name": "llm-quota-monitor-check",
     "schedule": { "kind": "cron", "expr": "*/15 * * * *" },
     "sessionTarget": "current",
     "payload": {
       "kind": "agentTurn",
       "message": "Run the LLM quota monitor skill: check current token usage against quotas and warn if thresholds are exceeded.",
       "model": "default",
       "thinking": "medium"
     },
     "delivery": { "mode": "announce" }
   }
   ```

## Using agency-operator

If you have the agency-operator skill installed, you can add the cron job via its configuration workflow. Refer to the agency-operator skill for details.

## Verification

After installation, you can check the cron job with:
```
openclaw cron list
```
Look for the job named "llm-quota-monitor-check".

## Notes

- The skill will use the current session's token usage from `session_status`. For accurate daily totals, ensure the skill runs frequently enough to capture usage from all sessions.
- The skill stores usage logs in the memory wiki under `LLM Quota Monitor/`. Ensure the memory agent has permission to create/update pages there.