# docker-ecs-cicd-test

Repository to test using GitHub Actions to build a simple docker application, push it to ECR, and then deploy to ECS.

Some further documentation can be found in the subdirectories.

## GitHub Actions workflows

### Build and Deploy to Amazon ECS

- Any commits made to the `staging` branch will trigger a build and the application will be immediately deployed to the staging environment.
- Any commits made to the `main` branch will trigger a build and the application will be immediately deployed to the prod environment.
- Branch protection rules are in place so that commits can only be made to the master branch via a pull request. Status checks are required to pass before merging, so this means the only way to deploy to Prod is by building and deploying in staging, and then completing a pull request to the main branch.
- See the comments in the workflow file for more info.

### Manual deploy existing image to Amazon ECS

- This workflow can be triggered manually, and allows a previously built commit (ie container image) to be redeployed to the ECS cluster. This is useful for rolling back.
- See the comments in the workflow file for more info.

## To deploy

1. Fork this repository
2. Install and configure the AWS CLI ([AWS docs here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-prereqs.html))
3. Deploy the ECS cluster using CloudFormation

    ```bash
    cd cfn/

    aws cloudformation deploy --template-file vpc.yaml --stack-name hello-world-vpc --parameter-overrides file://parameters.json

    aws cloudformation deploy --template-file ecs-cluster.yaml --stack-name hello-world-ecs-cluster --parameter-overrides file://parameters.json --capabilities CAPABILITY_NAMED_IAM
    ```

4. Push a git tag to GitHub to trigger the workflow

    ```bash
    git tag v0.0
    git push origin v0.0
    ```

5. Check the GitHub workflow output. You should see that it failed with `Error: arn:aws:ecs:eu-west-2:***:service/hello-world-service is MISSING`.  However the container image should have been pushed to ECR, and the task definition should have been created.
6. Deploy the ECS service using CloudFormation

    ```bash
    aws cloudformation deploy --template-file ecs-service.yaml --stack-name hello-world-ecs-service --parameter-overrides file://parameters.json
    ```

7. Re-run the failed GitHub workflow to ensure that it now succeeds
8. Test the ECS service by browsing to the URL of the load balancer. You can get this URL by examining the load balancer in the AWS console, or from CloudFormation with the following command:

    ```bash
    aws cloudformation describe-stacks --stack-name ecs-test-cluster --query "Stacks[0].Outputs[?OutputKey=='ServiceUrl'].OutputValue" --output text
    ```

## Other useful resources

Reference architecture from AWS:

- https://github.com/awslabs/ecs-refarch-continuous-deployment

Blog posts:

- Using GitHub actions and Terraform: https://particule.io/en/blog/cicd-ecr-ecs/
- Using CodePipeline: https://ecsworkshop.com/ecsanywhere/workloadmanagement/cicd/

Documentation for GitHub Actions:

- https://github.com/aws-actions/amazon-ecs-render-task-definition
- https://github.com/aws-actions/amazon-ecs-deploy-task-definition
