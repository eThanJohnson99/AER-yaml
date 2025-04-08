# AWS EKS Resource Configuration Templates

A collection of YAML configuration templates for managing AWS EKS resources such as ELB (Load Balancers), EBS (Block Storage), EFS (File System), and more. These templates are organized by environment (test, development, production) and resource type to provide consistent configuration across your Kubernetes clusters.

## Repository Structure

```
AER-yaml/
├── ELB/
│   ├── NLB/
│   │   ├── dev/
│   │   │   ├── dev-app-nlb.yaml
│   │   │   └── dev-api-nlb.yaml
│   │   ├── prod/
│   │   │   ├── prod-app-nlb.yaml
│   │   │   ├── prod-api-nlb.yaml
│   │   │   └── prod-backend-nlb.yaml
│   │   └── test/
│   │       ├── test-app-nlb.yaml
│   │       └── test-api-nlb.yaml
│   └── ALB/
│       ├── dev/
│       ├── prod/
│       └── test/
├── EBS/
│   ├── dev/
│   ├── prod/
│   └── test/
├── EFS/
│   ├── dev/
│   ├── prod/
│   └── test/
└── ... (other AWS resources)
```

## Prerequisites

Before using these templates, you need:

1. A running AWS EKS cluster
2. `kubectl` configured to communicate with your cluster
3. Proper IAM permissions for creating/managing the resources
4. Environment variables set (see below)

## Environment Variables

These templates contain placeholders that need to be replaced with your specific values. Set the following environment variables before applying the templates:

| Variable                | Description                                 | Example                                |
| ----------------------- | ------------------------------------------- | -------------------------------------- |
| `AWS_REGION`            | AWS region where resources will be created  | `us-east-1`                            |
| `AWS_ACCOUNT_ID`        | Your AWS account ID                         | `123456789012`                         |
| `CERTIFICATE_ID`        | SSL certificate ID for HTTPS listeners      | `abcdef1234567890`                     |
| `S3_LOGS_BUCKET_PREFIX` | Prefix for S3 buckets storing logs          | `my-company`                           |
| `SECURITY_GROUP_ID`     | Security group ID for restricted access     | `sg-1234567890abcdef0`                 |
| `ALLOWED_CIDR_BLOCKS`   | CIDR blocks allowed to access the resources | `10.0.0.0/16,172.16.0.0/16`            |
| `COST_CENTER`           | Cost center tag for billing                 | `123456`                               |
| `WAF_WEB_ACL_ID`        | Web ACL ID for WAF protection               | `3ab78708-85b0-49d3-b4e1-7a9615a6613b` |

## Usage

1. Clone this repository:

```bash
git clone https://github.com/yourusername/AER-yaml.git
cd AER-yaml
```

2. Set your environment variables:

```bash
export AWS_REGION="us-west-2"
export AWS_ACCOUNT_ID="123456789012"
export CERTIFICATE_ID="abcdef1234567890"
export S3_LOGS_BUCKET_PREFIX="my-company"
# Set additional variables as needed
```

3. Apply a template using `envsubst` and `kubectl`:

```bash
# Process the template and replace variables
envsubst < ELB/NLB/dev/dev-app-nlb.yaml | kubectl apply -f -
```

4. Verify the resource creation:

```bash
kubectl get services -n dev-env
```

## Template Customization

Each template is configured for its specific environment and use case. Common customization points:

- Health check paths and ports
- TLS configuration
- Load balancer attributes
- Resource tagging

Review the AWS Load Balancer Controller documentation for all available options and annotations: [AWS Load Balancer Controller Documentation](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)

## Contributing

1. Create new resource templates in the appropriate directory
2. Follow the naming convention: `{env}-{resource-type}-{resource-subtype}.yaml`
3. Use variables for values that change between environments or deployments
4. Add appropriate comments and documentation

## License

This project is licensed under the terms of the included LICENSE file.
