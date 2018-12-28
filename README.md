# aws-sam-api-gateway-with-cognito-authorizer

An AWS SAM template which creates an API Gateway API with Cognito authorizer and a Lambda function


## Requirements

* AWS CLI already configured with at least PowerUser permission
* [Python 3 installed](https://www.python.org/downloads/)
* [Docker installed](https://www.docker.com/community-edition)
* [Python Virtual Environment](http://docs.python-guide.org/en/latest/dev/virtualenvs/)

## Setup process

Here is the how you can test the template on your side:

1. Download the sam-app.zip file and unzip it

2. Open the template.yaml and update the Lambda function ARN and UserPoolArn. Save the template.yaml file

	Note: I hard-coded the function name and function ARN in the template. Under the AWS::Serverless::Function resource, I define the Lambda name as HelloWorldFunction

		FunctionName: HelloWorldFunction

	Under the AWS::Serverless::Api resource, The uri of the backend Lambda function is the following. The function name in the function ARN matches the FunctionName I defined for the Lambda function.

		uri: "arn:aws:apigateway:us-east-2:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-2:<your-aws-account>:function:HelloWorldFunction:test/invocations"

3. Open the terminal and go to the unzipped folder

```bash
cd sam-app
```

4. Build the application

```bash
  sam build --use-container
```

5. Package the application
```bash
	sam package \

    --output-template-file packaged.yaml \

    --s3-bucket <your-s3-bucket-name>
```

6. Deploy the application

```bash
	sam deploy \

    --template-file packaged.yaml \

    --stack-name sam-app \

    --capabilities CAPABILITY_IAM \

    --region us-east-1
```

--region should be the same as the region of your S3 bucket


7. Open your CloudFormation console and you should be able to see a stack named sam-app

8. Once the Status of the stack becomes CREATE_COMPLETE, you can open the stack and see the API and Lambda function under the Outputs Section

