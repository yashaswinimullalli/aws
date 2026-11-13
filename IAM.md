
# Experiment: AWS Identity and Access Management (IAM)

## Aim
To explore AWS IAM users, groups, and policies, assign users to appropriate groups, and verify their access permissions.

## Procedure

### Task 1: Explore IAM Users, Groups and Policies

1. Start the AWS Lab and open the AWS Management Console.
2. Search for **IAM** and open the IAM Dashboard.
3. Navigate to **Users** and observe the existing users:
   - user-1
   - user-2
   - user-3
4. Navigate to **User groups** and observe the following groups:
   - EC2-Admin
   - EC2-Support
   - S3-Support
5. Open each group and examine its attached permissions and policies.
6. Identify the permissions assigned to each group.

### Task 2: Assign Users to IAM Groups

1. Open the **S3-Support** group and add user-1.
2. Open the **EC2-Support** group and add user-2.
3. Open the **EC2-Admin** group and add user-3.
4. Verify that all users are assigned to their respective groups.
5. Navigate to the IAM Dashboard.
6. Copy the **IAM Sign-in URL** for testing user permissions.

### Task 3: Test IAM User Permissions

1. Open an Incognito window and access the IAM Sign-in URL.
2. Log in as **user-1** and verify:
   - S3 resources can be viewed.
   - EC2 access is denied.
3. Log out and log in as **user-2**.
4. Verify:
   - EC2 instances can be viewed.
   - Stopping an EC2 instance is denied.
   - S3 access is restricted.
5. Log out and log in as **user-3**.
6. Open the EC2 Dashboard and select the LabHost instance.
7. Choose **Instance state → Stop instance**.
8. Confirm the operation and verify that the instance enters the stopping state.


## User and Group Mapping

| IAM User | IAM Group | Permissions |
|---|---|---|
| user-1 | S3-Support | S3 Read-only |
| user-2 | EC2-Support | EC2 Read-only |
| user-3 | EC2-Admin | EC2 View, Start and Stop |

## Result

Successfully explored AWS IAM users, groups, and policies, assigned users to appropriate groups, and verified access permissions for AWS services.

## Important Concepts

| Concept | Description |
|---|---|
| IAM | Identity and Access Management |
| IAM User | Individual identity in AWS |
| IAM Group | Collection of IAM users |
| IAM Policy | Defines permissions for AWS resources |
| Managed Policy | Standalone reusable policy |
| Inline Policy | Policy embedded directly into an IAM identity |
| Least Privilege | Granting only the permissions required |


