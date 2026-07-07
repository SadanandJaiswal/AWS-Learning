# AWS EC2 (Elastic Compute Cloud)

Amazon EC2 (Elastic Compute Cloud) is an AWS cloud computing service that provides **resizable virtual servers**, called **EC2 instances**, to run applications without buying, maintaining, or managing physical hardware.

**note** : It is Region Specific. Only Instance for respective Region will be seen OR in Global View.

## Scenario

Imagine you have an online business and need to host your website or application.

### Way 1: Buy Physical Servers
- Purchase your own physical servers.
- Set up the hardware and networking.
- Install the operating system and software.
- Manage maintenance, security, upgrades, and scaling yourself.

**Drawback:** High upfront cost, ongoing maintenance, and limited scalability.


### Way 2: Use AWS EC2 (Recommended)
- Rent virtual servers (EC2 instances) from AWS.
- Choose the required CPU, memory, storage, and operating system.
- Launch the instance within minutes.
- Deploy and host your website or application.
- Scale resources up or down based on demand.
- Pay only for the resources you use.

**Benefit:** No hardware management, lower upfront cost, high availability, and easy scalability.


## Key Advantages
- Resizable virtual servers
- Pay-as-you-go pricing
- Easy to scale
- Highly available and reliable
- Secure networking and access control
- Supports Windows and Linux operating systems


### Terminologies
- Instance Type : Select the hardware capacity (CPU, memory)
- AMI - Amazon Machine Image : Choose the OS and software
- Storage : Configure the type and size of storage (e.g EBS Volume)
- Security Group : Set up firewall rules to control inbound/outbound traffic    
- Key Pair : Create and use an existing key pair for SSH access
- Network Settings : Configure VPC, subnet, and assign public and private IP addresses
- IAM Role : Attach an IAM Role for permission to access other AWS resources
- User Data : Add scripts to be executed when the instance is start
- Elastic IP : Optionally associate a static IP address for consistent public access


## Create Instance 
- EC2 Service > Launch Instance
- Instance Name > AMI (AWS Linux) > Instance Type (micro)
- Key Pair : to connect with this instance from your machine (download .pem file)
- Configure Network Settings
- Storage (8gb SSD)
- Number of Instance


### Using UserData 
- Run or perform certain task when instance is start
```bash
#!/bin/bash

sudo yum update -y

# Install server (httpd)
sudo yum install -y httpd

# Start Apache Server
sudo systemctl start httpd
sudo systemctl enable httpd

# Optional : create html file
echo "<html><h1>Welcome to the Apache Server on AWS Linux</h1></html>" > /var/www/html/index.html
```

**Instance Created** : i-00ec8bf118420a126


## Security Groups :
Network Firewall rules that control the inbound and outbound traffic for instance

- Click on instance > Security > Security Groups
- Edit inbound / outbound rule

**Port 80** : HTTP Request
**Port 20** : SSH Request
**Prot 443** : HTTPS Request

### Important points related to security groups
- Regin Specific Service
- Only allow rule (not deny rule)
- All inbound traffic are blocked and outbound allowed by default
- **Define Rules** for :
    - Protocols (HTTP, SSH, etc)
    - Port Numbers
    - IP addresses or range


## Connect with Instance

### 1. Using EC2 Instance Connect
- Selct instance
- Click connect > in Web Browser
- EC2 Instance connect > connect instance

### 2. Using SSH Client (Putty)
Connect from window to instance via SSH
- Download the SSH Client (Putty for windows)
- Convert the .pem file to .ppk using (PuTTygen)
- Open Putty > SSH > Auth > Credentials : provide private key (.ppk)
- Sessoin > Provide Public IP of instance and port (22) and click open
    - alternately u can use : ec2-user@PublicIP
- Now Instance cli is open, provide username : ec2-user

### 2.2 Using SSH Client (wsl)
- open cmd > ```ubuntu```
- Copy the Window file to ubuntu (~ location): 
    ```cp /mnt/c/Users/sadanandmansu.jaisw/Desktop/Learning/Aws-Learning/MyFiles/MyWebServer1-Key.pem ~/```
- Run the instance
```bash
# 400 (4 : owner permission, 0 : group permission, 0 : other permission)
chmod 400 MyWebServer1-Key.pem

ssh -i MyWebServer1-Key.pem ec2-user@<Instance PUblic IP>
```

#### Some ports you should be aware of
• HTTP (Port 80) - Unencrypted web traffic.
• HTTPS (Port 443) - Encrypted web traffic (SSL/TLS).
• SSH (Port 22) - Secure remote access to servers (Linux/Unix).
• FTP (Port 21) - File Transfer Protocol (unsecured).
• SFTP (PoFt 22) - Secure File Transfer Protocol.
• SMTP (Port 25) — Simple Mail Transfer Protocol (email sending).
• RDP (Port 3389) - Remote Desktop Protocol (Windows remote access).
• MySQL (Port 3306) - MySQL database connections.
• PostgreSQL (Port 5432) - PostgreSQL database connections.
• DNS (Port 53) — Domain Name System (converts domain names to IP addresses).


### AWS EC2 Commands
- Describe Instance : `aws ec2 describe-instances`
- Stop Instance : `aws ec2 stop-instance --instace-ids <i-instanceid>`
- Start Instance : `aws ec2 start-instance --instace-ids <i-instanceid>`
- Get New IP after starting : `aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]" --output table`