# API Gateway Access Controlling Methods

- API Key
- Lambda Authorizer
- Cognito User Pool Authorizer
- IAM Authorizer
- Others
  - VPC Endpoint
  - API Gateway Resource Policy
  - Tagged-based Access Control

## API Key

An API key is a token that you can distribute to your customers to grant access to your API. API keys are useful in cases where you need to identify the consumer of your API, but you do not need to authenticate the consumer. API keys are used to track and control how the API is being used, for example, to prevent malicious use or abuse of the API.

### When to use API Key

- When you need to implement custom authorization logic.
- Not recommended for authentication and authorization.
- Used to enforce throttling limits to the API daily or monthly quota (Limiting the number of requests).
- Can be used with other Access Control methods e.g. Lambda Authorizer, User Pool Authorizer and IAM Authorizer.
- Can be used in microservices to access internal APIs (From protected environments).

### How to use API Key

1. Create an API Key in the API Gateway console.
2. Add the API Key to the API Gateway stage.
3. Add the API Key to the HTTP request header.
4. Enable API Key in the API Gateway stage settings.
5. Use the API Key to access the API.

### API Key Limitations

- API keys are not secure for authentication and authorization.

## Lambda Authorizer

A Lambda authorizer is an AWS Lambda function that you provide to control access to your API methods. Useful when you want to implement a custom authorization scheme that uses a bearer token authentication strategy or that uses request parameters to determine the caller's identity.

![Lambda Authorizer](./assets/lambda-authorizer.png)

### When to use Lambda Authorizer

- When you need to implement custom authorization logic.
- When you need to authenticate the caller using bearer token authentication strategy.
- When you need to authenticate the caller using request parameters.
- When you need to authenticate the caller using a third-party identity provider.

### How to use Lambda Authorizer

1. Create a Lambda function that implements the authorization logic.
2. Add the Lambda function to the API Gateway method.
3. Use the Lambda function to authorize the caller.

### Lambda Authorizer Limitations

- Lambda authorizers are not suitable for high-performance applications.

## Cognito User Pool Authorizer

**Cognito User Pool:** Amazon Cognito user pools are user directories that provide sign-up and sign-in options for your web and mobile applications. You can use Amazon Cognito user pools as an identity provider for API Gateway to control access to your API methods.

**Cognito User Pool Client:** A user pool client is a frontend app that uses the Cognito user pool to authenticate users. The client is a JavaScript app that runs in a web browser or a mobile app that runs on a device. The client interacts with the user pool through the Amazon Cognito API.

**Cognito User Pool Domain:** A user pool domain is a hosted UI that allows users to sign in to your app and access the Amazon Cognito user pool. The domain is a URL that you can use to redirect users to the hosted UI. The domain is a subdomain of the Amazon Cognito domain.

### When to use Cognito User Pool Authorizer

- When you need to authenticate the caller using Amazon Cognito user pools.
- When you need to implement user management features such as sign-up, sign-in, and password recovery.
- When you need to implement multi-factor authentication (MFA) for your users.

### How to use Cognito User Pool Authorizer

1. Create an Amazon Cognito user pool.
2. Add the user pool to the API Gateway method.
3. Use the user pool to authorize the caller.

### Cognito User Pool Authorizer Limitations

- Cognito user pool authorizers are not suitable for custom authorization logic.
- It only checks if the user is available in the user pool, if the user is available, it will allow the user to access the API

### Creating a User Pool 

1. Go to the Amazon Cognito console and create a user pool.
2. Add an app client to the user pool.
3. Create a domain for the user pool.
4. Create a user in the user pool.
5. Add the callback URL to the user pool client.
6. Go to the API Gateway console and create a new authorizer.
7. Add the user pool to the API Gateway method.
8. Use the user pool to authorize the caller.

The domain generated in the user pool client should look like this:

```
 https://exampledev.auth.us-west-2.amazoncognito.com
```

With the host and the client id of the user pool client, we can build a URL for login like this:

```
https://exampledev.auth.us-west-2.amazoncognito.com/login?response_type=token&client_id=6ah2cp494ua6vjel1k7qd8vo33&redirect_uri=http://localhost:3000
```

This URL will allow the user to login and get the token to access the API.

```
http://localhost:3000/#id_token=eyJraWQiOiJDdVVETjNvZHpBM3JUNlQ5TmRVVjBEZWhoWXRqU0VGMlptalwvSGt3aGxHYz0iLCJhbGciOiJSUzI1NiJ9.eyJhdF9oYXNoIjoienA3ZDAxNlI4UDg2WENlamN5NHptUSIsInN1YiI6IjQ4OTFhM2YwLWIwNTEtNzA5ZS1mZDgwLTdlYjMyMGQxODkwZiIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAudXMtd2VzdC0yLmFtYXpvbmF3cy5jb21cL3VzLXdlc3QtMl9nS2tDTUZVZWciLCJjb2duaXRvOnVzZXJuYW1lIjoiZmNhYnJhIiwiYXVkIjoic2x2dWVuNWI1aW44dmhzaWMyaTBkdWRhMCIsInRva2VuX3VzZSI6ImlkIiwiYXV0aF90aW1lIjoxNzE1NzU0MDgzLCJleHAiOjE3MTU3NTc2ODMsImlhdCI6MTcxNTc1NDA4MywianRpIjoiZmU3MDIwZGYtZGI4OS00OWZkLWI5N2ItMzAxOGIwMjQ5YjhkIiwiZW1haWwiOiJmY2FicmFAZ21haWwuY29tIn0.cpPfUuIN28Za1Ybtaiv3oI7YQ7zx6tgyBH3BgJOjCOJoMEzL7NFXVbbX22ekfSKmivVqoIDDKWJ6cn_oBChUoDg9F9OPEsxgcT2F7L1JPa0dMmvknaVfX1YO9Qs1mKtLXGtJKAIlH1pZUu7yjSvUnPsX69b5NEvrYZ1SgN1KJWxHr6UZDStd1RgSBxaMEMAtG2F-TadKXSl6VpzAM3UTkTUc_NNPDq9dOOfp1wTD9viLc6RCGo3ReG5XSx1aP2CzoKpDE4G2oxYfyLL6X2LXol6ANOs1MNBbqc6RcEZsVYPu7sbclM3W0r30Qj-NcK73rDPpjHP-Tkuyrh8yfLF-dw&access_token=eyJraWQiOiJPRTFYbEZBKzVsZ09leG51QWdJeGhVSmRDZDZ5akxSeWlwdnVUSkNncEdrPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiI0ODkxYTNmMC1iMDUxLTcwOWUtZmQ4MC03ZWIzMjBkMTg5MGYiLCJ0b2tlbl91c2UiOiJhY2Nlc3MiLCJzY29wZSI6InBob25lIG9wZW5pZCBlbWFpbCIsImF1dGhfdGltZSI6MTcxNTc1NDA4MywiaXNzIjoiaHR0cHM6XC9cL2NvZ25pdG8taWRwLnVzLXdlc3QtMi5hbWF6b25hd3MuY29tXC91cy13ZXN0LTJfZ0trQ01GVWVnIiwiZXhwIjoxNzE1NzU3NjgzLCJpYXQiOjE3MTU3NTQwODMsInZlcnNpb24iOjIsImp0aSI6Ijg5NjY4Y2E4LTlhZjMtNDFjYS1hYzBkLWNjNDJhMmU4Njg2ZiIsImNsaWVudF9pZCI6InNsdnVlbjViNWluOHZoc2ljMmkwZHVkYTAiLCJ1c2VybmFtZSI6ImZjYWJyYSJ9.GHMPFAlQlGhSse_P-Ndm4-7113UXGW8xAs3N0byI6SlBVZO4yrg9Sjdsv1DXdn0qLqmusIyR0BGnu4qmO8bS6DO8NAkglk8fbTIvuLn1iMaMpTMc7o_ZRfsq-OwPPJefGD8ApA3rrHOJgJsIpU9gjfW7rvopVX-e0b9KyZ9wSeohaI6iaGMFRJAQ4mYpwahGLT0zQdCFfmB-kvEUsnH-KVcnURKDKNKzfxa2Fo2QErQBMX6uaJto02keGM3PLvxEXkAzmXvJ0cuvv73ZczzYRUXkeketPt5kqrEJ5MgC0rjrvf4N5FtGTznYr80RIvF4_P0_6U3srxqUCQX3ZYQJMw&expires_in=3600&token_type=Bearer
```

And from the response, we can get the token to access the API.

## IAM Authorizer

An IAM authorizer is an API Gateway feature that uses Amazon API Gateway to control access to your API methods. You can use IAM authorizers to authorize API requests using AWS Identity and Access Management (IAM) policies.

### When to use IAM Authorizer

- When you need to authenticate the caller using AWS IAM policies.
- When you need to implement fine-grained access control for your API methods.
- When you need to implement cross-account access control for your API methods.
- When you need to implement resource-based access control for your API methods.
- When you need to implement role-based access control for your API methods.

### How to use IAM Authorizer

1. Create an IAM policy that defines the access control rules.
2. Add the IAM policy to the API Gateway method.
3. Use the IAM policy to authorize the caller.

### IAM Authorizer Limitations

- IAM authorizers are not suitable for custom authorization logic.
- IAM authorizers are not suitable for bearer token authentication strategy.
- IAM authorizers are not suitable for request parameter authentication.
- IAM authorizers are not suitable for third-party identity provider authentication.

