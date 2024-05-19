# AWS CodeBuild

## Overview

AWS CodeBuild is a fully managed build service that compiles source code, runs tests, and produces software packages that are ready to deploy. With CodeBuild, you don’t need to provision, manage, and scale your own build servers. CodeBuild scales continuously and processes multiple builds concurrently, so your builds are not left waiting in a queue. You can get started quickly by using prepackaged build environments, or you can create custom build environments that use your own build tools. With CodeBuild, you are charged by the minute for the compute resources you use.

## Error Gotten While Building

```bash
Error:
Cannot resolve serverless.yml: Variables resolution errored with:
  - Cannot resolve variable at "functions.getNotes.events.0.http.authorizer.arn": User: arn:aws:sts::339713076171:assumed-role/codebuild-notes-build-dev-service-role/AWSCodeBuild-52b2d5db-dca5-4d5d-a960-48cba4461ad5 is not authorized to perform: ssm:GetParameter on resource: arn:aws:ssm:us-west-2:339713076171:parameter/notes/dev/userpoolArn because no identity-based policy allows the ssm:GetParameter action
```

This means that the CodeBuild service role does not have the necessary permissions to access the SSM parameter store. To fix this, we need to update the CodeBuild service role to include the necessary permissions.

### Update CodeBuild Service Role

1. Go to the IAM console
2. Click on `Roles`
3. Search for the CodeBuild service role
4. Click on the CodeBuild service role
5. Click on `Attach policies`
6. Search for `AmazonSSMReadOnlyAccess`
7. Check the box next to `AmazonSSMReadOnlyAccess`
8. Click on `Attach policy`
9. Go back to the CodeBuild console and try building the project again
10. CodePipeline -> Pipeline -> Release Change (Manual Approval)

## Where are the build artifacts stored?

The build artifacts are stored in an S3 bucket. You can find the S3 bucket by going to the CodeBuild console, clicking on the build project, and looking at the `Artifacts` section.

