# Activity: AWS Elastic Beanstalk

## Aim

To access a pre-created AWS Elastic Beanstalk environment, deploy a
sample Tomcat application, and explore the AWS resources that support
the application.

## Procedure

### Task 1: Access the Elastic Beanstalk Environment

1.  Start the AWS Lab and wait until the AWS status indicator turns
    green.
2.  Open the AWS Management Console using the **AWS** link.
3.  Search for **Elastic Beanstalk** in the Services search bar and open
    it.
4.  On the **Environments** page, locate the existing Elastic Beanstalk
    application.
5.  Wait until the Health column displays **Ok**.
6.  Under the Environment name column, choose the environment name to
    open its Dashboard.
7.  Verify that the application health is **Ok**.
8.  Choose the **Domain** link near the top of the page.
9.  Verify that the browser displays **HTTP Status 404 - Not Found**.
10. Return to the Elastic Beanstalk console. The 404 response is
    expected because application code has not yet been deployed.

### Task 2: Deploy a Sample Application to Elastic Beanstalk

1.  Download the sample application from the AWS sample link: [Download
    tomcat.zip](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip)
2.  Return to the Elastic Beanstalk Dashboard.
3.  Choose **Upload and Deploy**.
4.  Choose **Choose File** and select the downloaded `tomcat.zip` file.
5.  Choose **Deploy**.
6.  Wait one or two minutes for the environment to update and the
    application to deploy.
7.  After deployment is complete, choose the **Domain URL** link.
8.  Verify that the deployed web application is displayed.
9.  Return to the Elastic Beanstalk console and choose **Configuration**
    in the left pane.
10. Observe the **Instance traffic and scaling** panel, including EC2
    security groups, minimum and maximum instances, and instance type.
11. Observe the **Networking, database, and tags** panel. No database
    configuration is displayed because this environment does not include
    a database.
12. Choose **Edit** in the Networking, database, and tags panel to view
    the available database configuration options.
13. Choose **Cancel** without making changes.
14. Under Environment in the left panel, choose **Monitoring**.
15. Browse the monitoring charts to observe the information available
    for the environment.

### Task 3: Explore the AWS Resources That Support Your Application

1.  Search for **EC2** in the AWS Management Console and open the EC2
    console.
2.  Choose **Instances**.
3.  Observe the two running EC2 instances that support the web
    application. Both instances contain `samp` in their names.
4.  Explore the resources created by Elastic Beanstalk:
    -   A security group with port 80 open.
    -   A load balancer that both instances belong to.
    -   An Auto Scaling group that runs from two to six instances
        depending on network load.
5.  Verify that these supporting AWS resources are available in the
    account.

## Important Concepts

  -----------------------------------------------------------------------
  Concept                             Description
  ----------------------------------- -----------------------------------
  AWS Elastic Beanstalk               Service used to deploy and manage
                                      applications

  Environment                         The collection of AWS resources
                                      running the application

  Domain URL                          URL used to access the deployed
                                      application

  Upload and Deploy                   Option used to upload and deploy
                                      application code

  Configuration                       Displays environment and resource
                                      settings

  Monitoring                          Displays charts about the
                                      environment

  EC2                                 Compute instances that host the
                                      application

  Security Group                      Controls network traffic to and
                                      from instances

  Load Balancer                       Distributes application traffic
                                      across instances

  Auto Scaling                        Adjusts the number of instances
                                      based on demand
  -----------------------------------------------------------------------

## Result

Successfully accessed a pre-created AWS Elastic Beanstalk environment,
deployed a sample Tomcat application, verified the deployed application
through its domain URL, and explored the EC2 instances, security group,
load balancer, and Auto Scaling group supporting the application.

## Submission

1.  Choose **Submit** at the top of the lab instructions.
2.  Choose **Yes** to confirm.
3.  Wait for the Grades panel to appear and check the score.
4.  If some checks are not credited, note that certain checks require at
    least five minutes after completing the action. Wait a few minutes
    and submit again.
5.  Open **Submission Report** to view detailed feedback.
6.  Choose **End Lab** and confirm with **Yes**.
7.  Close the confirmation panel using the **X** in the top-right
    corner.
