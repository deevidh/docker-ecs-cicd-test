# ECS Task Definition

This template task definition is used by the GitHub Actions workflows. The workflows populate the `image` field with the location of the desired container in ECR, and then update the ECS service to use the new task definition.

In it's current form, this template should work with the sample application and CFN provided in this repository.  However there are many parameters in this template which should be customised based on your application.
