# AI-Assisted Serverless Incident Triage on AWS

This project demonstrates a controlled Lambda failure, CloudWatch detection, AI-assisted investigation, safe remediation, and recovery verification.

## Enterprise problem

Operations teams lose time when malformed application events fail without a repeatable detection and investigation path. This lab creates that path as a small, reproducible case study.

## Architecture

```text
Test event
    |
    v
AWS Lambda Event Function
    |                \
    v                 v
CloudWatch Logs   Lambda Errors metric
                          |
                          v
                  CloudWatch alarm
                          |
                          v
               CloudWatch investigation
```
## Incident lifecycle

1. Deploy the CloudFormation stack with `ValidationMode=unsafe`.
2. Prove the healthy checkout event works.
3. Send the malformed event three times.
4. Correlate the `validation_failed` log, `ValueError`, and alarm.
5. Use CloudWatch investigations to review observations or hypotheses.
6. Redeploy with `ValidationMode=safe`.
7. Re-run the same malformed event and verify a controlled `400` response.

## Deployment

```text
aws cloudformation deploy --template-file template.yaml --stack-name ai-devops-incident-triage --parameter-overrides ValidationMode=unsafe --capabilities CAPABILITY_IAM --region us-east-1
```

## Remediation

```text
aws cloudformation deploy --template-file template.yaml --stack-name ai-devops-incident-triage --parameter-overrides ValidationMode=safe --capabilities CAPABILITY_IAM --region us-east-1
```
## Evidence

See `incident-report.md` for the detection signal, direct logs, AI-assisted findings, remediation, and recovery checks. Redact account identifiers and credentials from every screenshot.

## Cost and cleanup

The lab is designed for AWS Free Tier-scale usage. Related API and telemetry usage can incur charges beyond applicable allowances.

```text
aws cloudformation delete-stack --stack-name ai-devops-incident-triage --region us-east-1
aws cloudformation wait stack-delete-complete --stack-name ai-devops-incident-triage --region us-east-1
```