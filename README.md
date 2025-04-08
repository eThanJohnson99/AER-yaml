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
│       │   ├── dev-app-alb.yaml
│       │   └── dev-api-alb.yaml
│       ├── prod/
│       │   ├── prod-app-alb.yaml
│       │   ├── prod-api-alb.yaml
│       │   └── prod-backend-alb.yaml
│       └── test/
│           ├── test-app-alb.yaml
│           └── test-api-alb.yaml
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
export SECURITY_GROUP_ID="sg-1234567890abcdef0"
export ALLOWED_CIDR_BLOCKS="10.0.0.0/16,172.16.0.0/16"
export COST_CENTER="123456"
export WAF_WEB_ACL_ID="3ab78708-85b0-49d3-b4e1-7a9615a6613b"
```

3. Apply a template using `envsubst` and `kubectl`:

```bash
# Process an NLB template and replace variables
envsubst < ELB/NLB/dev/dev-app-nlb.yaml | kubectl apply -f -

# Process an ALB template and replace variables
envsubst < ELB/ALB/dev/dev-app-alb.yaml | kubectl apply -f -
```

4. Verify the resource creation:

```bash
# For NLB (Service resources)
kubectl get services -n dev-env

# For ALB (Ingress resources)
kubectl get ingress -n dev-env
```

## Template Types

### Network Load Balancer (NLB) Templates

- Implemented as Kubernetes Service resources
- Use `service.beta.kubernetes.io/aws-load-balancer-*` annotations
- Suitable for TCP/UDP traffic, high throughput requirements
- Options for internal or external exposure

### Application Load Balancer (ALB) Templates

- Implemented as Kubernetes Ingress resources
- Use `alb.ingress.kubernetes.io/*` annotations
- Suitable for HTTP/HTTPS traffic with path-based routing
- Support for WAF integration, SSL termination, and authentication

## Template Customization

Each template is configured for its specific environment and use case. Common customization points:

- Health check paths and ports
- TLS configuration
- Load balancer attributes
- Resource tagging
- Security settings (WAF, security groups)
- Logging configuration

Review the AWS Load Balancer Controller documentation for all available options:

- [NLB Annotations](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/service/annotations/)
- [ALB Annotations](https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/ingress/annotations/)

## Environment Configurations

### Test Environment

- Internal load balancers
- Basic configuration for testing
- Minimal resource settings

### Development Environment

- External load balancers with SSL
- HTTP to HTTPS redirection
- Access logs enabled

### Production Environment

- Enhanced security with WAF and Shield Advanced
- Deletion protection
- Cross-zone load balancing
- Capacity units reservation
- Advanced security settings

## License

This project is licensed under the terms of the included LICENSE file.
