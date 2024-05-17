# General notes about serverless and AWS

## What is Serverless?

Serverless is a cloud computing model that allows you to build and run applications without managing the underlying infrastructure. In a serverless architecture, the cloud provider takes care of provisioning, scaling, and managing the servers, allowing you to focus on writing code and building applications.

### We can assign individual permissions to each lambda function

- We can assign individual permissions to each lambda function using the `iamRoleStatements` property in the `serverless.yml` file. This allows us to define fine-grained permissions for each lambda function, restricting access to specific AWS resources. To achieve this, we need to install a plugin called `serverless-iam-roles-per-function`.

```bash
npm i -D serverless-iam-roles-per-function
```

Ane example of how to use this plugin is shown below:

```yaml
functions:
  hello:
    handler: handler.hello
    iamRoleStatements:
      - Effect: Allow
        Action:
          - s3:ListBucket
        Resource: arn:aws:s3:::my-bucket
```

### Reusing HTTP connections

- When working with AWS SDK for JavaScript, you can reuse HTTP connections to improve performance and reduce latency. By reusing HTTP connections, you can avoid the overhead of establishing a new connection for each request, resulting in faster response times. [Read more](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/node-reusing-connections.html)

### API Gateway Timeout

- The API Gateway has a default timeout of 30 seconds for Lambda functions. If a Lambda function takes longer than 30 seconds to respond, the API Gateway will return a timeout error. It is important to ensure that your Lambda functions respond within the timeout limit to avoid errors. However, Lambda functions can run for a maximum of 15 minutes.

### Managing Permissions with Serverless Framework

- It is recommended to manage permissions using the Serverless Framework to define IAM roles, policies, and permissions for your serverless applications. By defining permissions in the `serverless.yml` file, you can ensure that your resources have the necessary permissions to interact with other AWS services.




