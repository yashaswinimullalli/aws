# Lab 2: Build Your VPC and Launch a Web Server

## Aim

To create an Amazon VPC, configure public and private subnets, create a
security group, and launch an EC2 instance configured as a web server.

## Procedure

### Task 1: Create Your VPC

1.  Start the AWS Lab and wait until the AWS status indicator turns
    green.
2.  Open the AWS Management Console and search for **VPC**.
3.  Ensure the AWS Region is **N. Virginia (us-east-1)**.
4.  Choose **VPC dashboard → Create VPC**.
5.  Select **VPC and more**.
6.  Configure the following settings:
    -   Name tag auto-generation: `lab`
    -   IPv4 CIDR block: `10.0.0.0/16`
    -   Number of Availability Zones: `1`
    -   Number of public subnets: `1`
    -   Number of private subnets: `1`
    -   Public subnet CIDR: `10.0.0.0/24`
    -   Private subnet CIDR: `10.0.1.0/24`
    -   NAT gateways: **In 1 AZ**
    -   VPC endpoints: **None**
    -   DNS hostnames and DNS resolution: **Enabled**
7.  Verify the preview contains:
    -   VPC: `lab-vpc`
    -   Public subnet: `lab-subnet-public1-us-east-1a`
    -   Private subnet: `lab-subnet-private1-us-east-1a`
    -   Route tables: `lab-rtb-public` and `lab-rtb-private1-us-east-1a`
    -   Internet Gateway: `lab-igw`
    -   NAT Gateway: `lab-nat-public1-us-east-1a`
8.  Choose **Create VPC** and wait until all resources are created.
9.  Choose **View VPC**.

### Task 2: Create Additional Subnets

1.  In the VPC console, select **Subnets → Create subnet**.
2.  Create the second public subnet with these settings:
    -   VPC ID: `lab-vpc`
    -   Subnet name: `lab-subnet-public2`
    -   Availability Zone: Second Availability Zone (for example,
        `us-east-1b`)
    -   IPv4 CIDR block: `10.0.2.0/24`
3.  Choose **Create subnet**.
4.  Create the second private subnet with these settings:
    -   VPC ID: `lab-vpc`
    -   Subnet name: `lab-subnet-private2`
    -   Availability Zone: Second Availability Zone (for example,
        `us-east-1b`)
    -   IPv4 CIDR block: `10.0.3.0/24`
5.  Choose **Create subnet**.
6.  Navigate to **Route tables** and select
    `lab-rtb-private1-us-east-1a`.
7.  Open **Subnet associations → Edit subnet associations**.
8.  Keep `lab-subnet-private1-us-east-1a` selected and also select
    `lab-subnet-private2`.
9.  Choose **Save associations**.
10. Select `lab-rtb-public`.
11. Open **Subnet associations → Edit subnet associations**.
12. Keep `lab-subnet-public1-us-east-1a` selected and also select
    `lab-subnet-public2`.
13. Choose **Save associations**.
14. Verify the routes:
    -   Public route table: `0.0.0.0/0` points to the Internet Gateway
        (`igw-xxxxxxxx`).
    -   Private route table: `0.0.0.0/0` points to the NAT Gateway
        (`nat-xxxxxxxx`).

### Task 3: Create a VPC Security Group

1.  In the VPC console, select **Security groups**.
2.  Choose **Create security group**.
3.  Configure the following:
    -   Security group name: `Web Security Group`
    -   Description: `Enable HTTP access`
    -   VPC: `lab-vpc`
4.  Under **Inbound rules**, choose **Add rule**.
5.  Configure the rule:
    -   Type: `HTTP`
    -   Source: `Anywhere-IPv4`
    -   Description: `Permit web requests`
6.  Choose **Create security group**.

### Task 4: Launch a Web Server Instance

1.  Search for **EC2** and open the EC2 console.
2.  Choose **Launch instance**.
3.  Enter the instance name: `Web Server 1`.
4.  Select the default **Amazon Linux 2023 AMI**.
5.  Select instance type: `t2.micro`.
6.  Select the key pair: `vockey`.
7.  Under **Network settings**, choose **Edit** and configure:
    -   VPC: `lab-vpc`
    -   Subnet: `lab-subnet-public2`
    -   Auto-assign public IP: **Enable**
    -   Firewall: **Select existing security group**
    -   Common security groups: `Web Security Group`
8.  Keep the default storage settings.
9.  Expand **Advanced details** and paste the following script into the
    **User data** box:

``` bash
#!/bin/bash
# Install Apache Web Server and PHP
dnf install -y httpd wget php mariadb105-server

# Download Lab files
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip
unzip lab-app.zip -d /var/www/html/

# Turn on web server
chkconfig httpd on
service httpd start
```

10. Choose **Launch instance**.
11. Choose **View all instances**.
12. Wait until `Web Server 1` displays **2/2 checks passed**.
13. Select `Web Server 1` and open the **Details** tab.
14. Copy the **Public IPv4 DNS** value.
15. Open a new browser tab, paste the Public DNS value, and press Enter.
16. Verify that the web page displays the AWS logo and instance
    metadata.

## Important Configuration Summary

  Resource           Configuration
  ------------------ ------------------------------
  VPC                `lab-vpc`
  VPC CIDR           `10.0.0.0/16`
  Public subnet 1    `10.0.0.0/24`
  Private subnet 1   `10.0.1.0/24`
  Public subnet 2    `10.0.2.0/24`
  Private subnet 2   `10.0.3.0/24`
  Internet Gateway   `lab-igw`
  NAT Gateway        `lab-nat-public1-us-east-1a`
  Security Group     `Web Security Group`
  EC2 Instance       `Web Server 1`
  AMI                Amazon Linux 2023
  Instance Type      `t2.micro`
  Key Pair           `vockey`

## Result

Successfully created a VPC with public and private subnets across two
Availability Zones, configured route tables and a security group, and
launched an EC2 instance running a web server.

## Submission

1.  Choose **Submit** at the top of the lab instructions.
2.  Select **Yes** to confirm.
3.  Check the score under **Grades**.
4.  Open **Submission Report** to view detailed feedback.
5.  Choose **End Lab** and confirm.
