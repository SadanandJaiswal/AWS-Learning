# AWS EBS (Elastic Block Store)
AWS EBS is a cloud based storage service that provides durable, high performance block storage for use with Amazon EC2 Instances.

It work like a virtual hard drive, allowing you to store and access data even when your EC2 instance is stopped or terminated.

We can use the EBS volume of other stopped or terminated EC2 instance to another EC2 Instance. We can have attach multiple EBS to a EC2 instance. 

**For Example** : You need to host your MySQL or PostgreSQL database, you need reliable, high performance storage to handle frequent read/write operations. 

EBS provide persistent, fast storage that ensure your data is saved even if the EC2 instance is stopped or restarted, making it ideal for database workload.


### Key points of EBS

- IT is Region and AZ (Availability Zone) Specific
    - Mumbai have three zone (A,B,C) if EBS is on A zone of Mumbai, you can use it on B or other zone of Mumbai as well
- Build In Redundancy
    - EBS Volume is automatically replicated within the same availability zone to prevent data lose due to hardware failure.
- Different Volume Types 
    - gp2/3 (general purpose), io1/2(input output), sc1, st1
- Allow Encryption and Snapshot for backup
- Scalable (Volume can be resizeable)
    - No data loss will occur during resizing
    - No need to restart the EC2 instance during the process


### Delete on Termination
- By Default set to yes, it delete the EBS Volume when EC2 instance is terminated
- How To set
    - While creating Instance in Storage Configuration section, click on advane
    - Click on Volume and select Yes/No for delete on termination

### Creating EBS and Connecting to EC2 Instance
- **Create Volume** : 
    - In EC2 service's left navigation select Elastic Block Storage
    - Volumes > Create Volume
    - By default : Delete on Termination - No
- **Attach to Instance** : 
    - Go to Instances > Select Instance of same AZ of EBS Volume
    - Actions > Storage > Attach Volume
    - Select Volume and device name and attacch
- **Verify Attach** : 
    - Select the Instance from Instances Tab
    - Scroll Down > Storage
    - Block Devices
- **Note** :
    - To use the attached volume with instance, need to mount it


### EBS Snapshot
- EBS Volume is backedup to use incase of EBS Volume or any hardware failure
- Also you can use it to copy the data to
    - New Region 
    - New AZ (Availability Zone)

### Create and Copy Snapshot, to different availability zone (Usecase1)
- **Create** : Volumes > Actions > Create Snaphot
- **New Volume** : Select Snapshot > Actions > Create Volume from Snapshot (Volume can be increase not descreased)


### Mount Attached volume to EC2 Instance
- `lsblk` : show the disk and its partition, disk : nvme1n1 & partition : nvme1n1p1
- `sudo file -s /dev/nvme1n1p1` : /dev is for device, command is to check 
- `sudo mkdir /mybackup` : create point for mount, i.e mount partition will be accesible from this dir
- `sudo mount -o nouuid /dev/nvme1n1p1 /mybackup` : mount to /mybackup
- `df -h` : Show the filesystem and mounted on of that filesystem
- `cd mybackup` : Access the disk partition from this directory


### Unmount EBS
- `sudo umount /mybackup`
- `sudo umount -l /mybackup` : if volume is in use


### Need to copy EBS in different Region (Usecase2)
- Select Volume > Action > Create Snapshot
- Select Snapshot > Copy Snapshot (Use to copy snapshot in different region)


### EBS Encryption


### AWS Data Lifecycle Manager
- If you want to take snapshot of data weekly or daily basis you are not going to do it manually again and again instead use Lifecycle Manager
- Free of Cost (no Charge)
- Go to Volumes > Tags > Create Tag (key,pari)
- Elastic Block Store > Lifecycle Manager
- Custom Policy > Next Step > Volume > Key Value  > Next
- Select Schedule, when to cretae snapshot
- Retention Period : After how long the snapshot gets deleted 
- Advance (Additional Charges) : Copy to other region


### Recycle Bin
If you deleted by accidently or intentionally EBS volume, it will be there in recycle bin according to the recycle bin policy created

**Note** Charges are applicable