# ECS Task Definition and Container Env Vars

These templates are used by the GitHub Actions workflow to render the task definition which is used for deployments to the ECS Service.

The following steps are taken:

## Task Definition Variables

Any variables in `task_definition_template.json` in the format `${FOOBAR}` are replaced with the value of the variable `FOOBAR` from the GitHub workflow. This is useful for specifying various things like the container name, the AWS logs group and region, the name of the execution IAM Role.

## Container Environment Variables

Any variables you wish to pass to the container should be specified in the environment-specific files `prod_container_env_vars` and `staging_container_env_vars`.  The GitHub workflow will add the variables from these files into the task definition at the path `.containerDefinitions[0].environment`.

Variables should be specified in the files in the following format:

```bash
ENVIRONMENT=staging
FOO=bar
```

These will be converted to the correct JSON format when added to the task definition.

## Docker image

The URL for to the newly built docker image in ECR is added to the task definition at the path `.containerDefinitions[0].image`.