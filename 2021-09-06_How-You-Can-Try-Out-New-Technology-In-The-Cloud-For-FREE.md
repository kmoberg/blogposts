# Why you should be using the AWS cloud for your LAB - for free!
(img)

## Background
In IT, we do a lot of “lab” work. Trying new things, testing out a theory or an application that you have developed, or simply learning a new technology, such as Linux or even Windows server 😱.

This labbing can, of course, be done in a platitude of ways. On your computer, on an old server or even on a Raspberry Pi! All of these methods work very well, however, they all have some downsides: your computer might run slowly because you’re running a virtual machine on top of your ordinary OS. Old servers consume a lot of power, and noise, and a Raspberry Pi might not support the software you want to run, or isn’t powerful enough. On top of all of that, a home server or Raspberry Pi might not be available to you from outside of home without configuring VPNs.
 There is however, a simple way to allow you to run your lab, from anywhere, for **free\*** - in the cloud, using Amazon Web Services. AWS is the worlds largest public cloud provider, and according to [Forbes (2019)] it accounts for nearly _half_ of the worlds public cloud revenue with almost every Forbes top 100 companies using AWS for some of their digital needs. 

Ok, I hear you say, how does this benefit you and your lab? Well - Amazon offers one of the most generous free-tier offers for its new users out of all the large public cloud providers - a full **year** of free services! And if you know the limitations of the free tier, you can run clusters of virtual machines at a time if you want to try out auto-scaling instances, a platitude of databases, or want to crunch a dataset or two a month. Or, maybe you just want to host a website? You can do that too, for free, for a full year!

* AWS offers a specific “free tier” of products for 12 months for new accounts with set limitations to usage per month. This does not include all products, but is limited to specific ones. We **strongly** recommend you configure a billing alert to warn you if any cost accumulate on your account. Refer to this post #FIXME to see how you can configure this sort of alert.

## The AWS Free Tier
Amazon Web Services (AWS) has one of the most generous free tiers of the major public cloud providers. It offers a mix of over 100 **always free** and **12 months free** products that you can use to your hearts content to lab out your new applications and it is important that you understand how the free tier works, if you want to avoid being charged for your AWS usage. 

### Product Naming
If you have never used AWS before, there are some common names you need to be aware of before we get started. And yes - AWS naming of products can be... Hard to understand.

**Amazon EC2** is the AWS Virtual Machine service. Ever created a virtual machine in VMware, Hyper-V or Azure? This is the same thing. An “EC2 Instance” refers to a virtual machine. Oh, and since it’s called EC**2**, you might think there was an EC1 at some point? Nope. EC2 stands for Elastic Compute Cloud = EC2.

**Amazon S3** is for **object** storage. What does object storage mean?  In S3, you can store images (I use it to store and serve all my screenshots out of), documents, config files, text files, you name it. If it is an object, you can store it here. However, keep in mind, it is NOT a file system! So what happened to S1, and S2 you might say? Again, never existed. 🤦‍♂️ S3 = Simple Storage Service.

**AWS Lambda** a serverless compute service. Ever wanted to write a function or an API without worrying how to run it? Write a lambda function! The code can be triggered multiple ways, and will run, without you ever configuring a server!

**Amazon RDS**, AWS managed Relational database service! Not only does it save you from having to become a Windows or Linux server guru just to get a simple database running, it might also even be cheaper with out the overhead of having to have an operating system as well. It also out of the box supports scaling, high availability, and can be much more secure than you having to become a leet hacker yourself. 

**DynamoDB** SQL not your thing? DynamoDB is for you then! A NoSQL database with MongoDB support! If you ask MongoDB, they’ll say that their cloud offering is much better than DynamoDB, and you can’t compare the two, but from experience... DynamoDB is pretty good for 9 out of 10 use cases!

**VPC**, Virtual Private Cloud. A VPC allows you to segment your resources into multiple networks, completely segmented from each other. Think of this, kinda like a VLAN, except it’s not a VLAN. Confusing, I know, but you don’t need to worry too much about it for now. 

And that’s only barely touching the surface of AWS products. As of writing, in September 2021, there are over 200 AWS services, with more being added almost monthly. In 2020, a fantastic [YouTube Video] was made of every AWS product. The week after it was made, another 10 services were launched.

### Free Tier Offers
For the most up to date, and complete list of current free tier offers, make sure to check out Amazons own page on the different products. Keep in mind that although this article does list a few, very long running and most common free tier offers, they may change at Amazons discretion, so make sure to double check [https://aws.amazon.com/free/] for the up to date info.

**Amazon EC2**: t2/t3.micro - 750 hours/month for 12 months. 
AWS offers you 750 hours per month, for 12 months. You can chose to spend these 750 hours however you like, if you want to run 10 instances for 75 hours, or 75 instances for 10 hours, you can do so, or you can run 1 instance for an entire month, 24/7. This offer is limited to the t3.micro instance, which is equivalent to 2 vCPUs and 1GB RAM. Before you think that this is not enough: A single t3.micro instance, can easily serve a hundred thousand monthly web users, if configured well and using managed RDS.

**Amazon RDS**: t3.micro - 750 hours/month for 12 months.
Same deal as with EC2, you can run a single DB server 24/7 for a month or 75 instances for 10 hours. Combined with an EC2 web server, this combination is very powerful.

**AWS Lambda**: 1 million free requests/month, always free!
Pretty self explanatory! 1 million requests a month for free! Always!

**Amazon S3** 5 GB storage for 12 months.
You get 5 GB of object storage for free, however, one metric to keep in mind here is that you do get a limited number of GET and PUT requests: **20,000 GET **requests, **2,000 PUT** requests.

Again, there are over 100 products available on the free tier, so make sure to check out Amazons page to see details on each subject.

## Before We Get Started
With an understanding of how the free tier works, it’s now time to get started with some labbing! 

There are two ways to follow along with the labs in this article:

1. Follow along in the AWS Console, and click the appropriate buttons to complete the configuration.

2. Use the AWS CLI for your operating system. Everything you can do in the AWS Console, you can do with the AWS CLI from your computer. Learning the AWS CLI can be very useful and allows for scripting of commands and automatically building and tearing down configurations once you are done.

 > There is nothing wrong with either alternatives, and they are both perfectly legitimate ways of interacting with AWS. In fact, most people work with a combination of both tools depending on what task they are doing. However: We do recommend that you configure your AWS CLI and try it out, even if you want to primarily use the web console, as there are some things, like CloudFormation that require that you use the CLI.

### Terraform
Finally, there is one more way to do all configuration, which I will touch on in a separate article, as it is another huge project in and of it self - **Terraform**. Terraform is an **Infrastructure as Code (IAC)** tool, a declarative programming language that allows you to tell the code what you want it to look like, then it will do the work for you. You can tell it to start a specific instance, with X storage and Y RAM etc. and it will do it for you. It will also allow you to destroy everything, with a single command line command. **THIS** is what makes labbing in the cloud extremely easy. Watch out for part two of this article series for that!

## Let’s get labbing!

### 1. Creating an AWS Account
* Go to [https://aws.amazon.com](https://aws.amazon.com) and click the **Create an AWS Account** in the *top right* corner of the page.
* Input your email address and select a root password.
  * We **highly recommend** that you use a password manager such as [1Password](https://1password.com) to generate a 20+ character password. This password will only be used for your *root* account, and will not be one that you use regularly, however it will have full access to all AWS services in your account and it would be very bad if the account was compromised!

* Select an AWS account name. This can be anything you wish, and it will be the account name that you will use when signing in.

### 2. Contact Information
* Select *personal account* unless you have an organization you want to connect your account to.
* Input your phone number, country and address information.
* Read the user agreement and check that you have read and agree to the terms.

### 3. Payment information.
This step is **mandatory** to complete as well, and you **must** input a **valid** creditcard in order to create an account. You will be charged $1 USD to the card, that will be refunded to you in a couple of days after creation.

 > **NOTE:** As long as you follow along with this lab, your card will not be charged for any usage, unless you on your own, start up services or leave them running so that you exceed the Free Tier limits.

> Later in this lab, *we will create an e-mail alert that will warn you if you approach or exceed the free-tier limits*.

* Fill out a valid creditcard and complete the form.

### 4. Identity Confirmation
* Finally, confirm your itentity via SMS or a robot call.

### 5. Support Plan
* Select the **Basic Plan** (Free). We do not need support from AWS at this time.

### 6. Complete!
We have now successfully created an AWS account and we are ready to continue!

You will be presented with a screen filled with tutorials that can help you get started with AWS. Note that many of these tutorials are *not* included with the free tier, so make sure you understand what is covered before trying the tutorials not specifically marked with Free Tier.

Click the Sign In To the Console to continue.

## Congratulations! 
Congratulations! You now have an Amazon AWS account, with free tier active for the next 12 months!

## Securing Your Account
The account you just created and can now sign in with is known as the **root account**. This account has complete access to your entire AWS account, and in general, you should avoid using this account in your regular day-to-day use.

The [official AWS documentation] has this to say on the subject:

>**Important!**
We strongly recommend that you not use the root user for your everyday tasks, even the administrative ones. Instead, adhere to the [best practice of using the root user only to create your first IAM user.] Then securely lock away the root user credentials and use them to perform only a few account and service management tasks. To view the jobs that require you to sign in as the root user, see AWS Tasks That Require Root User. For a tutorial on how to set up an administrator for daily use, see Creating Your First IAM Admin User and Group.

In this lab, we will first secure the root account, then create an IAM administrator user that you can use for your daily administration. Having one IAM account per user of the AWS Account will provide accountability and traceability, allowing you to quickly identify who has provisioned resources, created or deleted security groups, etc.

## Securing the Root Account
Start by signing in to your AWS account using the root credentials you created in the previous lab.

**NOTE:** If you are presented with a sign-in screen asking you to select the type of account to log in to, select **Root user**.

(img)


### Signing In

* Sign in with the e-mail address of the **root account** and click next.

* Enter your password on the next screen.


### Enabling MFA for the Root Account
When you're signed in, you are greeted by the AWS Management Console. The AWS Console is where you will be managing everything related to your AWS account.

We want to manage our users' identity, which you can find in the *Identity and Access Management* panel, also known as **IAM**.

* In the Top Menu Bar, click **SERVICES**
* In the menu, locate **IAM** under **Security, Identity & Compliance**, or use the search feature to search for **IAM**.

From the IAM panel, we can create users, groups, policies, and more, which we will go into greater detail about in a later workshop. For now, focus on the big panel front and center, marked **Security Status**. This panel is continuously updated and will inform you about some primary security status items for your account that AWS deems essential. As you can see, we need to configure multiple things here to be compliant.

(img)

* The first thing we must do, is activated MFA (Multi-Factor Authentication) on our root account. Click the row in the Security Status to go straight to the appropriate panel.
* Click Multi-Factor authenticator
* Select the type of MFA device you wish to use. In most cases, a Virtual MFA device using [1Password], [Google Authenticator], [Microsoft Authenticator] or any other MFA device will suffice.
    * If you have a U2F Security key, you can also use it as an MFA device!

* Click on **SHOW QR** code to display the QR code and scan it with your MFA device.
* Once done and added to your MFA app, you need to enter **TWO CONSECUTIVE CODES** from the MFA device into the AWS Console! This means that you will have to wait for the second code to be generated and take up to 60 seconds.
* Click completely to finish the process. MFA is now active on the account, and your root account is much more secure!


### Creating an IAM Administrator Account
* Now that the root account is secure, we want to create an IAM user for you to use as your daily administrator account.
* Click to go back to the IAM Dashboard.
* Select the **Create individual IAM users** dropdown from the Security Status and select Manage Users.
* Click **Add User**
* Give your account a name, and select **** *programmatic access* and *AWS Management Console Access .* This will give your IAM user access to both the AWS CLI and the AWS console itself.
* Either autogenerate a password or select a custom password. If you choose a custom password, make sure it is a **strong** password of 15+ characters with symbols and numbers. Use a password manager such as [1Password] save the password for you.
* If you are creating the account for yourself, remove the checkmark next to **Require password reset**.

#### IAM Permissions
We will go into IAM permissions in a later workshop, so now, follow the instructions, and we will cover each step in detail later.

In this step, we will create a group for administrators with **full access** to the AWS Account.

* Click **Create Group** to create an Administrators Group.
* Call the Group: **"AWS-Global-Admin"**.
* Add the policy **AdministratorAccess** by checking the box next to it.

> **IMPORTANT!** Please note: Administrators with the *AdministratorAccess* do not neccesarily have access to billing features of AWS. Do gain access to this, you must give IAM users access to the console. Follow [this] AWS documentation to activate the feature.

* Click **Create Group**.


* Click **"Next: Tags"** at the bottom of the screen.
* At this point there is no need to add tags to this user, so you may skip this step if you wish, but you can also enter in key-value pair format, things such as:
    * Name: Your Name
    * Role: Your Role
    * Title: Your Title
    * Etc.


> **NOTE**: Tags are cAsE sEnSiTive, meaning that **Name** and **name** are considered **different** tags!

(img)

* Click **"Next: Review"**.
* Verify that the information is correct.
* Click **"Create User"**

** **VERY IMPORTANT:** This screen contains an *Access key ID* and a *Secret access key* which can only be seen on this screen, once!** You **must** remember to store both of these keys, as they will be important when configuring the AWS CLI! They **cannot** be recovered, only regenerated!

* Your user has now been created, and you can sign in using the URL shown in the Success box. Don't worry if it looks like a complicated URL; we will fix this soon!
* Click "Close" to end the wizard and return to the IAM Dashboard.

### Creating an IAM password policy
Finally, the last step in the security status check is to apply an IAM password policy. This policy will dictate password length and complexity requirements, and things such as password expiration dates, and it is up to you how you wish to configure this policy.

A couple of recommendations:@

* We suggest requiring 15+ character passwords to encourage the use of password managers.
* We suggest **requiring** the use of MFA devices for **all users** as this *significantly* increases account security.
* IF you have forced MFA, we (and NIST) recommend setting the password expiration to infinite, or 365 days as research has shown that forcing password changes often, significantly reduces the complexity of the passwords that people use.

* Click on "Apply an IAM password policy" to get started.
* Select "Set password policy."
* Pick the options and set the requirements for your organization.
* Save the policy to apply it.

Your accounts are now compliant!


### Enabling MFA on your new IAM account
To enable MFA on your newly created IAM account:

* Click on "Users."
* Select your new user.
* Select the tab "Security Credentials."
* Click on "Assigned MFA device" - "Manage."
* Follow the same steps as for the root user.

> **Note:** You can use the same MFA app for multiple users! Just create a new entry in your MFA app. Make sure to label them in the app, so you know which code is for which account!

## Launching your first virtual machine
Before You Begin
In this lab, we will launch instances in the AWS cloud, all within the free-tier, and as long as your account is free-tier eligible and you follow the instructions in the, you will not be billed for the resources you are creating.

Please do remember to terminate or delete any instances or volumes you create.

We will utilize Linux instances in this lab. If you have not used Linux in the past - no fear, we will show you the few instructions you need to know to get started.

> **IMPORTANT**: AWS Uses **REGIONS** when launching EC2 instances. Each instance you launch will only be available in the region you select. You **MUST** select your appropriate region **before** you start the instance! You can switch region in the **top right** of the AWS console by clicking the region name and selecting the region you want. For these labs, it doesn't matter which region you pick, but we recommend that you choose the region closest to you. In our case, we will select the *Stockholm* region.

### Requirements
You will need some form of terminal emulation program on your computer to connect to the Linux instances.

* If you are on macOS or Linux, you can use the built-in Terminal.
* If you are on Windows 10, you will need to download a terminal application.
    * We recommend [GitBash] ,which, when installed, will allow you to follow along with the instructions in the same way Mac and Linux users use it.


## Getting Started
### 1. Launching Your First Instance
Launching EC2 instances is not much work, but the first time you launch an instance, you will have to go through a few extra steps to configure access properly.

* Start by navigating to the EC2 management console by clicking **Services** in the top menu, then selecting **EC2** under **Compute**.
* Click on **Launch Instance**
* Select the *Amazon Linux 2 AMI (HVM), SSD Volume Type*, make sure it is marked with "Free tier eligible."
* Select the **t3.micro** instance type - again, make sure it is marked with "Free tier eligible."
* Click on **Next: Configure Instance Details** - NOT Review and Launch, as we need to configure some options in the next few pages.


* Remove the checkmark next to T2/T3 unlimited.
* In the field marked "User data" enter the following *bootstrap script*

#!/bin/bash
yum update -y
yum install httpd
systemctl start httpd
systemctl enable httpd
echo "Hello internet, This is the cloud" > /var/www/html/index.html

This script will update Linux on the instance and install the Apache web server, start it and enable it through reboots. It will finish by adding an index.html file that says, "Hello internet! This is the cloud!".

* Click on add storage
* Leave the defaults and move on to "Add Tags."
* Add any tags you wish, one recommendation is to add the tag `Name,` which will name your instance. Note the tags are case-sensitive!
* Move on to add security group

#### Security Group
This is where you restrict access to the server instance. The default is to allow SSH from anywhere (indicated by the source 0.0.0.0/0). As we are going to be terminating this instance pretty quickly, you can leave SSH exposed to the internet like it is for now, but for production, this is **not recommended!** and is a terrible practice to do so.

In the bootstrap script, we installed the Apache web server, and we want to allow internet access to this server.

* Click on **Add rule**
* Select type **HTTP** and leave the defaults. It should default the source to `0.0.0.0/0` and, in some cases, add `::/0`. This is fine.

* Finally, click on *Review and Launch*
* Launch the instance with the **Launch** button in the bottom right.

#### Key Pair
To connect via SSH, we are going to use SSH-key-pairs. There is **no** password access to AWS EC2 instances, but AWS instead uses key pairs. It stores one public key on the server, while you keep the private key.

If you have an existing SSH-key-pair that you wish to use, you can upload the public key to AWS and let AWS use it as the security token, or you can create a new key pair from this screen.

* To do so, select **create a new key pair** from the drop-down menu and give it a name. The name doesn't matter.
* Click **Download Key Pair**.
* Click Launch Instance

The instance is now starting up. To check on the progress, click on **View Instances**.

## 2. Connecting to the EC2 Instance
(img)

* Once the instance is marked with the status *running*, you can connect to the instance. You will find the DNS name and IP address by clicking on the instance and looking in the Description tab at the bottom. Look for the **IPv4 Public IP** and click it to copy it.

* Open your Terminal application and navigate to where you stored the SSH Key file you downloaded in the last step, in my case:

cd ~/Downloads

From that directory, you connect to the instance by passing in **your** `SSH Key Name` and the `IPv4 address` into the command. If you get a message about trusting the IP, type `YES`:

ssh -i "your-keyname.pem" ec2-user@YOUR-IP-FROM-AWS

For example:

ssh -i "aws-demo.pem" ec2-user@13.53.194.39

(img)
* You will be prompted with an ERROR message! This is because the permissions for the file are wrong, and you need to protect it. Do this by issuing the command:

chmod 400 "your-keyname.pem"

* Try connecting again to the instance, using the same command as before.
* Congratulations! You are now connected to the cloud instance!

(img)

## Check that Apache is working
You can also check that Apache is working by opening the IP address in a browser of your choice. If everything has worked, it should return the message we created earlier.
(img)

## Terminating Resources
To ensure you are not billed for any costs, we recommend terminating the instance after you are done testing it, and deleting any resources connected to it, in this case, we have created an EBS instance by default.

* To terminate the instance, go to the EC2 console, click the instance in question
* Click **Actions**
* Select **Instance State** then **Terminate**
* Confirm that you wish to terminate the instance.

(img)

In most cases, the EBS volume will be deleted automatically, but we want to ensure that it is deleted.
* To delete the EBS volume, in the **left menu bar** click on **Volumes** under **Elastic Block Store**.
* If there is a volume here, select it, click **Actions** then **Delete Volumes**

Once your instances and volumes are deleted, no further cost incurring resources should be running. You can leave the keypair and security groups as is if you want, or you can remove these too.
