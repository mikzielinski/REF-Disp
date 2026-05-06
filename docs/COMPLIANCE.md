# Compliance Notes

## Logging and Data Handling

- Do not log secrets, passwords, API keys, or raw credentials.
- Keep PII out of logs by default; if needed, mask or tokenize values.
- Use structured run logs: `RunId`, `CorrelationId`, `BusinessKey`, `RuntimeMode`, `AIProvider`.
- Keep `excludedLoggedData` in `project.json` aligned with security policy.

## AI Governance

- AI is disabled by default unless `AI.Enabled=true`.
- Use approved provider from `AI.Provider` and keep API keys in Orchestrator Assets.
- Enforce guardrails from config:
  - `AI.TimeoutSec`
  - `AI.CostLimitPerRun`
  - `AI.AllowPII` (default `false`)
  - `AI.FallbackMode`
- If AI config is invalid, fallback to non-AI path and log warning.

## Traceability and Audit

- Every run generates a JSON run report in `Data/Output`.
- Report includes timing, queue metrics, and AI/runtime mode metadata.
- Queue payload should include:
  - `schemaVersion`
  - `sourceSystem`
  - `businessKey`
  - `correlationId`

## Retention and Access

- Define retention period for reports and logs by environment.
- Restrict access to logs/reports to least privilege groups.
- Review and rotate AI/API assets on schedule.
