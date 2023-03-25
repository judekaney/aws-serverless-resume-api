# AWS Serverless Resume API
Create a serverless API backed by a Python-based Lambda function and S3 bucket that responds to HTTP requests with your resume in JSON format—all in AWS. 
> NOTE: The Amazon [API Gateway](https://aws.amazon.com/api-gateway/pricing/) [free tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all) includes **one million API calls** received for REST APIs per month for 12 months. AWS [Lambda](https://aws.amazon.com/lambda/pricing/) includes **one million free requests** per month. The API you'll be deploying is [throttled](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-request-throttling.html) to a limit of 3 API calls per second. This entire stack, including [S3](https://aws.amazon.com/s3/pricing/), should be effectively free. 

## Architecture

![Diagram](aws-serverless-resume-API-diagram.png)

* API Gateway to deploy your API
* Lambda to create your serverless function, using Python
* S3 to store your document in JSON format 

## You'll need
* An AWS account
* [AWS CLI](https://aws.amazon.com/cli/) installed on your OS
* Your resume in [JSON](https://en.wikipedia.org/wiki/JSON) format

> Need help formatting your resume? Click [here](https://jsonresume.org/getting-started/)

## Getting started
### Get the CloudFormation template and resume
* Download the `serverless-resume-api.yaml` CloudFormation template above and prepare your JSON resume. You can optionally use the `testresume.json` file provided above.

### Authenticate with AWS
* In your terminal, type `aws configure` to log into AWS CLI. You'll be prompted for your `AWS Access Key ID`, `AWS Secret Access Key`, `Default region name`, and optionally `Default output format`. 

> Need help finding your access keys? Click [here](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html)

### Deploy the CloudFormation template and upload your resume
1. In your terminal, type the following command to use the Infrastrucutre as Code (IaC) file to deploy your resources:
  ```sh
  aws cloudformation deploy --stack-name <stack-name> --template-file </path/to/serverless-resume-api.yaml> --capabilities CAPABILITY_NAMED_IAM   
  ```
  * Replace `<stack-name>` with whatever you would like to call your CloudFormation stack.
  * Replace `</path/to/serverless-resume-api.yaml>` with the local path to your `serverless-resume-api.yaml` file. 
2. After your terminal shows `"Successfully created/updated stack - <stack-name>"` use the following command to upload your resume to the S3 bucket:
  ```sh
  aws s3 cp <path/to/local/resumefile.json>  s3://my-resume-bucket-<account-number>    
  ```
  * Replace `<path/to/local/resumefile.json>` with the local path to your resume file.
  * Replace `<account-number>` with your AWS account number.

> To get your AWS account number, run the `aws sts get-caller-identity` command

### Test your API

After uploading your resume, it should now be available at:
```sh
https://<your-api-id>.execute-api.<your-aws-region>.amazonaws.com/prod/getresume    
```
> To get your API ID, run the `aws apigateway get-rest-apis --query "items[?name=='resume-api'].id" --output text` command

* Replace `<your-api-id>` with your API ID.
* Replace `<your-aws-region>` with your AWS region.
