---
title: Automate your tasks with AWS Lambda
layout: post
categories:
- automation
- AWS
- lambda
- web scraping
---
## Introduction and Goal
In a previous post, I [kept up with the news using Python](https://ericchan24.github.io/automation/cron/email/web%20scraping/2019/11/30/Keep-Up-With-the-News-Using-Python/){:target="_blank"}.
I utilized Cron on my local machine to scheudle and run a Python script to
scrape and grab all Fangraphs articles for the day.

This works well when my local machine is turned on and available to run the
script. If not, the script fails to run and I miss out on the latest
baseball analysis from Fangraphs.

A better way to do this is with AWS Lambda. Lambda is a serverless cloud
computing service and is perfect for this task. To get this running, I'll have
to write some code that will be run by Lambda and stored in an S3 bucket.  

## Tools used
AWS lambda

## Create a S3 Bucket
Create a S3 bucket to store the articles. From the main AWS page, click on
the Services tab on the top of the screen. Type S3 in the search bar and click
on S3.

![S3](/assets/img/2020-05-May/20/s3_photo.png){:width='550px'}

Create a bucket. I named my bucket fangraphs.

## Initialize Lambda Function
From AWS, click on the Services dropdown and type in Lambda.

![Lambda](/assets/img/2020-05-May/20/lambda_photo.png){:width='500px'}

Click Create Function.

![Create Function](/assets/img/2020-05-May/20/create_function.png){:width='500px'}

Select Author from scratch and enter a Function name. Choose your programming
language and click Create Function.

![Create Function1](/assets/img/2020-05-May/20/create_function1.png){:width='500px'}

## Write Lambda Function code
Write your lambda function code. Name your script lambda_function.py containing
a function named lambda_handler().

```python
# lambda_function.py
def lambda_handler(event, context):
    # write code here
```
The code that I wrote grabs all Fangraphs articles for the day, converts them
to a text object and saves it to my S3 bucket. This is a link to the
[code](https://github.com/ericchan24/automate_your_tasks_with_aws_lambda/blob/master/scripts/lambda_function.py){:target="_blank"}.

Here's one tricky part of the code. You will need your AWS credentials to access
your S3 bucket. The code below shows how I did that.

```python
region = os.environ.get('AWS_REGION')
access_key = os.environ.get('AWS_ACCESS_KEY')
secret_key = os.environ.get('AWS_SECRET_KEY')

# access your S3 bucket
s3 = boto3.resource(
    's3',
    region_name = region,
    aws_access_key_id = access_key,
    aws_secret_access_key = secret_key
)
# place the articles in the S3 bucket
s3.Object('fangraphs', f_name).put(Body = content)
```

These are the [environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html){:target="_blank"}
that Lambda can access.

## Install Packages and Create a Zip File
If your code uses packages outside of the base Python installation, you will
have to create a zip file containing those packages and upload them to Lambda.
Here are the [packages](https://gist.github.com/gene1wood/4a052f39490fae00e0c3){:target="_blank"}
available in AWS Lambda environments.

In addition to the Python base installation, my code used BeautifulSoup, lxml,
and requests.

Make sure these files are installed in the same directory as your code.
To do this, navigate to the directory where your code is and type the following:

```bash
pip install beautifulsoup4 --target .
pip install lxml --target .
pip install requests --target .
```

When you have all your packages and code ready to go, zip all these files
together. Navigate to the directory where your lambda_function.py file is
located and type the following:

```bash
zip -r ../lambda_function.zip *
```

Saving the zip file one level up from the lambda_function.py file worked for me.

## Upload Zip File
Back on the AWS Lambda Function screen, navigate to the Function code section.
Under the Code entry type dropdown, select Upload a .zip file and choose your
zip file.

![Upload Zip](/assets/img/2020-05-May/20/upload_zip.png){:width='500px'}

## Set Permissions
Permission needs to be granted to your Lambda Function to allow access to your
S3 bucket. Click the Services dropdown and type IAM. Click on IAM Manage access
to AWS resources.

On the IAM Dashboard, under Access Management, select Role. Click the Create
Role buttom.

![IAM](/assets/img/2020-05-May/20/IAM.png){:width='500px'}

On Create role page 1, select Lambda and click on the Next: Permissions
button on the bottom of the page.

![IAM1](/assets/img/2020-05-May/20/IAM1.png){:width='500px'}

On Create role page 2, type S3 in search box and select AmazonS3FullAccess.
Click the Next: Tags button on the bottom of the page.

![IAM2](/assets/img/2020-05-May/20/IAM2.png){:width='500px'}

On Create role page 3, click Next: Review

![IAM3](/assets/img/2020-05-May/20/IAM3.png){:width='500px'}

On Create role page 4, Type in a Role name and description. Click the
Create role button on the bottom of the screen.

![IAM4](/assets/img/2020-05-May/20/IAM4.png){:width='500px'}

Go back to your Lambda Function and selec thte Permissions Tab. Under Execution
role, click the Edit button.

![IAM5](/assets/img/2020-05-May/20/IAM5.png){:width='500px'}

Increase your Timeout. Under Existing role, select the role that you just
created. Click the Save button.

![IAM6](/assets/img/2020-05-May/20/IAM6.png){:width='500px'}

Click the Permissions tab and verify that the Execution role has been set
correctly.
![IAM7](/assets/img/2020-05-May/20/IAM7.png){:width='500px'}

## Test Your Code
After setting up permisisons, test your code. Click the Test button in the top
right of the Lambda Function dashboard.

![Code Test](/assets/img/2020-05-May/20/code_test.png){:width='500px'}

AWS Lambda will tell you whether or not your test succeeded with execution logs
for tracing your runs.

![Code Test1](/assets/img/2020-05-May/20/code_test1.png){:width='500px'}

My code is writing a file to my S3 bucket, so I navigated to the bucket to
verify that a file was written there.

![S3 Photo1](/assets/img/2020-05-May/20/s3_photo1.png){:width='500px'}

## Schedule Your Code to Execute
On the Lambda Function dashboard, under CloudWatch Events/EventBridge click Add
Trigger.

![CloudWatch](/assets/img/2020-05-May/20/cloudwatch.png){:width='500px'}

On the Add trigger menu, select CloudWatch Events/EventBridge and Create a
new rule. I named my rule FangraphsDaily, gave it a description and used a Cron
expression for my daily run. Click the Enable trigger checkbox.

![CloudWatch](/assets/img/2020-05-May/20/cloudwatch1.png){:width='500px'}

## Auomation Complete!
You have set up a Lambda Function to automatically run a script and save the
results to a S3 bucket. Instead of running and scheduling this job locally,
take advantage of AWS Lambda to automate your tasks!
