# Deploy to multiple environments

We can deploy our serverless application to multiple environments using the `--stage` option in the `sls deploy` command. The `--stage` option allows us to specify the environment or stage to which we want to deploy our application. By default, the stage is set to `dev`.

## Previous steps

Before deploying into multiple environments, we can delete the resources created in the previous steps:

```bash
sls remove
```

Then here's how you can deploy your serverless application to different environments:

1. **Isolate the resources for each environment**: To deploy your serverless application to multiple environments, you need to isolate the resources for each environment. This can be done by using separate AWS resources (e.g., DynamoDB tables, S3 buckets) for each environment.

2. **Deploy the resources for each environment**: Once you have isolated the resources for each environment, you can deploy your serverless application to each environment using the `--stage` option. For example, to deploy your application to the `prod` environment, you can run:

   ```bash
   sls deploy --stage prod
   ```

   Similarly, you can deploy your application to the `demo` environment by running:

   ```bash
   sls deploy --stage demo
   ```

   If you don't specify a stage, the default stage (`dev`) will be used:

   ```bash
   sls deploy
   ```

3. **Test the new environment**: After deploying your serverless application to a new environment, you can test it by interacting with the API endpoints or services. For example, if you have set up a Cognito User Pool for authentication, you can create a user in the Cognito User Pool and test the authentication flow for the new environment.
