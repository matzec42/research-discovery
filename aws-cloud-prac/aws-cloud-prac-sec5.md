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

- Testing a new commit
- Do what you can, with what you have, where you are. ~ T.R.