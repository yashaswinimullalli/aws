# Lab 4: AWS Lambda -- Automating EC2 Instance Management

## Aim

To create an AWS Lambda function, configure an Amazon EventBridge
scheduled trigger, and automatically stop a running Amazon EC2 instance
using the Boto3 Python SDK.

## Procedure

### Task 1: Create a Lambda Function

1.  Start the AWS Lab and wait until the AWS status indicator turns
    green.
2.  Open the AWS Management Console.
3.  Search for **Lambda** and open the AWS Lambda console.
4.  Choose **Create a function**.
5.  Select **Author from scratch**.
6.  Configure the function:
    -   Function name: `myStopinator`
    -   Runtime: `Python 3.11`
7.  Expand **Change default execution role**.
8.  Select **Use an existing role**.
9.  From the Existing role dropdown, select `myStopinatorRole`.
10. Choose **Create function**.

### Task 2: Configure the Trigger

1.  In the Lambda function overview, choose **Add trigger**.
2.  From the **Select a trigger** dropdown, choose **EventBridge
    (CloudWatch Events)**.
3.  Under Rule, select **Create a new rule**.
4.  Configure the rule:
    -   Rule name: `everyMinute`
    -   Rule type: `Schedule expression`
    -   Schedule expression: `rate(1 minute)`
5.  Choose **Add**.

### Task 3: Configure the Lambda Function

1.  Below the Function overview pane, choose the **Code** tab.
2.  Select `lambda_function.py`.
3.  Delete the existing code.
4.  Copy and paste the following Python code:

``` python
import boto3

region = '<REPLACE_WITH_REGION>'

instances = ['<REPLACE_WITH_INSTANCE_ID>']

ec2 = boto3.client('ec2', region_name=region)


def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
```

5.  Replace `<REPLACE_WITH_REGION>` with the actual AWS Region code. For
    example, use `us-east-1` for N. Virginia.
6.  Keep the single quotation marks around the Region value.
7.  Open the EC2 Console in another browser tab.
8.  Select the running EC2 instance named `instance1`.
9.  Open the **Details** tab and copy its Instance ID (starting with
    `i-`).
10. Return to the Lambda console.
11. Replace `<REPLACE_WITH_INSTANCE_ID>` with the copied EC2 instance
    ID.
12. Keep the single quotation marks around the instance ID.
13. Choose **File → Save**.
14. In the Code source pane, choose **Deploy**.
15. Choose the **Monitor** tab.
16. Observe the function invocation, error count, and success-rate
    metrics.

### Task 4: Verify That the Lambda Function Worked

1.  Return to the EC2 Console browser tab.
2.  Refresh the Instances page.
3.  Verify that the `instance1` EC2 instance has stopped.
4.  Try starting the instance again.
5.  Wait and refresh the page.
6.  Verify that the instance is stopped again within approximately one
    minute because the EventBridge rule triggers the Lambda function
    every minute.

## Important Configuration Summary

  Setting               Value
  --------------------- ----------------------------------------
  AWS Service           AWS Lambda
  Function name         `myStopinator`
  Function creation     Author from scratch
  Runtime               Python 3.11
  Execution role        `myStopinatorRole`
  Trigger service       Amazon EventBridge (CloudWatch Events)
  Rule name             `everyMinute`
  Rule type             Schedule expression
  Schedule expression   `rate(1 minute)`
  Python SDK            Boto3
  EC2 instance name     `instance1`
  Lambda action         Stop EC2 instance

## Python Code

``` python
import boto3

region = '<REPLACE_WITH_REGION>'

instances = ['<REPLACE_WITH_INSTANCE_ID>']

ec2 = boto3.client('ec2', region_name=region)


def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
```

**Note:** Replace the Region and EC2 instance ID placeholders with the
actual values from your lab environment. Do not remove the quotation
marks.

## Result

Successfully created an AWS Lambda function using Python 3.11,
configured an EventBridge scheduled trigger, and automated the stopping
of a running EC2 instance every minute using the Boto3 SDK.

## Submission

1.  Choose **Submit** at the top of the lab instructions.
2.  Choose **Yes** to confirm.
3.  Wait for the Grades panel to appear.
4.  Check the score under **Grades**.
5.  Open **Submission Report** to view detailed feedback.
6.  If some checks are not credited immediately, wait a few minutes and
    submit again.
7.  Choose **End Lab** and confirm.

## Important Concepts for Revision

  Concept               Meaning
  --------------------- -------------------------------------------------
  AWS Lambda            Runs code without managing servers
  EventBridge           Triggers functions based on events or schedules
  Boto3                 AWS SDK for Python
  IAM Execution Role    Gives Lambda permission to access AWS resources
  Schedule Expression   Defines when a scheduled rule runs
  `rate(1 minute)`      Triggers the function every minute
  `stop_instances()`    Boto3 method used to stop EC2 instances
