# How to use ENV in a serverless.yml file

SSM (AWS Systems Manager) is a service that allows you to configure and manage your AWS resources centrally. You can store configuration information such as text strings, keys, values, numbers, JSON strings, JSON arrays, etc. in SSM and then access this information from your applications, services, EC2 instances, Lambda functions, etc.

To use SSM in a serverless.yml file, you can define environment variables in the `provider` section of the file. Here's an example:

```yaml
provider:
  name: aws
  runtime: nodejs14.x
  environment:
    MY_ENV_VAR: ${ssm:/myapp/dev/myenvvar}
```

In this example, we define an environment variable `MY_ENV_VAR` and set its value to the SSM parameter `/myapp/dev/myenvvar`. When you deploy your serverless application, the value of the SSM parameter will be injected into the environment variable.

We can even create a resource in the `resources` section of the serverless.yml file to define the SSM parameter:

```yaml
resources:
  Resources:
    MySSMParameter:
      Type: AWS::SSM::Parameter
      Properties:
        Name: /myapp/dev/myenvvar
        Type: String
        Value: myvalue
```

After defining the SSM parameter, you can reference it in the `provider` section of the file as shown in the first example.
