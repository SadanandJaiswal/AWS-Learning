# AWS S3 (Simple Storage Service)
It is a cloud based storage service that allows you to store, manage and retrive large amount of data like files, folders, images, videos, and backup securely and at scale.

It provides highly reliable, scaleable object storage, making your data accessible from anywhere, anytime, via the internet

It is Global Specific. 

## Key Concepts 
### Bucket
- A container for your data. (similar to top level folder)
- Store data as objects
- globally unique name
- It is **Region Specific**
- Replicate the data to all availability zone within the region.


### Object
- A file stored in S3
- Each object within a bucket is stored as key value pair
    - Key : name/path of the object
    - Value : content of the object (the file/data itself)
- Maximum size for single object is 5TB
- Multipart upload is recommeneded for objects larger than 5GB (split the file in smaller part and upload them seperately, `internal`)


### S3 Bucket Policies
JSON based access control policies that you attach directly to the bucket to manage permissions for accessing the bucket and its objects.

They allow you to define who can access the data and what actions they can perform, such as read, write, or delete, enabling fine-grained control over the security of data stored in S3. 

- GetObject : Used to retrieve or download files from S3 bucket.
- PutObject : Used to upload or add files into S3 bucket.


### Host Static Website
#### Upload File
- S3 sidebar : General Purpose Bucket > Uploads
- Select file (.html,.css,.png) and configure some setting if needed and upload
- To preview file : select file and click `open`

#### Host
- Selcte the bucket > properties > scroll to bottom
- Enable : Static website hosting
- select index document and save changes
- Get : bucket website endpoint

#### Make files and website public
- Select Bucket > Permissions > Edit Block Public access
- Disable Block (public access)

#### Create Bucket Policy
- Bucket > Permissions > Bucket Policy > edit
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::demobucket-user1-bucket1/*"     // Add /* in resouce as we want policy for objects within the bucket
        }
    ]
}
```


### S3 Versioning
- It allows you to keep multiple versions of an object in the same bucket, providing protection against accidential deletions or oerwrites.
- When versioning is engabled, S3 stores every version of an object, allowing you to recover the older version if needed, making it ideal for data safety and backup. 

- Enable Versioning 
    - Select Bucket > Properties > Edit Bucket Versioning > enable
    - Versioning will be done for objects that exist after versioning is enabled
    - get unique version ID
- This create new object (updated) with version. Meaning if u updated index.html, there will be two index.html (earlier one) and updated one (with new version ID).
- When current used version is deleted, last older versoin will replace it


### S3 Replication
It allow you to automatically copy objects from one S3 bucket to another, which can be 
- within the same region (SRR - Same Region Replication) OR
- in another region (CRR - Cross Region Replication)

It is commonly used for compilance, redundancy, and to improve data access performance by maintaining copies closer to your user


### Create Bucket Replica in Same/Cross Region
- Create new Bucket (same/cross region)
- Select the Bucket need to replicate > Management
- Replication Rules > Create Replication Rule
- Select source and destination bucket
- Save > Select to replicate already present file (if wanted)
- Select Generate Completion Report (if needed)
- Choose or Create role for permission


### S3 Storage Classes
Amazon S3 (Simple Storage Service) offers several storage classes designed for different access patterns, durability, and cost requirements.

| Storage Class                                        | Best For                                                | Access Frequency | Retrieval Time                               | Cost                                                      |
| ---------------------------------------------------- | ------------------------------------------------------- | ---------------- | -------------------------------------------- | --------------------------------------------------------- |
| **S3 Standard**                                      | Frequently accessed data                                | High             | Milliseconds                                 | Highest storage cost                                      |
| **S3 Intelligent-Tiering**                           | Data with unpredictable access patterns                 | Variable         | Milliseconds                                 | Automatically moves data between tiers to reduce costs    |
| **S3 Standard-IA (Infrequent Access)**               | Data accessed less often but needed quickly             | Low              | Milliseconds                                 | Lower storage cost, retrieval fee applies                 |
| **S3 One Zone-IA**                                   | Infrequently accessed, non-critical or recreatable data | Low              | Milliseconds                                 | Cheaper than Standard-IA, stored in one Availability Zone |
| **S3 Express One Zone**                              | High-performance, latency-sensitive workloads           | Frequent         | Single-digit milliseconds (very low latency) | Optimized for speed within one Availability Zone          |
| **S3 Glacier Instant Retrieval**                     | Archived data that still requires immediate access      | Rare             | Milliseconds                                 | Lower cost than IA, retrieval charges apply               |
| **S3 Glacier Flexible Retrieval** (formerly Glacier) | Long-term archives                                      | Very Rare        | Minutes to hours                             | Very low storage cost                                     |
| **S3 Glacier Deep Archive**                          | Compliance and long-term backup                         | Extremely Rare   | 12–48 hours                                  | Lowest storage cost                                       |
| **S3 on Outposts**                                   | Data that must remain on-premises                       | Varies           | Local access                                 | For AWS Outposts deployments                              |


### S3 Bucket LifeCycle
It is a set of rules to control the movement of objects from different storage classes or delete them entirely, based on specific conditions like age or inactivity.

#### Example : You have a bucket that stores photos.
You create these lifecycle rules:
- After 30 days → Move photos from S3 Standard to S3 Standard-IA to save money.
- After 90 days → Move them to S3 Glacier because they are rarely accessed.
- After 1 year → Delete them automatically.


### Create Bucket LifeCycle Rule
- Select the Bucket > Management > Create LifeCycle Rule
- Select the Activity to perform and provide condition


### S3 Snow Family
The S3 Snao Family is a group of a physicall devices offered by AWS to help move large amount of data to the cloud when using the internet isn't practical

These devices are used when there is too much data to upload over a regular connection or when dealing with remote area without good internet.

- **Snow Family Includes**
    - **AWS Snowcone** : A small portable device for few terabytes of data.
    - **AWS Snowball** : A large device for moving petabytes of data and can also be used for edge computing.
    - **AWS Snowmobile** : A massive truck size container used for exabyte-scale data transfers, typically used by big companies moving entire data centers.

These devices help you transfer data quickly, securely, and cost effectively to AWS, especially when internet speed or reliability is an issue.


### S3 Storage Gateway
It is a hybrid cloud storage service that connects on premises environment to cloud storage on Amazon S3. It helps extend local storage to the cloud by acting as a bridge.

![alt text](Assets/s3_storage_gateway.png)

- **Types of Gateway**  :
    - **Amazon S3 File Gateway** : Store and access objects in Amazon S3 from NFS or SMB file data with local caching.
    - **Tape Gateway** : Store virtual tapes in Amazon S3 using iSCSI-VTL, and store archived tapes in Amazon S3 Glacier Flexible Retrieval Amazon S3 Glacier Deep Archive.
    - **Amazon FSx File Gateway** : Access fully managed file shares in Amazon FSx for Windows File Server using SMB.
    - **Volume Gateway** : Store and access iSCSI block storage volumes in Amazon S3.

#### Full Form
| Term | Full Form | Purpose |
|------|-----------|---------|
| **NFS** | **Network File System** | A file-sharing protocol commonly used in Linux and UNIX systems. It allows users to access files over a network as if they were stored locally. |
| **SMB** | **Server Message Block** | A network file-sharing protocol primarily used by Windows systems to share files, printers, and other network resources. |
| **iSCSI** | **Internet Small Computer Systems Interface** | A block storage protocol that carries SCSI commands over IP networks, allowing remote storage devices to appear as locally attached disks. |
| **VTL** | **Virtual Tape Library** | A software-based tape library that emulates physical tape drives, enabling existing backup software to store backups on virtual tapes in the cloud. |
| **Amazon FSx** | **Amazon File System X** | A fully managed AWS service that provides high-performance file systems for workloads requiring shared file storage, such as Windows, Lustre, NetApp ONTAP, and OpenZFS. |
