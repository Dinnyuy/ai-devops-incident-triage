# Incident Report: Missing Customer Identifier

## Summary

A controlled checkout event omitted `customer_id`. With `ValidationMode=unsafe`, the Lambda handler raised `ValueError`, produced an `Errors` metric, and caused the `ai-devops-lambda-errors` alarm to enter `ALARM`.

## Scope and impact

- Environment: Personal AWS learning account in `us-east-1`
- Workload: `ai-devops-incident-triage`
- Impact: Test invocations failed. No production traffic or customer data was used.

## Detection evidence

- Alarm: `ai-devops-lambda-errors`
- Application log event: `validation_failed`
- Runtime error: `ValueError: customer_id is required`
- Incident timestamp: Add the UTC timestamp from your alarm or logs.
- Evidence image: Add a redacted alarm or log screenshot.

## AI-assisted findings

Record the observation, suggestion, or hypothesis produced by CloudWatch investigations. State whether direct logs support it, and note any uncertainty instead of presenting AI output as fact.

## Root cause

The malformed event did not contain `customer_id`. Unsafe validation mode raised an exception instead of returning a controlled client-error response.

## Remediation

The same CloudFormation stack was redeployed with `ValidationMode=safe`. The handler then returned `statusCode: 400` for the unchanged malformed event without raising another exception.

## Recovery validation

- The remediated invocation had no CLI `FunctionError`.
- `artifacts/remediated-response.json` contained a controlled `400` response.
- No new unhandled exception appeared in the tailed logs.
- The CloudWatch alarm returned to `OK` after evaluation.

## Preventive actions

- Validate required fields before business logic runs.
- Add contract tests for malformed events.
- Keep validation mode safe by default.
- Review deployment parameter overrides during release approval.