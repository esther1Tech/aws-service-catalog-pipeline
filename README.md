# aws-service-catalog-pipeline

A copy of an AWS sample, **Pipeline to Service Catalog**, kept here for study. It manages AWS Service Catalog portfolios and products as code in version control and delivers changes through AWS CodePipeline, so the catalog is updated by a commit instead of by hand in the console.

> Based on an AWS sample (Copyright Amazon.com, Inc. or its affiliates). The original licence is kept in [`aws-pipeline-to-service-catalog/LICENSE`](aws-pipeline-to-service-catalog/LICENSE). Personal learning copy.

## What it does

1. A `mapping.yaml` file describes the catalog: portfolios, their owners and permissions, and the products inside them.
2. Each product is a CloudFormation template stored in `portfolio-infrastructure/` (a VPC with NAT gateways and endpoints, and a sample DynamoDB table).
3. A CodePipeline watches the repository. On a change, a Lambda function (`scripts/sync-catalog.py`, using `boto3`) reads `mapping.yaml` and creates or updates the portfolios, products and shares in Service Catalog.

## What's in the repo

| Path | Purpose |
|---|---|
| `aws-pipeline-to-service-catalog/pipeline-to-service-catalog.yaml` | CloudFormation template that creates the pipeline |
| `aws-pipeline-to-service-catalog/lambda-cloudformation.yaml` | Template for the sync Lambda function |
| `aws-pipeline-to-service-catalog/portfolio-infrastructure/` | `mapping.yaml` plus the product templates (VPC, DynamoDB) |
| `aws-pipeline-to-service-catalog/scripts/sync-catalog.py` | The sync logic |
| `aws-pipeline-to-service-catalog/bin/deploy.sh` | Helper script to deploy the stack |
| `aws-pipeline-to-service-catalog/images/` | Architecture diagrams |

The full description of the `mapping.yaml` properties is in [`aws-pipeline-to-service-catalog/README.md`](aws-pipeline-to-service-catalog/README.md).

## Notes

- Check the owner e-mail values in `mapping.yaml` (they are `EMAIL@EXAMPLE.COM` placeholders) before you deploy.
- Deploying creates real AWS resources. Review the templates and the cost first.
