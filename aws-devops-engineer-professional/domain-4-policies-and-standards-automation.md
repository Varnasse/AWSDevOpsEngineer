---
description: This page discusses key concepts related to Policies and Standards Automation.
---

# Domain 4: Policies and Standards Automation

### Certification Objectives

* Apply concepts required to enforce standards for logging, metrics, monitoring, testing, and security
* Determine how to optimise cost through automation
* Apply concepts required to implement governance strategies

### Securing our Infrastructure

#### AWS IAM big picture

A quick refresher on the four major pieces of IAM:

* Users
* Groups
* Roles
* Policy documents

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption><p>IAM big picture</p></figcaption></figure>

Users are a permanent set of credentials.&#x20;

Groups are an organisational tool for users.

Roles are temporary credentials.

These are all delivery vehicles for the policy documents

#### AWS IAM roles

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>IAM Roles</p></figcaption></figure>

USE ROLES EVERYWHERE YOU CAN

The steps to set up cross-account access - the role of an IAM user will have to have a trust relationship built in order for it to work. You should know how to define the trust policy, and which user or which principle is allowed to assume it. And then the individual user must be given a policy document that gives them the ability to assume the specified role.

#### AWS IAM policy example

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption><p>IAM policy example</p></figcaption></figure>

The condition sections show that we are adding in a modifier to make this policy more restrictive. Unless the holder makes a call from that specific IP range,  we will not accept that call.

#### Data protection: in transit

A theme across AWS is that you are responsible for the production of your data. It is up to you to ensure your data is protected both at rest and in transit.

When transferring in from outside of AWS, use Direct Connect or VPNs to transfer sensitive information.

Certificate Manager can be used to generate free certificates to encrypt your data.

#### Data protection: data-in-flight

Understand how encryption plays a role in our elastic load balancers. Both the application load balancer or the ALB and the network load balancers or the NLB support secure transmission of your data.

The ALB allows you to open an HTTPS listener and attach an SSL certificate to terminate the connection. In order to further secure your data, you will need to open up another secure connection from the ALB to the back-end EC2 instances.

Amazon CloudFront also serves as an encryption method for your traffic. You can attach your own SSL certificate to your CloudFront distribution which allows you to serve your content with an encrypted connection and your own domain name.&#x20;

#### Data protection: network perimeter controls

Around your network, be mindful of security groups, network access control lists and host firewalls:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Network Access Control Lists or NACLs act as a perimeter-level defence. They can be used to protect your entire subnet and everything that lives inside of it. NACLs are stateless meaning they require both inbound and outbound rules for traffic to flow correctly. These rules are processed in order, so ensure rule one doesn't conflict with rule two.

Security groups wrap around the EC2 instances' elastic network interface and are stateful meaning return traffic is automatically allowed, ignoring the rules for traffic flow in the outbound direction.

#### Data protection: at rest

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

S3 supports server-side encryption with customer-managed keys, S3-managed keys and KMS master keys.&#x20;

The default encryption can be enabled for S3.

S3 Glacier is the only service that enabled encryption automatically by default.&#x20;

#### Amazon GuardDuty

If you see the phrase "Network intrusion detection" in the exam, this should immediately point to Amazon GuardDuty.&#x20;

Amazon GuardDuty is an AWS-managed service, which can be easily enabled:

* Protects AWS accounts and workloads
* Monitors your AWS environment for  suspicious activity and generates findings
* Allows you to add your own threat lists and trusted IP lists
* Analyses multiple data sources:
  * AWS CloudTrail, Amazon VPC Flow Logs and DNS Logs.

GuardDuty is using machine learning to evaluate your traffic patterns to accurately predict future problems. It also watches AWS API calls so if an unusual API call occurs, it can be flagged.

#### Amazon Inspector

* Automated assessments that help improve the security and compliance of applications
* Offers an agent-based solution
* Detects vulnerabilities
* Verifies security best practices
* Generates findings reports

Amazon Inspector automatically assesses the security of your operating systems. It does this by using an installed agent which can report back to the inspector service based on the rule sets that you have selected.

#### Assessment templates and rules

It is up to you to select a set of rules for what you care about on your EC2 systems. There is a list of templates to choose from to quickly get set up.

For example, selecting the authentication best practices rule set will flag having root SSH access enabled.&#x20;

### AWS Systems Manager

This is Amazon's home-built configuration management tool.&#x20;

* Manage systems
  * In the cloud or on-premises
* Automate admin tasks
  * Collect software inventory
  * Apply OS patches
  * Create system images
  * Configure Windows and Linux operating systems

System Manager uses an agent that you can install on your server and enables you to have a single configuration management system to use.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>AWS Systems Manager features</p></figcaption></figure>

#### Run Command

This feature allows you to fire off a script or commands across an entire fleet of resources. A big benefit of this is that you are able to push a script to update or configure your servers without signing in to each one automatically.&#x20;

The run command has some pre-built commands but most of the time, you will write specific commands.

#### Patch Manager

Patch Manager can assist us with processing and updating our systems. We can deploy operating systems and software patches across a large group of servers.&#x20;

We can create a maintenance window for when these patches would be applied. For example, we do not want to patch during peak times.&#x20;

You choose the severity level of the patches you wish to apply and when we actually want to go through and do these updates, it will generate an audit report telling you exactly what has been changed.

#### Parameter Store

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Parameter Store</p></figcaption></figure>

When it comes to secure storage and distribution - things like credentials. If you are using the EC2 Systems Manager, using the parameter store feature can be a great way to assist with previously mentioned tasks.&#x20;

Parameter Store can contain configuration information, such as passwords, keys, license codes, etc.

We do not need to store this information directly on the individual servers, we can reference the content by writing scripts and utilise the System Manager API call to securely decrypt these values and pull them in whenever we need to have them.

#### Security credential storage options

There are many options for securing credential storage:

* AWS System Manager Parameter Store
* AWS Secrets Manager
  * Strictly for credentials
  * All values are encrypted via AWS KMS
  * Supports automated credential rotation
* AWS Licence Manager
  * Strictly for application licence storage
  * Supports licence use tracking with custom-enforced usage rules

### AWS Config

* Continuously track resource configuration changes
  * Sends notifications when changes occur
* Enables compliance monitoring and security analysis
* Evaluate the configuration against policies defined using AWS Config rules

We AWS Config, we are able to not only monitor a group of resources but also set up rules for how to handle changes to those resource groups.&#x20;

AWS Config can be used to discover new resources and add them to a resource group.&#x20;

#### AWS Config and AWS Config rules

We are tracking changes inside of AWS Config which allows us to see when a resource was changed, who made the changes and what time it took place.&#x20;

It also helps us with logging actions because we have a list of when all these calls were made. With the ability to send notifications based on the actions observed.

### Cost Optimisation

#### AWS Trusted Advisor

* Provides guidance to help you:
  * Reduce cost
  * Increase performance
  * Improve security

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>AWS Trusted Advisor</p></figcaption></figure>

You can set up alarms and notifications to proactively let you know when a situation where you could be spending too much money.

#### Using tagging for cost management

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p>Tagging example</p></figcaption></figure>

Setting intentional and strong tagging standards and enforcing these standards with resources like AWS Config, and AWS Lambda.

### Governance and Compliance

#### AWS Service Catalog

* Create and manage catalogues of approved IT services
* Limit access to underlying AWS services
* Helps achieve consistent governance and compliance requirements
* Enable turn-key self-service solutions for all end-users

It provides a great and highly configurable way to enforce best practices towards compliance and governance.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

#### EC2 instance compliance

There are many ways to build compliance.

Restricting EC2 instance launches to only approved customer Amazon machine images and then bootstrapping them with user data.

The use of configuration management platforms will help you enforce all of the standards that you are trying to achieve.

### Test Axioms

* Know AWS Config and AWS Inspector
* Understand access control to all services
  * Know your authentication methods
* Understand encryption of data in transit and at-rest
* Know AWS Systems Manager features in-depth

