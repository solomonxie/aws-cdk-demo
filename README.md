# aws-cdk-demo

A minimal AWS CDK (Python) demo that provisions an HTTP API Gateway backed by
a Lambda function. Deploying it stands up a single `GET /hello` endpoint
that returns a static success message.

## Architecture

```
Client --> HTTP API Gateway (HelloHttpApi) --> Lambda (HelloHttpApiLambda)
```

- `app.py` — CDK app entry point; defines `HelloHttpApiStack`, which wires
  an `aws_apigatewayv2.HttpApi` to a Lambda function via a proxy integration.
- `lambda/lambda-handler.py` — the Lambda handler, returns a `200` with a
  static body.
- `requirements.txt` — Python dependencies (AWS CDK v1 core + Lambda/API
  Gateway v2 modules).

## Prerequisites

- [Node.js](https://nodejs.org/) (required by the AWS CDK CLI)
- Python 3.7+
- An AWS account and credentials configured (e.g. via `aws configure` or a
  named profile)

## Setup

```sh
npm install -g aws-cdk
pip install -r requirements.txt
```

## Build & Deploy

Replace `MY_ACCOUNT_ID` and `sam-admin` with your own AWS account ID and
credentials profile.

```sh
# One-time per account/region: provision CDK's bootstrap resources
cdk bootstrap aws://MY_ACCOUNT_ID/us-east-1 --profile sam-admin

# Preview the generated CloudFormation template
cdk synth --profile sam-admin

# Deploy the stack
cdk deploy --profile sam-admin
```

After deployment, the CLI output prints the API's base URL. Test it with:

```sh
curl https://<api-id>.execute-api.us-east-1.amazonaws.com/hello
```

Expected response:

```
Lambda was invoked successfully.
```

## Teardown

```sh
cdk destroy --profile sam-admin
```
