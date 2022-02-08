# docker-ecs-cicd-test

Repository to test using GitHub Actions to build a simple docker application, push it to ECR, and then deploy to ECS.

Further READMEs are present in the subdirectories.

## Useful resources

Reference architecture from AWS:

- https://github.com/awslabs/ecs-refarch-continuous-deployment

Blog posts:

- Using GitHub actions and Terraform: https://particule.io/en/blog/cicd-ecr-ecs/
- Using CodePipeline: https://ecsworkshop.com/ecsanywhere/workloadmanagement/cicd/

Documentation for GitHub Actions:

- https://github.com/aws-actions/amazon-ecs-render-task-definition
- https://github.com/aws-actions/amazon-ecs-deploy-task-definition
