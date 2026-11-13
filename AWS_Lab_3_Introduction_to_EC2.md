# Lab 3: Introduction to Amazon EC2

## Aim

To launch, monitor, secure, resize, and manage an Amazon EC2 instance,
configure HTTP access to a web server, explore EC2 service limits, and
test stop protection.

## Procedure

### Task 1: Launch Your Amazon EC2 Instance

1.  Start the AWS Lab and wait until the AWS status indicator turns
    green.
2.  Open the AWS Management Console and navigate to **EC2**.
3.  Verify that the Region is **N. Virginia (us-east-1)**.
4.  Choose **Launch instance**.
5.  Enter the instance name: `Web Server`.
6.  Select the default **Amazon Linux 2023 AMI**.
7.  Select instance type: `t2.micro`.
8.  Select key pair: `vockey`.
9.  Under **Network settings**, choose **Edit**.
10. Select VPC: `Lab VPC`; keep subnet: `PublicSubnet1`; ensure
    auto-assign public IP is enabled.
11. Under Firewall, choose **Create security group**:
    -   Security group name: `Web Server security group`
    -   Description: `Security group for my web server`
    -   Remove the existing inbound rule.
12. Keep the default storage settings.
13. Expand **Advanced details** and enable **Termination protection**.
14. Paste the following script into the **User data** box:

``` bash
#!/bin/bash
dnf install -y httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

15. Choose **Launch instance**, then **View all instances**.
16. Select `Web Server` and wait until Instance State is **Running** and
    Status Checks show **2/2 checks passed**.

### Task 2: Monitor Your Instance

1.  Select the `Web Server` instance.
2.  Open the **Status checks** tab and verify that System reachability
    and Instance reachability checks have passed.
3.  Open the **Monitoring** tab to view Amazon CloudWatch metrics.
4.  From **Actions**, choose **Monitor and troubleshoot → Get system
    log**.
5.  Verify in the log that the HTTP package was installed through the
    user data script.
6.  Choose **Cancel**.
7.  From **Actions**, choose **Monitor and troubleshoot → Get instance
    screenshot**.
8.  View the screenshot and choose **Cancel**.

### Task 3: Update Your Security Group and Access the Web Server

1.  Select `Web Server` and open the **Details** tab.
2.  Copy the **Public IPv4 address**.
3.  Open a new browser tab, paste the IP address, and press Enter.
4.  Observe that the web page is not accessible because inbound HTTP
    traffic is not allowed yet.
5.  Return to the EC2 Console and choose **Security Groups**.
6.  Select `Web Server security group`.
7.  Open **Inbound rules → Edit inbound rules → Add rule**.
8.  Configure:
    -   Type: `HTTP`
    -   Source: `Anywhere-IPv4`
9.  Choose **Save rules**.
10. Return to the web server browser tab and refresh the page.
11. Verify that **Hello From Your Web Server!** is displayed.

### Task 4: Resize Your Instance -- Instance Type and EBS Volume

**Stop the instance** 1. In EC2, select `Web Server`. 2. Choose
**Instance state → Stop instance** and confirm with **Stop**. 3. Wait
until the Instance State displays **Stopped**.

**Change instance type and enable stop protection** 4. Select
`Web Server`. 5. Choose **Actions → Instance settings → Change instance
type**. 6. Change the instance type to `t2.small` and choose **Apply**.
7. Choose **Actions → Instance settings → Change stop protection**. 8.
Enable stop protection and choose **Save**.

**Resize the EBS volume** 9. Open the **Storage** tab. 10. Select the
Volume ID, then select the volume. 11. Choose **Actions → Modify
volume**. 12. Change the volume size from 8 GiB to `10` GiB. 13. Choose
**Modify**, then confirm by choosing **Modify** again.

**Start the resized instance** 14. Navigate to **Instances** and select
`Web Server`. 15. Choose **Instance state → Start instance**.

### Task 5: Explore EC2 Limits

1.  Search for **Service Quotas** in the AWS Management Console and open
    it.
2.  Choose **AWS services** from the navigation menu.
3.  Search for `ec2` and select **Amazon Elastic Compute Cloud (Amazon
    EC2)**.
4.  In **Find quotas**, search for `running on-demand`.
5.  Observe the filtered EC2 service quotas and the limits on running
    On-Demand instances.

### Task 6: Test Stop Protection

1.  Open the EC2 Console and choose **Instances**.
2.  Select `Web Server`.
3.  Choose **Instance state → Stop instance**, then choose **Stop**.
4.  Observe the error indicating that the instance cannot be stopped
    because stop protection is enabled.
5.  Choose **Actions → Instance settings → Change stop protection**.
6.  Clear **Enable** and choose **Save**.
7.  Select `Web Server` again.
8.  Choose **Instance state → Stop instance** and confirm with **Stop**.
9.  Verify that the instance stops.

## Important Configuration Summary

  Setting                      Value
  ---------------------------- --------------------------------------------
  AWS Region                   N. Virginia (`us-east-1`)
  Instance name                `Web Server`
  AMI                          Amazon Linux 2023
  Initial instance type        `t2.micro`
  Key pair                     `vockey`
  VPC                          `Lab VPC`
  Subnet                       `PublicSubnet1`
  Security group               `Web Server security group`
  Security group description   `Security group for my web server`
  HTTP inbound rule            HTTP from `Anywhere-IPv4`
  Initial EBS volume           8 GiB
  Updated instance type        `t2.small`
  Updated EBS volume           10 GiB
  Termination protection       Enabled
  Stop protection              Enabled, then disabled for final stop test

## Result

Successfully launched and monitored an Amazon EC2 web server, configured
HTTP access through a security group, resized the EC2 instance and its
EBS volume, explored EC2 service quotas, and tested stop protection.

## Submission

1.  Choose **Submit** at the top of the lab instructions.
2.  Choose **Yes** to confirm.
3.  Check the score under **Grades**.
4.  Open **Submission Report** to view detailed feedback.
5.  If some checks are not credited immediately, wait a few minutes and
    submit again as instructed by the lab.
6.  Choose **End Lab** and confirm.
