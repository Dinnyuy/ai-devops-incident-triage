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