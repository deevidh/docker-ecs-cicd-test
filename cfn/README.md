# Cloudformation templates

- `vpc.yaml` Creates a VPC with two public subnets
- `ecs-cluster.yaml` Creates an ECS cluster and supporting resources (ECR, security groups, IAM roles, ALB)
- `ecs-service.yaml` Creates an ECS service. This depends on the task definition having been created by the CI/CD pipeline.

## Deployment

You can deploy the CFN using the AWS CLI as follows:

```bash
cd cfn/
aws cloudformation deploy --template-file vpc.yaml --stack-name hello-world-vpc --parameter-overrides file://parameters.json
aws cloudformation deploy --template-file ecs-cluster.yaml --stack-name hello-world-ecs-cluster --parameter-overrides file://parameters.json --capabilities CAPABILITY_NAMED_IAM
```

Next you must create the task definition through some other means (eg CI/CD pipeline)

```bash
aws cloudformation deploy --template-file ecs-service.yaml --stack-name hello-world-ecs-service --parameter-overrides file://parameters.json
```
