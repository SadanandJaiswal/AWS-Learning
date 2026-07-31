# AWS RDS (Relational Database Service)
It is a fully managed database service provided by AWS that make it easy to setup, operate and scale relational databases in the cloud.

It is Region Specific

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
- Automatically Do Maintainance and Backup (create snapshots)


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

**note** : when u open the EC2 instance public url in browser then u get this node application, because we have mapped host(instance) 80 port with docker containers 3000 port and when we use url then its a HTTP method and its default port is 80. This result in application to open on public url of instance. If you use HTTPS then this will not show the application.


### Take Manual Snapshot, Restore DB, Copy Snapshot
**Create Snapshot**: 
- Aurora and RDS service > Side Bar > Snapshots
- Take Snapshot > provide db and snapshot name > done

**Restore Database from Snapshot**:
- DB snapshot maintain the full state of database (data, configurations) like replica
- Restore Database by selecting snapshot and then action > restore database

**Copy Snapshot** : 
- Select Snapshot > Actions > Copy Snapshot 
- Select the region where u want to copy the snapshot and it will create snapshot in that region.


### Benefits of RDS
- High availability and fault tolerance.
- Vertical and Horizontal Scaling
- Automated backups and recovery.
- Read replicas for improved read performance
- Multi AZ setup for DR (Disaster Recovery)
- Cost-effectiveness.


### Common Use Cases for RDS:
- Web Applications: Relational databases are ideal for web apps requiring structured data.
- E-commerce Platforms: For handling inventory, customer data, and order transactions.
- Business Applications: ERP, CRM, and financial applications with strong data integrity needs.