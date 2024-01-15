# AMAZON WEB SERVICES

## Contents

1. [IAM Identity and Access Managment](#1-iam-identity-and-access-managment)
2. [EC2 Elastic Compute Cloud](#2-ec2-elastic-compute-cloud)
3. [EC2 Instance Storage](#3-ec2-instance-storage)

## 1 IAM Identity and Access Managment

Create users.
* Go to IAM.
* Select users.
* Select create user.
* Create user name.
* Select next.
* Tick i want to create an IAM user.
* Select custom password and enter password.
* Untick change password at next login.
* Select next.
* Can add user to group or create a group.
* Select create a group.
* Create group name.
* Add administrator access.
* Select create user group.
* Tick administrator access to make it active.
* Select next.
* Can optionally enter a tag.
* Select create user.
* Can email sign in instructions, download csv file or return to users list.
* Create an alias for the account in the IAM dashboard.

IAM Policies can be attached to groups or individual users. 
It is preferable to attech policies to groups
as the users in these groups will inherit the group policies.

Attach policy to a user directly.
* Go to user
* Select add permissions.
* Search for the permission to add.
* Add the required permission.

Some handy policies.
* IAMReadOnlyAccess.
* AdministratorAccess.

Create a policy.
* Go to policies in IAM.
* Select create policy.
* Use the visual or JSON builder.
* Add the required permissions to the policy.
* Name the policy.
* Select create policy.

Multi Factor Authentication MFA. Some options are.
* Google Authenticator (phone only).
* Authy (multi device).
* YubiKey.
* Hardware Key Fob.

Define a password policy.
* Go to Account Settings in IAM.
* Select edit.
* Select default or custom policy.

Add MFA to account.
* Click on account name in top right corner.
* Select security credentials.
* Select assign MFA device.
* Name the device.
* Select the authenticator device.
* Follow instructions to implement the MFA device.
* NOTE for training purposses do not set up MFA on the root account.
  If you loose the MFA device you will be locked out of the root account.

  AWS Access Keys. Are used to access your account via the command line.
  To install the AWS CLI google instructions. To check installed version open
  a command line and type aws --version.

Create an access key.
* Select users in IAM.
* Click on the user you want to create an access key for.
* Click on security credentials.
* Scroll down to create an access key.
* Select the command line interface option.
* Create the access keys and save them.
* Using the keys enter following in the aws cli.
```cli
aws configure
AWS Access Key ID [None]: <public-key>
AWS Secret Access Key [None]: <private-key>
Default region name [None]: eu-west-1
Default output format [None]
aws iam list-users
```

Cloud Shell can be opened via the Cloud Shell icon on the AWS toolbar.

IAM Roles for Services. Some AWS services will need to perform actions on your behalf.
To do this permissions will be assigned to AWS services with IAM Roles.
To create a role do the following.
* In IAM select Roles.
* Select create role.
* Select trusted entity.
* Select the use case.
* Select next.
* Attach a policy.
* Select next.
* Name the role.
* Select create role.

Create an IAM credentials report. Two types of reports are.
* IAM Credentials Report (Account level) lists all accounts users and their credentials.
* IAM Access Advisor (User-level) shows the service permissions granted to a user when those services were last accessed.

Generate a credential report.
* In IAM click on credential report on left hand toolbar.
* Download credential report.

View an IAM Access Advisor.
* In IAM go to users.
* Select users.

## 2 EC2 Elastic Compute Cloud

AWS Budget Setup.
To allow IAM users who are admins to view billing information.
* Log in as root.
* Click on account top right.
* Click on Account.
* Scroll down to IAM user and role access to Billing information.
* Select edit.
* Check Activate IAM Access.
* Click Update.

Checking bills.
* Click on account top right.
* Click on Bills left hand toolbar.
* You can check charges per service.
* The Free Tier is also available on the left hand toolbar.
* You can use Budgets in the left hand toolbar.

Amazon EC2 provides.
* Renting virtual machines (EC2).
* Storing data on virtual drives (EBS).
* Distributing load across machines (ELB).
* Scaling services using an auto-scaling group (ASG).

EC2 sizing and configuration options.
* Operating system Windows, Linux or Mac OS.
* How much compute (CPU).
* How much random access memory (RAM).
* Storage space.
* Network card.
* Firewall rules.
* Bootstrap script.

Create an EC2 Instance.
* In Console Home.
* Go to EC2 dashboard.
* Click on instances in left hand toolbar.
* Click on Launch Instances top right corner.
* Name the instance and optionally add tags.
* Choose image for your instance.
* Choose architecture.
* Choose instance type.
* Create an SSH key pair.
* Setup networking options.
* Configure storage.
* Enter user data (script which will run first time).

Example user data.
```cli
#!/bin/bash
# Use this for your data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

Stop or Terminate an Instance.
* Go to Instances.
* Select instance(s).
* Click Instance State on top toolbar.
* Select Stop to stop instance(s).
* Select Terminate if you want to terminate the instance.

AWS Security Groups.
* Security Groups only contain aalow rules.
* Security Groups can reference by IP or by security group.
* Has inbound and outbound rules.
* An instance can have multiple security groups.
* Security Groups are region specific.
* The Security Group is not part of the EC2 instance.
* By default all inbound traffic is blocked and all outbound traffic is allowed.

Create a Security Group.
* Go to Network and Security in left hand toolbar.
* Select Security Groups.
* Select a security group.
* Select Inbound or Outbound rules.
* You can edit the Inbound or Outbound rules.

AWS SSH Summary.
* SSH can be used to connect to Mac, Linux, Windows >= 10.
* Putty can be used to connect to all versions of Windows.
* EC2 Instance Connect can connect to all instances.

Connect to an EC2 instance using SSH.
```cli
chmod 0400 <ssh-key>
ssh <ssh-key> ec2-user@<ip-address>
```

Connect using Putty on Windows.
* Download putty.
* Install Putty.
* You can use PuttyGen to convert the ssh key file to ppk format.
* Load the ssh key into PuttyGen.
* Save public key with a .ppk extension.
* Open Putty.
* Select SSH.
* Enter ec2-user@\<ip-address>
* in the left hand toolbar select Connection -> SSH -> Auth.
* Enter the private key file.
* Save the session.

Connect using powershell on windows.
* Go to directory that has the ssh key.
* Ensure that you are the owner of the ssh key.
* The key must be in .pem format.
* ssh -i \<key-name> ec2-user@\<ip-address>

Connect to an instance using EC2 Instance Connect.
* Go to Instances.
* Select an instance.
* Click Connect on top toolber.
* Select EC2 Instance Connect.
* Enter the user name.
* Requires SSH to be allowed in security groups.

Add IAM Role to an EC2 instance.
* Go to instances.
* Select an instance.
* Select Actions on top toolbar.
* Select Security in drop down.
* Select modify IAM role.
* Select a role and save.

## 3 EC2 Instance Storage

* An EBS Elastic Block Store volume is a network drive you can attach to your instances while they run.
* It allows instances to persist data even after they are terminated.
* Can only be mounted to one instance at a time (CCP Level).
* Are bound to a specific security zone.
* Like a USB stick.
* Free tier is 30GB.
* Its  network drive not a physical drive.
* Can be detached from one instance and attached to another.
* To move a volume across availability zones AZs you must need to snapshot it.
* Have a provisioned capacity (Size in GBs and IOPS).
* You get billed for provisioned capacity and can increase the capacity over time.

Create EBS Instance Storage.
* When you create an EC2 instance a root volume will be created.
* To see the volumes attached to an instance go to instances then click on storage.
* Volumes can also be viewed by going to Volumes in the left hand toolbar.
* To create a volume go to Volumes in left hand toolbar and click Create Volume in
  the top right corner. Select volume type, size and availability zone.

NOTE You can choose the Delete on termination option when creating the instance.

Attach EBS Instance Storage.
* Go to Volumes in left hand toolbar.
* Select a volume to attach.
* Select Actions in top toolbar.
* Select Attach volume.
* Select the instance to attach to.

EBS Snapshots.
* Make a backup (snapshot) of your EBS volume at a point in time.
* Not necessary to detach volume to do snapshot but recommended.
* Can copy snapshots across AZ or region.
* EBS Snapshot Archive allows a snapshot to be moved to an archive tier that is 75% cheaper.
* Takes within 24 to 72 hours for restoring the archive.
* Recycling Bin for EBS Snapshots allows you to setup rules to retian deleted snapshots so
  you can recover them after an accidental deletion. Can specify retention 1 day to 1 year.

Creating an EBS Snapshot.
* Go to Volumes.
* Select a volume.
* Select Actions top toolbar.
* Select Create Snapshot.
* Add Description to snapshot.
* Select Create Snapshot.
* To view snapshots go to Snapshots in left hand toolbar.
* To copy a snapshot select a snapshot and right click on it. Select Copy Snapshot. Select the
  region you want to copy the snapshot to.
* To recreate a volume from a snapshot go to Snapshots in lefthand toolbar. Select a snapshot.
  Select Actions top toolbar. Select Create volume from snapshot. Select a volume size. Select
  an Availability Zone.

Set Up Recycle Bin.
* To set up the Recycle Bin go to Snapshots in lefthand toolbar.
* Select Recycle Bin in top toolbar.
* Select create retention rule.
* Name the retention rule.
* Select the resource type.
* Select the retention period. Select Create retention rule.

Restore from Recycle Bin.
* Go to Recycle Bin in Snapshots.
* Select Resources in lefthand toolbar.
* Select the resource you want to recover.
* Select Recover top toolbar.

Archive a snapshot.
* Go to Snapshots.
* Select a snapshot.
* Select Actions top toolbar.
* Select Archive snapshot.
* Confirm if you wish to archive this snapshot.

AMI Amazon Machine Image.
* AMI are a customization of an EC2 instance.
* You add your own software, configuration and operating system.
  provides faster boot and configuration time because all software is pre packeged.
* AMI are built for a specific region and can be copied across regions.
* You can launch an EC2 instances from.
  1 A public AMI: AWS provided.
  2 Your own AMI you make and maintain yourself.
  3 An AWS Marketplace AMI (An AMI someone else made).

Steps to build an AMI.
* Start an EC2 instance and customize it.
* Stop the instance (for data integrity).
* Build an AMI this will also create EBS snapshots.
* Launch instances from other AMIs.

Create an AMI.
* Create an EC2 instance.
* Add some customizations.
* Go to Instances.
* Select an instance.
* Right click and select image and templates.
* Select Create image.
* Name the image.
* Select Create image.
* Select AMIs in left hand toolbar.

Launch Instance from AMI.
* Go to AMI.
* Select an AMI.
* Select Launch instance from AMI top toolbar.
* Can also launch an AMI by going to Instances in left hand toolbar. Select Launch instances.
  When selecting an OS image open the My AMIs tab to select an AMI. Continue to launch like any
  other instance.

AMI Builder.
* Used to automate the creation of virtual machines or container images.
* Automates creation, maintain, validate and test EC2 AMIs.
* Can be run on a schedule.
* Free service (only pay for the underlying resources).

EC2 Instance Store.
* EBS volumes are network drives with good but limited performance.
* If you need high performance hardware disk use EC2 Instance Storage.
* Better I/O performance.
* EC2 Instance Store lose their storage if they are stopped (ephemeral).
* Good for buffer / cache / scratch data / temporary content.
* Risk of data loss if hardware fails.
* Backups and Replication are your responsibility.

EFS Elastic File System.
* managed NFS (Network File System) that can be mounted to 100s of EC2 instances.
* EFS works with Linux EC2 instances in multi AZ.
* Highly available, scalable, expensive (3X gp2), pay per use, no capacity planning.

EFS Infrequent Access (EFS-IA).
* Storage class that is cost optomized for files not accessed every day.
* Up to 92% lower cost compared to EFS Standard.
* EFS will automatically move your files to EFS-IA based on the last time they were accessed.
* Enable EFS-IA with a Lifecycle Policy.
* Example is move files that have not been accessed for 60 days to EFS-IA.
* Transparent to the applications accessing EFS.
* Customer has responsability for backup/snapshot procedure, setting up data encryption,
  responsible for data on drives.

  Amazon FSx.
  * Launch 3rd party high performance file systems on AWS.
  * Fully managed service.

  Amazon FSx for Windows File Server.
  * A fully managed highly reliable and scalable Windows native shared file system.
  * Built for Windows File Server.
  * Supports SMB protocol and Windows NTFS.
  * Integrated with Microsoft Active Directory.
  * Can be accessed from AWS or your on premise infrastructure.

  Amazon FSx for Lustre.
  * A fully managed high performance scalable file storage for High Performance Computing (HPC).
  * The name Lustre is derived from Linux and Cluster.
  * Machine learning and video processing.
  * Scales up to 100s of GBs.

  ## 7 ELB and ASG Elastic Load Balancing

  * Scalability is the ability to accomodate a larger load by making the hardware stronger (scale up)
    or by adding nodes (scale out).
  * Elasticity once a system is scalable, elasticity means that there will be some auto scaling so 
    that the system can scale based on the load. This cloud friendly pay per use match demand, 
    optomize costs.
  * Agility means new IT resources are only a click away which means that you reduce the time to make
    those resources available from weeks to just minutes.

Elastic Load Balancers.
* An ELB (Elastic Load Balancers) is a managed load balancer.
  * AWS guaruntees that it will be working.
  * AWS takes care of upgrades, maintenance, high availability.
  * AWS provides only a few configurations knobs.
* It costs less to setup your own load balancer but it will be a lot more effort.
* 4 kinds of load balancers offered by AWS.
  * ALB Application Load Balancer (HTTP/HTTPS) Layer 7.
  * NLB Network Load Balancer (ultra high performance for TCP/UDP) Layer 4.
  * GLB Gateway Load Balancer Layer 3. Route traffic to firewalls that you manage on EC2 instances. 
    Intrusion detection.
  * Classic Load Balancer (retired 2023) Layer 4 and 7.

Create Load Balancers.
* Create a number of EC2 instances to be balanced.
* Go to Load Balancers in the left hand toolbar.
* Select Create load Balancer.
* Selct the type of load balancer you want.
* Configure the load balancer.
* Choose the Network Mapping zones for the balancer.
* Assign or create a Security Group for the load balancer.
* Configure Listeners and Routing to target group ie a group of instances to load balance accross.
* Create the Load Balancer.
* Use the dfault DNS name to access the load balancer.

Auto Scaling Group.
* The goal of ASG Auto Scaling Group is to.
  * Scale out (add EC2 instances) to match a decreased load.
  * Scale in (remove EC2 instances) to match a decreased load.
  * Ensure there is a minimum or maximum number of machines running.
  * Automatically register new instances to a load balancer.
  * Replace an unhealthy instance.
* Cost savings. Only run optimal capacity.

Create an Auto Scaling Group.
* Select Auto Scaling Group in left hand toolbar.
* Select Create Launch Template. This is a template that is used to launch new EC2 instances.
  Is similar to setting up instances. Select the security groups. Select the ECB volumes if applicable.
  Add user data.
* Select Launch Template.
* Select the version of the template to launch.
* Select Next.
* Choose the availability zones.
* Select Next.
* Select a load balancer.
* Select a Health Check option.
* Select Next.
* Select Desired, Max and Min capacity.
* Select Next.
* Select Auto Scaling Group.
* You can go to Auto Scaling Groups or Load Balancers in the left hand toolbar to get further
  information on the health of the system. You can change the health check parameters in the
  load balancer.

Auto Scaling Groups Strategies.
* Manual Scaling.
* Dynamic Scaling.
  * Simple/Step Scaling.
    * When a CloudWatch alarm is triggered (example CPU > 70%) add 2 units.
  * Target Tracking Scaling.
    * Example average ASG CPU to stay at 40%.
  * Scheduled Scaling.
    * Anticipate a scaling based on usage patterns.
  * Predictive Scaling.
    * Uses machine learning to predict traffic.

NOTE Section Cleanup. Delete the ASG and the Load Balancer. Instances will be terminated when ASG is deleted.

## AWS S3

* Amazon S3 allows people to store objects (files) in buckets (directories).
* Buckets must have a globally unique name (accross all regions and all accounts).
* Buckets are defined at the region level.
* S3 looks like a global service but buckets are created in a region.
* Naming convention.
  * No uppercase, no underscores.
  * 3-63 characters long.
  * Not an IP.
  * Must atart with lowercase letter or number.
  * Must not start with the prefix xn--.
  * Must not end with the suffix -s3alias.

Create an S3 Bucket.
* Go to the Amazon S3 page.
* Name the bucket with a unique name.
* Assign an availability where the bucket will be based.
* Select Create Bucket.
* To upload to the bucket select the bucket and select upload. Select a file to upload.
* You can also create folders inside the bucket.
* You may not be able to access files using the public url without further configurations.

Create a Bucket Policy.
* In Amazon S3 Buckets go to Permissions on top toolbar.
* untick the Block public access box.
* Select Save Changes.
* Scroll down to Bucket policy.
* Select Edit.
* You can use the policy generator to create the policy.
* When inputting the ARN put a /* at the end to allow access to all objects inside the bucket.
* Select Add Statement when finished.
* Select Generate Policy. Copy the json into the Policy field.

Host a Static Webpage.
* Inside Amazon S3 Buckets select a bucket.
* Select Properties on top toolbar.
* Scroll down to Static website hosting.
* Select Edit.
* Enable hosting.
* Specify a html document.
* Save changes.
* Make sure to upload a html document.
* Go to Properties and scroll down to Static website hosting to get the url.

Versioning.
* In Amazon S3 Buckets go to Properties on top toolbar.
* Go to Bucket Versioning and select Edit.
* Select Enable and Save Changes.
* To see the versions go to Objects on the Buckets page and toggle the Show versions button
  below the top toolbar.
* To permanently delete an object select the object(s) to delete and then select Delete in the top toolbar.
  * You will be deleting a specific version of an object. Input permanent delete to confirm.
  * Select Delete Objects.
* To delete an object but not permanently turn off Show versions and select an object.
* Select Delete in top toolbar.
  * You will be adding a delete marker and will be prompted to input the word delete to confirm.
  * Select Delete Objects.
  * If you toggle on Show versions you will see that the object is still in the bucket.
* To delete the delete marker turn on versioning and delete the delete marker which will restore the previous version.

S3 Replication.
* Create two buckets in different availability zones.
* Ensure versioning is enabled on both buckets.
* In Amazon S3 Buckets go to Managment in the top toolbar.
* Scroll down to Replication rules.
* Configure the rule.
* Select Create new role for the IAM Role.
* Save the rule.

S3 Storage Classes.
* Create a bucket.
* Upload some data.
* Go to Properties.
* Select a Storage Class.
* Select Upload.

Create S3 Lifecycle Rules.
* In Buckets select Managment in top toolbar.
* Select Lifecycle rule.
* Choose rule scope.
* Add a transition.
* Selct Creat Rule.