# SOAR Platform

Automated SSH brute force detection and incident response on AWS serverless architecture.

## How it works

A cron job on Ubuntu servers monitors `/var/log/auth.log` and forwards failed login attempts to AWS API Gateway. Four Lambda functions handle the pipeline in sequence via SQS: ingress validation, event storage in DynamoDB, threat analysis, and email alerting via SNS.

Severity is calculated automatically based on attempt frequency: 3 attempts triggers a Low alert, 15 or more triggers Critical. Average response time from detection to notification is under 30 seconds.

All Lambda functions run in private VPC subnets with no direct internet access. Infrastructure is deployed in 15 minutes via Terraform and GitHub Actions CI/CD.

## Tech stack

| Component | Technology |
|-----------|------------|
| Runtime | Python 3.11 on AWS Lambda |
| Queue | Amazon SQS |
| Database | DynamoDB (35-day retention) |
| Alerting | Amazon SNS |
| Entry point | API Gateway |
| Infrastructure | Terraform + GitHub Actions |

## Setup

```bash
cd terraform
terraform init
terraform apply
```

Requires AWS credentials with permissions for Lambda, DynamoDB, SQS, SNS, API Gateway, and VPC.
