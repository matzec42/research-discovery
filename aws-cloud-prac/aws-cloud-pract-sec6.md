# AWS Certified Cloud Practitioner Certification (CLF - C02 Exam) - Notes, Research

# Section 6 - AWS Storage Services

## 44. Introduction
- S3 (object storage) is big focus on many AWS exams. (OOP)

## 45. Block vs File vs Object Storage
- Block: hard disk drive (HDD) or solid state drive (SSD); HDD still used with cloud, slower than SSD and cheaper, an older technology. Uses flash memory; newer, much faster than HDD and more expensive (better performance)
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
- **See slide deck diagram**. EC2 instance and Volume in the same AZ. Snapshot is taken to cpature a point-in-time state of the data on that Volume. Snaps stored in Amazon S3, which is regional (not in the AZ where the EC2 and Volume are). You can take multiple Snapshots (incrementally copy data).
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
- 

## 50. Amazon Elastic File System (EFS)

## 51. Amazon Simple Storage Service (S3)

## 52. Amazon S3 Storage Classes

## 53. Working with S3 Buckets and Objects

## 54. S3 Versioning, Replication and Lifecycle Rules

## 55. Configure Replication and Lifecycle

## 56. Create an Amazon S3 Static Website

## 57. S3 Permissions and Bucket Policies

## 58. Amazon FSx

## 59. Archiving with S3 Glacier

## 60. AWS Storage Gateway

## 61. AWS Elastic Disaster Recovery

## 62. Exam Cram

## Quiz 2: AWS Storage Services

## 63. Cheat Sheets