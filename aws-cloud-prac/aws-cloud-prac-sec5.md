# AWS Certified Cloud Practitioner Certification (CLF - C02 Exam) - Notes, Research

# Section 5 - AWS Compute Services

## 28. Introduction

## 29. Server Visualization
- Ex. of regular server (hardware), operating system (OS), application / service. Limitations: OS is tied to the hardware, difficult to move the app/service to another piece of hardware; underutilized resources, higher costs; scalability constraints (have to add physical components to server, e.g.); longer deployment time
- Server with virtualization --- server + hypervisor (creates a layer of abstraction between physical server and the virtual server/VM) + VM (virtual machine, hypervisor recreates the server virtually/non-physically on the VM) + Operating System & Webservice
- You can run multiple VMs (and therefore, many operating systems and apps/services) on one server
- You gain portability --- VMs can be moved to other servers/hypervisors
- Better resource utilization at a lower cost, quick deployment times
- Been around for 20+ years now, a well-established technology. It underpins the cloud --- many AWS services run on top of hypervisors

## 30. Amazon EC2 Overview
- Service in which we run EC2 instances (virtual servers) in the cloud
- EC2 host servers ---- managed by AWS, with Zen and Nitro hypervisors, run on physical servers
- You as an AWS user manage EC2 instances
- They come with different types; ranges of CPU, memory, storage and networking capabilities
- An example of IaaS --> infrastructure as a service
- AWS manages the hardware/infrastructure, you manage the operating system & operations on top of it

RE: Networking --- Public, Private and Elastic IP addresses
- **See chart on slides**
- **Public IP address** -- lost when isntance is stopped, used in public subnets, no charge, associated w/ the private IP address, can't be moved between instances
- **Private** -- retained whn instance is stopped (more permanent), used in both public and private subnets
- **Elastic** -- static public IP address, you're charged even if it's not used, associated with a private IP address on the EC2 instance

- Public subnets w/ diagram --- you can choose which AZs and subnets to launch instance into.
- Internet gateway --- attached to the VPC (virtual private cloud), pathway out to the internet.
- Public subnet route table --- a local route destination, and igw-id (internet gateway ID, which relays all other communcation out to the internet; e.g., computer's browser communicates to the EC2 instance over the internet, via the gateway)

- Private subnets --- no internet connectivity by default, only private IP addresses (no public addresses). Great for many use cases, we want to deploy into these as often as possible and use a **load balancer** into the public subnets.
- Use them as much as possible for security reasons.
- Sometimes though, your instances need to connect to the outside world (e.g., for a service or updates from the internet)
- **Nat gateway** is an AWS-managed device. Private subnet has its own route table (good practice to make it as such).
- Nat gateway conveys outbound traffic but does not allow inbound (for security)

## 31. Launching Amazon EC2 Instances
- Steps (**see diagram**)
1. Select instance type.  For these lessons, t2.micro
2. Select an Amazon Machine Image (AMI), which defines the configuration of the instance (operating system, any software, etc.; e.g., a Windows OS with 8 SQL databases).  **Think containers/images like Docker.**. Actual data stored in EBS Snapshots, a point-in-time back up of an EC2 instance.
3. You can then customize your instance later on.

- Demo --- created an EC2 instance (LinuxServer) with a key pair (not always necessary, can often connect Linux instances via cloud shell). Network settings were left on Linux, but for Windows we selected from an existing security group that we created (WebAccess)
- For Windows EC2 instance (WindowsServer in the demo), choosing or creating a key pair is necessary to connect (a key pair is used to decrypt the admin password, the decrypted password then connects to your instance).
- **Key pairs** get downloaded to your local machine after creation, store in a safe place.
- RE: connecting to Windows Server w/ remote desktop protocol. Click on WindowsServer instance, click on Security tab and click on the Security group ID (or go to it on left-side nav links). We added a rule to the Security group for Inbound --- connection allowed via RDS (remote desktop protocol) on port 3389 (AWS auto created/chose this port) and we chose IPv4.

## 32. Connecting to Amazon EC2
- How to connect to instances using secure shell protocol and remote desktop protocol
- Demo video

RE: connecting to the LinuxServer
- Connect to the EC2 instance from the outside world via the public IP address or the public IP DNS address.
- Options: EC2 Instance Connect, Session Manager (using the system's manager service, very secure way to connect without opening any ports), SSH client (e.g., connecting from your home computer; requires your private key pair and running the provided command it gives in the directions)
- With **EC2 Instance Connect**, 2 options; we're doing the one that has a public IP address, which allows us to connect directly from the internet
- Click Connect ---> generates a Linux terminal where you can interact with the EC2 instance. e.g., run command **ifconfig** and it shows the IP addresses associated with the instance; run ping google.com to prove connectivity (remember to press Control + C keys to stop this! :P )
- No issues for me, but connectivity issues might include: your instance doesn't have a public IP address, port 22 must be open in your Security group(s) used for this (check security port 22 and the source should be all zeros 0.0.0.0/0, indicating full/unrestricted access at the port)

RE: connect to WindowsServer
- There is a Session Manager option (for remote powershell) here too, but we're doing the RDP client
- Click RDP Client --> download remote desktop file (click button)
- **NOTE:** He says for Mac computers, you need to download RDP Client for Mac; this is outdated, you now just download and install Windows App.
- Copy the Public DNS Name from EC2 Connect page
- Open Windows App --> + drop down in upper right, choose Add PC --> paste the Public DNS name you copied --> creates a saved PC
- Double click to connect and it'll prompt you to give username and password
- Back in EC2, click on Get password near the bottom of the page --> then Upload private key file (that's from the previous lesson when you created a key pair)
- Click Decrypt password, copy it, log in with Administrator and the password
- You're now **logged into the desktop of a (remote) Windows server on AWS.**
- Troubleshooting: check that port 3389 is open, source is anywhere (all zeros).
- We terminated the WindowsServer instance, kept the LinuxServer for future lessons.
- **NOTE:** you pay for the storage allocated to a server, but not paying for the running compute or memory of a terminated server.

## 33. Amazon EC2 User Data and Metadata
- EC2 Console --> you can configure / run code (script) as instance is launched to track user data
- Metadata --- info about the EC2 instance available at http://169.254.169.254/latest/meta-data
- Can run a command (curl command followed by the URL) with this URL (doesn't connect to a network/internet, just a path that locally tells you about the instance)
- Example he gives of a couple commands---adds more to the end of the URLs, you can get the public or private IP address for that instance
- **User data** --> run commands when system is starting only (limited to 16 kb)
- **Metadata** --> returns info about the instance that is recorded locally

## 34. Create a Website with User Data
- Launching a Linux instance with a customized web server
- Copies user-data-web-server.sh raw code from the amazon-ec2 directory in the course repo (contains package/dependencies, starts a custom Apache server, fetches the AZ info --> assigns to a variable upon authentication, and then AZ info is fetched and gets assigned to that variable), HTML file is created
- EC2 --> launch instance --> choose instance type, key pair (skipped for this demo), i.e. the usual set up steps --> choose a Security group (we chose our previously made WebAccess, will reconfigure this later) --> Advanced details section
- Paste in code or upload file for customized server
- Security tab for that instance, created a new rule (HTTP access for anywhere; port 80 gets assigned for a web page)
- Click on the public IPv4 address (Details tab when looking at instances on Instances page), opens up the web page
- Timed out in regular browser, but works in Incognito window... :P
- **Takeaway:** you can run scripts when launching an EC2 instance using the User data section

## 35. Access Keys and IAM Roles with EC2
- EC2 instance in a public subnet in this example, trying to securely access an S3 storage bucket
- AWS CLI is configured with access keys for the instance (for security)
- Access keys are associated with an IAM user account; recall permissions are assigned to the IAM user
- Whatever commands run on the instance will pick up/have same permissions as the user
- Problem: access keys are long-term credentials and if compromised, it's a real security risk; they are stored in plain text on the instance itself / on the computer. (!)
- Solution: IAM Roles with policies assigned to them. This allows permissions to be supplied to the EC2 instance without the exposure of credentials (they aren't stored on the instance itself)
- Doing this, the instance assumes the IAM Role to access the S3 bucket---use this method whenever possible (preferable to using a user's long-term access keys)
- Recall: IAM Roles is using AWS STS (Security Token Service) in order to get credentials, but they're shorter term; the instance will automatically re-negotiate with STS to get refreshed credentials

## 36. Practice with Access Keys and IAM Roles
- Connecting to the previously made LinuxServer we made
- We run command aws s3 ls ---> gives us an "Unable to locate credentials..." ---> verifying that we don't have permissions
- Recall: access keys is one method and Roles the other/preferred

- **Demo using access keys:** IAM ---> Users, choose a user (Chris) --> click Security details --> Access Keys section, click Create Access Key --> Use case is Command LIne Interface (CLI) (ignore the warning, we know it's bad, it's just for demo :P ).
- This is essentially like the username and password (Access key and Secret access key). Recall this is what you can download as a .csv file
- We run aws configure, enter the credentials; region is us-east-1; press enter to get a clean line, we run aws s3 ls again, no error but nothing happens means you have permissions
- Created a bucket from the command line (aws s3 mb s3://mybucket-randomtextgoeshere); check with aws s3 ls and you'll see a bucket created
- We run cd ~/.aws --> goes up a level --> we run ls to see directories --> there are 2 (config and credentials)
- We run cat config --> see our default region; we run cat credentials --> oops, you can see credentials in plain text (if there was a server compromise, and they found this, then your entire AWS account is compromised)
- So, we change directories up with cd .. --> we run pwd to get to 'home base' user directory --> we run rm -rf ~/.aws/* to delete all files stored on the instance, so the credentials (access key and secret access key) aren't stored there anymore. Check this is correct by running aws s3 list, and we get that same "Unable to located credentials..." message, as expected.
- Went back into IAM and deleted the access key (deactivated it first, then deleted)

- **The proper way: demo using IAM Roles**
- IAM ---> Roles, create role ---> Use case is AWS service, EC2 --> search/select AmazonS3ReadOnlyAccess
- Look at the **Trust policy**, recall that it defines who or what assumes the role (property on the JSON object "Action": "sts:AssumeRole" is the part that says a person with this role has access to whatever it's for, in this example S3 read-only access; the "Principal" property defines the who or what gets this access, in this example it's the service EC2 that we're giving access to)
- Go back to EC2, select the instance (LinuxServer) --> Actions --> Security, choose Modify IAM role on drop down
- Select S3ReadOnly for IAM role, then click Update IAM role button
- Back in the LinuxServer CLI, we run aws s3 ls command --> we see the bucket with random text name that we made in the first demo (access keys method).
- **Takeaway:** creating a new Role with a policy that grants the kind of permission you want is safer, automatic (managed by AWS) and more importantly, credentials are not stored on the EC2 instance or on the computer itself in plain text, where they could be compromised.

## 37. AWS Batch
- 

## 38. Amazon LightSail

## 39. Docker Containers and Microservices

## 40. Amazon Elastic Container Service (ECS)

## 41. Launch Docker COntainers on AWS Fargate

## 42. Exam Cram

## Quiz 1: AWS Compute Services

## 43. Cheat Sheets