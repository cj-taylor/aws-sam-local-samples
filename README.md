# aws-sam-local-samples

This codebase goes through various AWS Serverless Application Model sample applications to configure them for full local development.

## Getting Started

1. Clone the repo

```bash
git clone https://github.com/cj-taylor/aws-sam-local-samples.git
cd aws-sam-local-samples
```

2. Install the dependencies

   - [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
   - [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
   - [Docker](https://docs.docker.com/get-docker/)

3. Setup the application

```bash
# example: stepfunction-stocktrader
cd stepfunction-stocktrader
make setup
```

![make-setup](https://user-images.githubusercontent.com/6315911/115114424-57dfed00-9f5d-11eb-9a85-987f4fd1d63f.gif)

5. View logs to observe the usage of the application (new terminal)

```bash
# example: stepfunction-stocktrader
cd stepfunction-stocktrader
make logs
```

![make-logs](https://user-images.githubusercontent.com/6315911/115114606-421ef780-9f5e-11eb-9be9-7ce685ce937e.gif)

6. Open a new terminal and run a command to run the application (new terminal)

```bash
# example: stepfunction-stocktrader
cd stepfunction-stockrader
make run
```

![make-run](https://user-images.githubusercontent.com/6315911/115114526-dfc5f700-9f5d-11eb-8903-aa854960e1ae.gif)

7. Cleanup after the application

_Note: Stop the commands running `make setup` and `make logs` first_

```bash
# example: stepfunction-stocktrader
cd stepfunction-stockrader
make teardown
```

![make-teardown](https://user-images.githubusercontent.com/6315911/115114578-1c91ee00-9f5e-11eb-8df7-ca9db6cba266.gif)

## Resources

- [AWS Serverless Application Model - Testing and debugging serverless applications](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-test-and-debug.html)
- [AWS Step Function - Setting Up Step Functions Local](https://docs.aws.amazon.com/step-functions/latest/dg/sfn-local.html)
- [Amazon DynamoDB - Deploying DynamoDB Locally on Your Computer](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)
- [Localstack - A fully functional local AWS cloud stack. Develop and test your cloud & Serverless apps offline!](https://github.com/localstack/localstack)
