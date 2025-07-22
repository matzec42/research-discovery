# AWS Certified Cloud Practitioner Certification (CLF - C02 Exam) - Notes, Research

# Section 1

## 1. Intro.

## 2. The CLF-C02 Exam

### General exam info
- 90 mins, 65 questions (pretty straightforward)
- $100
- 50% off voucher for next cert
- Testing center or online (details are specific for latter)
- Scaled scoring model - 100 -- 1000 points, 700 to pass
- Multiple choice (1 correct, 3 incorrect) or multiple responses (2 or more correct options from up to 5 options)

### Domains covered in exam
Domain 1:
- Define benefits of cloud
- ID design principles of cloud
- Benefits and strategies of cloud migration
- Cloud economics concepts

Domain 2: Security and Compliance
- Understand AWS shared responsibility model
- Understand AWS Cloud security, governance, compliance
- ID access management capabilities
- ID components and resources for security

Domain 3: Cloud Tech and Services
- Define methods of deploying and operating in AWS Cloud
- Define AWS global infrastructure
- ID compute services, network services, database services, storage services
- AI/ML and analytics services
- ID other in-scope AWS service categories

Domain 4: Billing, Pricing, Support
- Understand pricing models
- Resources for billing, budget, cost management
- AWS technical resources & support options

## 3. Free Tier vs Sandbox/Challenge Lab
- Different levels of control; sandbox is hosted by a provider
- No risk of bills; optional, not required for this course, costs an additional fee (skip for now)

## 4. Course Download
- See Google Drive for exam cram study slides, full course slides, and study plan (pacing guide). Downloaded from the link provided in this section.
- Practice questions and exams available through Digital Cloud Training (company affiliated w/ the training)
- Some simulate real exam, others more scaffolded (training mode) that show answers, explanations, etc.
- Link to a separate Udemy offering ... paid practice exam Q's. Stick to the free stuff



# Section 2 - Create AWS Free Tier Account
## 5. Intro.

## 6. AWS Account Overview
- Unique email account and credit card; creates a root user
- Best practice -- strong password, avoid using root user (full control) acct --> use AWS IAM (identity & access mgmt) acct to create users, groups, roles, policies (more managed and secure)
- Login thru AWS Management Console, authenticate w/ AWS IAM

## 7. Create Free AWS acct
- Unsure of my acct w/ CS, B. seemed to have set it up and he created a role (?) for me. Existing IAM but not able to login
- Create a new acct. **NOTE:** free acct is good for 6 months, then either transition to paid plan or it gets closed automatically

## 8. Configure Account, Create a Budget Alarm
- Created an alias (see screenshots for IAM acct ID, acct alias, and sign-in URL for this IAM)
- Billing preferences (set alarm) and budget plan ($5 a month) under Bills

## 9. Creating IAM Users and Groups
- Creating a user group with certain permissions, access (IAM --> User groups --> made an Admins group)
- Assigning permission policies (written in JS object notation). Chose first option, all access
- Created a user, added to the Admins group, and practiced logging in using the sign-in URL and the individual IAM user account and custom PW that you made (downloaded in a .csv file as well---recall that AWS does not store these. Access keys can be created for different situations/use cases).

## 10. Install Tools and Configure AWS CLI
- Cloned the repo, open VS Code
- Install the AWS CLI interface --- Google AWS CLI ---> Install/Update --> download the relevant version (macOS)
- Access AWS CloudShell --- already authenticates you, makes it easier to access the CLI interface in the cloud (rather than having to use it on your computer)



# Section 3: Amazon Web Services Fundamentals

## 11. Introduction

## 12. The AWS Global Infrastructure
- **Regions** --- separate physical locations around world
- **Availability zones** (1 or more, usually 3) within regions --- physical data centers
- You can spread your data across different AZ's to mitigate failures, keep apps running
- **AWS Global Network** manages connections, bandwith and latency between regions

- Create **subnets** (public or private) also for launching resources

Other ways:
- AWS Outposts (hardware in non-AWS corporate data center); e.g., EC2 can be run on premises, connected back to the AWS region
- AWS Local Zones --- like AZs, get resources closer to where resources are being used/access, pay more for it but lower latency (e.g., big metropolitan areas)
- AWS Wavelength Zones --- deploying resources over the 5G network (e.g., for a mobile application); improve latency as well
- In general, these other ways improve performance (speed)

RE: Regions:
- Contains AZs, which contain subnets (public and private). Subnet = IP networking environments
- Each AZ has redundant power sources and redundant networking b/w AZs
- Make your resources/app available widely in multiple AZs, then in case of disaster or failure, have back up/copies stored in a geographically distant area

RE: **Amazon CloudFront**
- Content delivery network
- Including to edge locations (isolated geographically...?)
- Content, videos, etc. are cached at different places around the world; users can access them globally with lower latency (get copies of resources closer to users for better performance)

Deploying Services Globally
- **AWS Management Console** allows you to easily deploy servers, DBs, variety of resources globally and easily (e.g., without having to rely on multiple services --- that's the huge upside to cloud computing). EX: deploying servers, DBs in multiple regions and setting them up to be redundant, copying data & synchronizing them so that resources are performant and you've got fail-safes in place

## 13. The AWS Shared Responsibility Model
- What you're responsible for vs. what AWS is responsible for
- Encryption, data integrity, authentication, network traffic, etc. are your responsibility --- AWS provides the services, but your job is to make sure things are secure
- AWS' user groups, security groups for EC2, network access control groups, etc. are a few examples of how AWS provides tools for this, but you have to use them
- "AWS is responsible for security OF the cloud, you are responsible for security IN the cloud."
- **See course slides for the 2 diagrams**

## 14. Application Programming Interfaces (APIs)
- Ports & protocols --- e.g., computer connects to Amazon.com (example given)
- Protocol --> HTTP or HTTPS, for example. The language used for communication over a network
- Port --> like an open door behind which a specific service is waiting for connections
- HTTP Methods: GET, POST, PUT (replace/update), DELETE
- APIs are sets of rules & protocols that allow computer programs to talk to each other; sharing info, data b/w programs; leverage standard protocols like HTTP
- In other words, a client computer access AWS via an API.  API defines the actions that you can request on AWS.
- e.g., when using Amazon's Management Console to spin up a new resource, server, DB instance, etc., it's calls to the AWS API (an API call on the backend to AWS)
- Every action you do is an API call to the AWS API
- Review of client-server relationship and APIs
- Flight aggregator (Momondo or Skyscanner) example; user searches on such a website, site makes API calls to airline websites **see diagram**
- Takeaway for AWS --- every action you take is an API call

## 15. AWS Pricing Fundamentals
- **AWS Compute** --- the amount of resources such as CPU and RAM and **duration** (per minute basis)
- When you shut down a service, you're no longer paying the CPU and RAM but you might still be paying for storage
- **AWS Storage** --- based on quantity of data **stored** or **allocated**
- e.g., S3 --- you have a gigabyte, you pay for a gigabyte; e.g., for EC2, you pay for the full volume of that storage (whether or not it's full)
- **Outbound Data Transfer** --- the quantity of data transferred **out** of cloud services. You don't pay to move it into cloud, but you pay to store & move it out of an AZ or Region. Businesses can get into tricky spots with this.

- **Pay-as-you-go:** adapt to changing business needs easily (e.g., you need 100 servers at a super busy time, but 10 after that time). Responsive to change, based on needs (not predictions), reduce risk overpositioning of missing capacity
- **Save when you reserve:** invest in reserved capacity (e.g., RDS and EC2). Committing for a period of time (1 or 3 years, e.g.). Save up to 75% compared to on-demand (pay-as-you-go). More you pay up front, the bigger the discount
-**Pay less by using more:** pay less using volume-based discounts. e.g., you're paying a certain $ amt per GB of data, then you hit a threshold of data storage, the price per GB reduces; then another threshold, another discount. A.K.A., tiered pricing

## 16. 6 Advantages of Cloud Computing
1. **Trade capital expense for variable expense.** e.g., purchasing servers (capital expense or CAPEX for a business) vs pay-as-you-go monthly using AWS services (an operational expense or OPEX for a business). Servers and hardware are expensive, so AWS can be a benefit here. CAPEX is tax deductible over depreciation lifetime (e.g., lifetime of purchased servers), while OPEX expense like AWS is tax deductible in the same year. Different advantages/trade offs.
2. **Benefits from massive economies of scale.**  They have a huge global network, infrastructure; the benefits like fast, managed services, not needing to purchase hardware, reliable networks, etc. from these economies of scale get passed onto customers/users of AWS. **Lower variable costs.**
3. **Stop guessing capacity.** Venn diagram --- overestimating what you think you need in computing resources vs. what you actually need. AWS helps avoid wasted $ or capacity.
4. **Increase speed and agility.** Deploy resources easily and quickly. It's all in the cloud, so businesses can adapt quickly, react to change, speed up product to market.
5. **Stop spending $ running and maintaining data centers.** Businesses don't have to spend money managing data, can invest in innovation instead.
6. **Go global in minutes.** Takes a couple of mins in the cloud to spread companies, infrastructure, and applications to different markets and customers.



# Section 4: AWS Authentication and Access Control

## 17. Intro

## 18. AWS Identity and Access Management (IAM)
- Service for authentication and authorization
- IAM Principals must be authenticated to send requests (with a few exceptions)
- A **principal** is a person or app that can make a request for an action or operation on an AWS resource
- Authentication ("you are who you say you are").
- Authorization ("you're allowed or not allowed to use these resources").
- AWS determines whether to authorize the request (allow/deny)
- **Identity-based policies** and **resource-based policies**
- Actions are authorized on AWS resources

Key Terms:
- **User:** an individual person/account
- **User Groups:** group of people with similar job roles, etc. that require the same level of access
- **Policies:** user gains permissions applied to the group through the policy. **ID-based policies** can be applied to users, groups and role.
- **Roles:** an identity which has permissions assigned to it via a policy, you can assume the role and take on those permissions. Think of it as switching hats (sys ops, dev, etc.), and you get access to different resources based on the role you have at a given time (the "hat" you're wearing).

**IAM Users**
- Account **Root User** --- full permissions. **Best practice** to avoid using this, and enable MFA.
- AWS IAM --- up to 5000 individual user acoounts can be created. **No permissions by default.** Must be added.
- e.g., Andrea (**see slides/diagram**). Amazon Resource Name (ARN) is part of a unique identifier
- Authentication can occur via Andrea's username/password in Amazon Management Console or access keys for API/CLI

**IAM User Groups**
- We can add many users to one group; a user can also be in more than one group
- If a user in in multiple groups, they get multiple sets of permissions
- Easy way to apply **permissions** to users using **policies**

**IAM Authentication Methods**
- User provides username, PW and (optional) MFA through AWS Management Console
- Through the CLI and API --- credentials --> access key ID and secrete access key to authenticate a user to the AWS API
- Access keys are for **programmatic access**

## 19. Creating IAM Users and Groups
- Demo / follow along --- **see video** 
- Review (done in earlier video). * is a wild card, allows all actions and resource access
- **RE: IAM Identity Center** --- AWS is encouraging people to use this, allows for single sign on. Stay tuned.
- Adding users to existing groups that have a policy > attaching policies to individual users
- **Login w/ IAM user account, NOT root user acct**

## 20. IAM Authentication and MFA
- e.g., user John. Username, password via the AWS Management Console.
- Access key ID + secret access key (the thing you download as a .csv). Can get authenticated via the CLI and API.
- Multi-Factor Authentication: something you know, something you have (e.g., phone), biometrics (e.g., face or thumbprint)
- Best practice to use for all accounts

## 21. Setup MFA
- He does example w/ authenticator app, I did passkey for me

## 22. IAM Roles and Policies
- An **IAM Role** is a IAM identity that has specific permissions; given to/assumed by users, apps and services
- sts:AssumeRole the short-term credential service (this is the API call)
- You can give user, an application, device the role and whatever permissions that come with it; the identity "becomes" the role and gains its permissions
- Common use case is a user that needs permissions/access to a resource or service for a limited time, then gives up that role when done
- e.g., permission to an S3 bucket that a user only has when in a certain role
- IAM policies define permissions; defined in JSON (gives example of AdministratorAccess, an ID-based policy)
- Bucket policies 
- ID-based policies can be applied to users, groups, roles
- Resource-based policies to resources... :P
- Need explicit "Allow" to grant permission, otherwise it's denied by default

## 23. Switching IAM Roles
- Demo --- creating a user Joe, creating a role (ec2-role), then granting permissions
- Copy ARN from the ec2-role Role in IAM (where you're admin, not Joe). Paste it into the JSON object on the "Resource" property (object is located in the Permissions policies card, click the + to expand & see the policy for that role as JSON)
- Back in IAM as admin (Chris), go to Users --> click Joe --> Add permissions drop down --> choose JSON, paste in object --> title it stsAssumeRole

{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": **new ARN from the ec2-role**
  }
}

- Can switch roles in the upper right corner drop down

## 24. IAM Identity Center
- Successor to AWS SSO (single sign-on). **See slide deck diagram.**
- Identity sources can be the IAM ID Center, an Active Directory (self-managed), or standard providers using SAML 2.0
- W/ an active directory, there's an AD Connector or AWS Managed Microsoft AD to facilitate this management of IDs and SSO
- There are built-in SSO integrations to business apps
- Also get SSO to different AWS accounts, including ones in diff AWS organizations
- IAM vs IAM Identity Center chart (**see slide deck diagram/chart**). Latter allows for more user-friendly, simplified SSO and access to AWS and non-AWS applications
Use cases:
- IAM --- managing AWS resources and permissions manually, fine-grained control; good for creating, managing IAM users and roles within AWS; you can federate (integrate) with external IdPs (identity providers) for SSO to AWS services, but more complex
- Identity Center --- SSO to AWS and non-AWS applications; centralized, so easier for a more complex environment; integrates with external directory services; streamlines acess to business apps and portals

## 25. IAM Identity Center in Action
- Demo --- created an organization in AWS, enabled ID Center, created two permission sets
- Created a group, then a user (Chris)
- Then, in IAM Identity Center (as Chris) --> Multi-account permissions (left side) --> AWS accounts
- Assign users and groups, permissions sets (did both); back under Multi-account permissions / AWS accounts, you can see the permissions sets listed (AdministratorAccess and ViewOnlyAccess)
- Went to email associated with that new user, clicked Accept Invitation, set up a new password and MFA
- Chris, R.A.; used built-in auth
- After signing in as Chris w/ newly created password, taken to AWS access portal page where you can choose the two roles by clicking on links; you can log into AWS with either permission set
- In ID Center (as admin), you can find & copy/use the AWS access portal URL for signing in (good to bookmark, supply to team, etc.)

## 26. AWS IAM Best Practicies
- Require human users to use federation with an identity provider to access AWS using temporary credentials (e.g., using Identity Center); current companies, etc. will keep using IAM with users, organizations, etc. But AWS is nudging new companies, people to do use Identity Center.
- Require workloads to use tmeporary cedentials with IAM roles to access AWS (e.g., don't store access keys in applications, configure them to use roles instead, so that they gain temporary access through a security token service)
- Require multi-factor authentication (MFA)
- Rotate access keys regularly for use cases that require long-term credentials
- Safeguard your root user credentials, don't use them for everyday tasks (companies tend to set this up very securly, strong password and multi-factor auth, then everybody gets and admin account --- in review, don't use the root user acccount day-to-day)
- Apply least-privilege permissions --- i.e., only what they need / what's required to do their job, not more
- Get started with AWS managed policies (good place to start since they're made fo ryou), and move toward least-privilege permissions
- Use IAM Access Analyzer to generate least-privilege policies based on access activity by users (tool built into IAM; which API calls are they making / interacting with, design policies accordingly / in a custom way)
- Regularly review & remove users, roles, permissions, policies and credentials
- Use conditions in IAM policies to further restrict access (e.g., access only from a range of company approved IP addresses)
- Verify public and cross-account access to resources with IAM Access Analyzer
- Use IAM Access Analyzer to validate your IAM policies for secure, functional permissions for your company
- Establish permissions guardrails across multiple accounts (e.g., use AWS Organizations and Control Tower for added security)
- Use permission boundaries to delegate permissions management within an account (e.g., gives a max amount of permissions that can be used by a given user, like a ceiling or max)

## 27. Exam Cram
- Review video
- Mentions a couple of things not covered in the videos (likely recycled from earlier versions of course?)

