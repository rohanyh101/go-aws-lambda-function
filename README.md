# AWS Lambda Function Demo using Go

## Overview

This repository contains a simple AWS Lambda function written in Go. It demonstrates creating, deploying, and managing a Lambda function using the AWS CLI.

## Features

- **Written in Go**: Leverages the Go programming language for the Lambda function.
- **AWS CLI**: Uses the AWS CLI for deployment and management.
- **Minimal Example**: A basic Lambda function to help you get started.

## Prerequisites

Before using this repository, ensure you have the following installed:

- [Go](https://golang.org/doc/install): Version 1.15 or later.
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html): Version 2 or later.
- An [AWS account](https://aws.amazon.com/) with appropriate permissions.

## Getting Started

### Clone the Repository

```powershell
git clone https://github.com/rohanyh101/go-aws-lambda-function.git
cd your-repository-name
```
## Build the Lambda Function

1. **Navigate to the project directory** (if not already there):

    ```powershell
    cd path/to/your/lambda-function
    ```

2. **Build the Go executable for Linux**:

   #### Make sure that the binary file name must be `bootstrap`, it's mandatory
   
    ```powershell
    GOOS=linux GOARCH=amd64 go build -o bootstrap
    ```

    This generates an executable file named `main` which is the entry point for your Lambda function.

## Deploy the Lambda Function

1. **Package the executable into a ZIP file**:

    ```powershell
    zip function.zip main
    ```

2. **Create an IAM Role** (if not already created):

```powershell
  # request
   aws iam create-role \
   --role-name lambda-ex \
   --assume-role-policy-document '{"Version": "2012-10-17","Statement": [{ "Effect": "Allow", "Principal": {"Service": "lambda.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'

  # response,
   {
    "Role": {
        "Path": "/",
        "RoleName": "lambda-ex",
        "RoleId": "AROASDRAJKGQBUH7OUJZOK",
        "Arn": "arn:aws:iam::34278964789:role/lambda-ex",
        "CreateDate": "2024-08-13T14:44:09+00:00",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "lambda.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
    }
}
```

   Please make sure you have a role with the necessary permissions. You can create one using the AWS CLI or AWS Management Console.

4. **Attach policy `AWSLambdaBasicExecutionRole` to the created Role**,
   ```powershell
   aws iam attach-role-policy --role-name lambda-ex --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
   ```

5. **Create the Lambda function**:

    ```powershell
    aws lambda create-function \
      --function-name your-function-name \
      --zip-file fileb://function.zip \
      --handler bootstrap \
      --runtime provided.al2023 \
      --role arn:aws:iam::account-id:role/your-role-name
    ```

    Replace `your-function-name`, `arn:aws:iam::account-id:role/your-role-name`, and other placeholders with your actual values.

## Test the Lambda Function

You can test the Lambda function from the AWS Management Console or use the AWS CLI to invoke it directly:

```powershell
# request
aws lambda invoke \
--function-name your-function-name \
--payload '{ "name": "rohan", "age": 23 }' output.txt

# response
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```
check the `output.txt` in your project directory for a response

## Clean Up

To delete the Lambda function, use:

```powershell
aws lambda delete-function \
  --function-name your-function-name
```

## Contributing
Feel free to submit issues, pull requests, or feedback. Contributions are welcome!

## Contact
if you have any questions or suggestions, please open an issue or contact me at rohanyh@gmail.com.
