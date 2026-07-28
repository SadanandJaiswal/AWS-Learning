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
    - ges unique version ID
- This create new object (updated) with version. Meaning if u updated index.html, there will be two index.html (earlier one) and updated one (with new version ID).
- When current used version is deleted, last older versoin will replace it