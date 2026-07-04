# Service 1 : AWS IAM
Identity and Access Management is a service used to securely control access to AWS resources.
It allows you to manage users, roles and permissions to define who can access what in your aws environment.

- Free Service
- Global Service (can be used from and in any region)
- Root User should not be used or shared

- Create Users: You can create individual user accounts for people who need access to your AWS resources.
- Assign Permissions: You can assign specific permissions to users, groups, or roles to control what actions they can perform on AWS services.
- Create Groups: You can group users together and assign permissions to the group, making management easier for multiple users.
- Create Roles: You can create roles to assign temporary permissions to AWS services or users, especially useful for securely managing permissions across different AWS resources.
- Define Policies: You can create and attach custom policies to define fine-grained permissions for controlling access to AWS resources.
- Manage Federated Access: IAM allows integrating with external identity providers (like Active Directory) for centralized management of user access across AWS.


### IAM User
- If admin role : can create delete groups, users, etc
- Can't access some things of AWS Account : 
    - Cost center
    - 

### IAM MFA (Multi factor Authentication)
- MFA (Multi-Factor Authentication) is an extra layer of security that requires users to provide two or more forms of verification, like a password and a code from their phone, to access their accounts.
- Username + Password + Security Code

### Ways of Accessign AWS
- The AWS Management Console provides a graphical, web-based approach.
- The AWS CLI provides a command-line, scripting approach.
- AWS SDKs and APIs offer programmatic, code-based access, allowing users to integrate AWS directly into their applications.


### AWS CLI
- Download CLI from google : aws official page
- Install the aws cli msi installer
- verify on cmd : aws --version

### AWS CLI Configure
- Go to AWS Console
- Select IAM Users > Select the user > Security Credentials
- Access Key > Create access key
- *For Temperory* : aws login command and login using session
- *For Longterm* : aws configure and provide access key name, secret key, region (eu-north-1)


### IAM CLI Commands
- aws iam list-users : 

### AWS IAM Best Practices
- Avoid using root account except of account setup.
- Add user to a group and assign permission to group
- Use password policy or MFA
- Use ACCESS KEYS for CLI/SDK
- Never share ACCESS KEYS or Password
- Audit the permission using IAM credential report.
