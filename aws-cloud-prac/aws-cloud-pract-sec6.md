# AWS Certified Cloud Practitioner Certification (CLF - C02 Exam) - Notes, Research

# Section 6 - AWS Storage Services

## 44. Introduction
- S3 (object storage) is big focus on many AWS exams. (OOP)

## 45. Block vs File vs Object Storage
- Block: hard disk drive (HDD) or solid state drive (SSD); HDD still used with cloud, slower than SSD and cheaper, an older technology. SSD uses flash memory; newer, much faster than HDD and more expensive (better performance)
- Our computers have their own HDD. The OS partitions & formats storage into volumes and different operations (OS, C: drive, D drive, etc.)
- File-based: there's block-based storage somewhere and a file-based storage is built on top of it; there's a network that connects a client computer with a Network Attached Storage Server (with its own disk drives). File system of computer is shared over the network, mounted to the OS. Ex. of a network location (share(\\ABC)(Z:)) through which a file system is being shared. Computer can interact with it just as if it's on the machine (e.g., local disk drive C or D) itself.
- **Object Storage Systems:** user uploads objects using a web browser to an object storage container via HTTP protocol, using a REST API. Objects can be files, videos, images, PDFs, text docs, etc. AWS S3 supports all file types; there is no hierarchy, they all just live in the buckets.
- Object storage is great --- easy to work with & interact with programmatically via HTTP/REST API, massively scalable and low cost

## 46. Amazon EBS and Instance Stores
- Ex. of Availability Zone (AZ), in which we've got 2 Volumes and an EC2 (server) instance
- C drive and D drive on the EC2 instance; they appear to be local disks, but in reality they're connected across the network (block-based, not file-based), the volume is attached over a network.
- Another ex. (Amazon EBS vs Instance Store) --- EBS Volumes are mounted across the network. Instance Store volumes are physically attached to the host server (EC2 Host Server), they are ephemeral (non-persistent).
- EBS Volumes are persistent
- Amazon EBS Snapshots --- backing up data. Taken to capture a point-in-time state of an instance (make a copy of the data in that volume at that instance in time). The Snap is stored in S3, not in the same place. Snapshots are incremental.
- From snapshots, we can then move/create an EBS volume in another AZ; that same data would be visible in another EC2 instance in a different AZ.
- You can also create an AMI (Amazon Machine Image) which will help automate the creation of new Volumes using Snapshots in different areas within a region

## 47. Amazon EBS Snapshots and DLM
- Taking backups of EBS volumes through Snapshots, something which you can automate with DLM (Data Lifecycle Manager)
- **See slide deck diagram (129)**. EC2 instance and Volume in the same AZ. Snapshot is taken to cpature a point-in-time state of the data on that Volume. Snaps stored in Amazon S3, which is regional (not in the AZ where the EC2 and Volume are). You can take multiple Snapshots (incrementally copy data).
- We can use Snapshots to create new Volumes and attach them to new EC2 instances in new AZs.
- More on automating this with AMI later.
- How to automate this sort of backup process? **Amazon DLM**
- DLM automates creation, rentention and deletion of EBS Snapshots and EBS-backed AMIs
- Protects data w/ regular backup schedule; create standardized AMIs that can be refreshed at regular intervals; retain backups as required by auditors or internal compliance; reduce storage costs by deleting outdated backups
- Create disaster recovery backup policies that back up data to isolated accounts (in a different account, region, etc.)
- Not a major topic of the exam, but good to know about Snapshots

## 48. Create and Attach an EBS Volume
- Demo --- create two EC2 instances in different AZs (east 1a and east 1b). Then, left side click on Elastic Block Store and choose Volumes; we created a new Volume
- Connect to instance 1A (Instances --> choose 1A, click Connect, opens up the Linux terminal/CLI). We run command sudo lsblk -e7
- Attach the Volume and re-run that same command --- we see that we've attached a new Volume that's 10 GB.
- Since we have a disk but no file system yet, we do so by running command sudo mkfs -t ext4 /dev/xvdf (see training repo ReadMe for this section)
- Then, create a mount point by running sudo mkdir /data
- We mount the EBS volume to the mount point using the command sudo mount /dev/xvdf /data
- Make it persistent running the command sudo nano /etc/fstab, then add /dev/xvdf /data ext4 defaults,no fail 0 2
- Change directory to /data, there's nothing there but we create a testfile.txt, make a directory (myfolder) just to have 'data' in there
- Back in EC2 ---> Volumes, click Actions, choose Create snapshot --> you can check it on Snapshots (left side nav under Elastic Block Store). Recall they're stored in S3 regionally, made to be available w/in any AZ.
- Then, we create a new Volume in east 1b (another AZ) using the Snapshot we made
- Snapshots --> Actions, choose Create volume, choose AZ us-east-1b
- Instances --> connect to the 1B server, pull up a terminal --> we run sudo lsblk -e7 to see that the Volume exists there (the 10 GB volume one we made)
- We **don't** need to make a file system, just mount the disk following the steps above; you could run that last command to make the data persistent on the instance, but we didn't do that.
- Clean up --> deleted Snapshots, Volumes and terminated Instances (all of them) to avoid charges
- **Takeaway:** you can create a Volume in one AZ, take a Snapshot of it, attach it to a Volume in a diff. AZ in the same region, and access the same data. Snapshots as data backup.

## 49. EBS Snapshots and AMIs
- **Demo:** create an EC2 instance, then make an AMI from it so that we can spin up identical web servers whenever wanted/needed
- Created an instance w/o a key pair (recall, bad practice but just for demo), paste User data (see the amazon-ebs directory in the course repo), paste in the code at the bottom of the launch instance page (Advanced details). This code spins up a custom web server (Apache), provides the HTML, launches websites
- AMI (left side navigation) --> Actions --> Copy AMI (encryption optional)
- Once running, a Snapshot of the volume is created too.
- 2 ways --- directly through AMI and launch instance or, as we did in demo, through Instances (launched a new instance, usual set up, etc.). Made a new instance in a new AZ, using the Snapshot (that is, a copy of the volume/data from that point in time from the first EC2 instance we made). Tested it by launching the web page again.
- Clean up: deleted the two EC2 instances. With the AMI, **deregister it before deleting the Snapshot**. Prompt you to copy the Snapshot ID so that you can easily search for it in Snapshots (left hand side, under Elastic Block Store)
-**Takeaway:** you can create an AMI of an EC2 instance by using a Snapshot (copy of its Volume at a point in time), and then use that AMI to more easily spin up an identical server that has the same configurations (instance details, Security group, User data if any was uploaded, etc.)

## 50. Amazon Elastic File System (EFS)
- Shared file system (regional). We can connect EC2 instances to file system from multiple AZs
- Regional file systems have mount targets in multiple AZs
- Instances connect to a mount point in the local AZ; NFS (network file system) is **Linux only**
- **One Zone** file systems have mount targets in a single AZ

- **Amazon Elastic File System (EFS)**
- Data consistency --- durably stored across AZs
- File locking --- NFS client apps use NFS v4 file locking for read and write operations on EFS files
- Storage classes --- 3 options. Standard, Infrequent Access(IA), Archive
- Durability --- 11 9s of durability (likie S3). Unlikely to lose data.
- Deployment options: an EFS File System in a Region, connected to an EC2 instance.  Can be replicated across/in another Region; mount points can be created but the file system will be read-only (until you fail-over to use the copy of the data in that system in that Region; e.g., some sort of a system failure)
- Can also connect outside of cloud, must be Linux-based and use NFS protocol; typically using a VPN or direct connection (e.g., a corporate data center)
- EFS Replication --- data is replicated across Regions for disaster recovery purposes with RPO/RTO in the minutes
- Automatic Backup --- EFS does file system backups
- Performance options --- provisioned throughput (specify the amount of throughput to that file system, independent of its size); bursting throughput (throughput scales w/ the amount of storage and supports bursting to higher levels)

## 51. Amazon Simple Storage Service (S3)
- Object-based storage system; **buckets** are containers for objects (files of any format you upload)
- Millions of objects can stored in a bucket; easily scalable, cost effective
- 2 URL formats for S3 buckets (**see slide 141**).
- Key/value store (key --> object name, value --> data itself)
- Review: accessible via HTTP protocol used w/ a REST API over the web (like most everything else in AWS)
- Names of buckets must be unique across AWS --- recall that part of URL includes the bucket name
- Objects include key (name of object), version ID, value (actual data), metadata, subresources, access control info
- In a virtual private cloud (VPC), S3 sits outside of it (**slide 142**) in the public space of AWS. We access it via the internet; if you want to connect from an EC2 instance to S3, you'll hit the internet gateway and access S3 via a public endpoint
- Not great for private instances --- you can use nat gateways instead
- Creating an **S3 Gateway Endpoint** is good way to directly access S3 from and EC2 instance via a **private IP address**

- **File Storage vs Object Storage:**
- File: data stored in directories, in a hierarchy, and are mounted to an OS (makes it appear like a local disk drive/storage on your computer); network connection is maintained. Ex: Amazon EFS
- Object: data stored in buckets, flat namespace (no hiearchy, though using pre-fixes on keys can mimic this), accessible by REST API and **cannot** be mounted
- Network connection is completed after each request (that is, it isn't continuously maintained). Every time you want to interact with objects in storage, you have to make an API call

## 52. Amazon S3 Storage Classes
- Durability & Availability
- RE: Durability --- protection against data loss, data corruption. S3 offers 11 9s of durability (99.999999999). They achieve this by spreading copies of your data across many AZs.
- RE: Availabity --- measurement of the amount of time that data is available to use; expressed as % of time per year (e.g., 99.99%). Slides 145-146
- Diff. costs with diff. storage tiers. IA = infrequently accessed
- **Takeaway:** diff. tiers allow for diffl levels of durability, availability. Inteliigent Tiering means AWS will automatically monitor and assess how much data is used or not used and help optimize it for you. For IA / Glacier, you're lowering availability for longer time periods but for lower cost. (e.g., Glacier Deep Archive for compliance data that you don't need to access/use but legally have to hold for a certain amount of time/years)

## 53. Working with S3 Buckets and Objects
- **Demo:** creating S3 buckets and manipulating objects
- S3 --> Create Bucket --> give it a unique name, kept it General purpose, kept Default encryption (server-side encryption); Advanced settings left it
- In a Bucket, you can upload objects (files); define Bucket policies (resource-based permission policies, recall), other tabs for metrics and things
- We add files from the course repo (fruit pics)
- You can modify Permissions; under Properties, you can choose Storage class
- Created a folder in the S3 bucket, put raspberry in there just to see that S3 just mimics hierarchy (adds folder name to the file's Key, e.g., mydocuments/raspberry.jpeg). Meaningless to S3, for us it helps us with organization of our data.
- RE: bucket access. We went into Permissions, clicked Edit under the Block public access (bucket settings) section. Then it prompted us to Edit bucket policy (we made one by adding statement in the console and editing the JSON object that appears). Prinipcal to *, Action to s3:GetObject, Resource w/ the ARN that is supplied. This policy says all principals are allowed the Action (s3:GetOjbect), on the Resource (which is the bucket, represented by the bucket ARN with a slash * at the end).  We're allowing GetObjects on the objects themselves.
- Go back to Objects section of the Bucket, visit the URL for one of them, and you get the image.
- We enabled public access for the objects stored in S3

## 54. S3 Versioning, Replication and Lifecycle Rules
- **Versioning**: keeping multiple variants of an object in the same bucket. Enables you to recover objects from accidental deletion or overwrite
- **Replication**: cross-region replication (CRR). Can do so b/w buckets in diff. regions or w/in the same region; Buckets must have versioning **enabled**
- **Lifecycle management**: 2 types of actions. **Transition actions:** when objects transition to another storage class. **Expiration actions:** defines when objects expire (get deleted by S3)
- S3 has some pre-made supported transitions; see Resources folder linked to the video for more

## 55. Configure Replication and Lifecycle
- 

## 56. Create an Amazon S3 Static Website

## 57. S3 Permissions and Bucket Policies

## 58. Amazon FSx

## 59. Archiving with S3 Glacier

## 60. AWS Storage Gateway

## 61. AWS Elastic Disaster Recovery

## 62. Exam Cram

## Quiz 2: AWS Storage Services

## 63. Cheat Sheets