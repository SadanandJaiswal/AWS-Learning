# AWS RDS (Relational Database Service)
It is a fully managed database service provided by AWS that make it easy to setup, operate and scale relational databases in the cloud.

Instead of installing and managing a database server yourself, AWS handles tasks such as:
- Database provisioning
- Operating system updates
- Database patching
- Automated backups
- High availability
- Scaling
- Monitoring
- Disaster recovery

Supported Database Engines :
- Aurora (MySQL Compatible)
- MySQL
- PostgreSQL
- Microsoft SQL Server
- Aurora (PostgreSQL Compatible)
- MariaDB
- Oracle
- IBM Db2


### Create RDS Instance
- Aurora and RDS service > Databases > Create Database
- Select Database Engine > Creation Method (Easy Creat / Full Configuration)
- Select Templates (Production  `OR`  Dev/Test  `OR`  Free tier)
- Availability and durability :
        - Multi-AZ DB cluster deployment (3 instances) : All AZ will have RDS instance running to support read action
        - Multi-AZ DB instance deployment (2 instances) : All AZ will have RDS instance but on stand by, if one fail other is in use
        - Single-AZ DB instance deployment (1 instance) : Only one RDS instance is running
- Engine Version
- Create Master Credentials (username:admin `AND` passwrod:RDSMySQLPassword)
- Instance Configuration  AND Storage Configuration
- Select Connectivity (Don't connect to an EC2 compute resource  `OR`  Connect to an EC2 compute resource)
- Create RDS Instance


### Create EC2 Instance (node app)
- Create Instance > Connect EC2 instance
```bash
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl status docker
sudo docker pull philippaul/node-mysql-app:02
sudo docker images

# Run the docker with pulled image application with setting environment variable for database
# DB_hostname is Database Endpoint 
sudo docker run --rm -p 80:3000 -e DB_HOST="database-1.cd2gmowggfq5.eu-north-1.rds.amazonaws.com" -e DB_USER="admin" -e DB_PASSWORD="RDSMySQLPassword" -d philippaul/node-mysql-app:02

sudo docker ps
sudo docker logs -f <container_name>
```
- Open the application with http: public ip of instance
- Perform Operation on RDS database from cli from EC2 instance
```bash
sudo docker run -it --rm mysql:8.0 mysql -h database-1.cd2gmowggfq5.eu-north-1.rds.amazonaws.com -u admin -p
```
