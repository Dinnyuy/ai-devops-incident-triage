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