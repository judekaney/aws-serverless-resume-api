# AWS Serverless Resume API
Create a serverless API backed by a Python-based Lambda function and S3 bucket that responds to HTTP requests with your resume in JSON format—all in AWS. 
(Want to do this automatically? Use this SAM)
## Architecture

![Diagram](aws-serverless-resume-API-diagram.png)

* API Gateway to deploy your API
* Lambda to create your serverless function, using Python (or your language of choice)
* S3 to store your document in JSON format 

## You'll need
* An AWS account
* Optional: A development environment of your choice (Lambda's built-in console is also sufficient)

### Step 1: Prepare your S3 bucket
Log into your AWS account:

`aws configure`

Enter your access key, secret access key, default region name, and output format. 

Next, create your bucket:

`aws s3api create-bucket --bucket your-bucket-name --region your-region`

Then, copy your resume to your bucket:

`aws s3 cp /path/to/resume.json s3://your-bucket-name/`

