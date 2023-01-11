---
description: This page will discuss key concepts related to Incident and Event Response.
---

# Domain 5: Incident and Event Response

### Certification Objectives

* Troubleshoot issues and determine how to restore operations
* Determine how to automate event management and alerting
* Apply concepts required to implement automated healing
* Apply concepts required to set up event-driven automated actions

### Troubleshooting Tips

#### Build a logging strategy

One of the best things you can do is establish a logging strategy. It is important to consider where the logs are being stored for both general storage and for access patterns.&#x20;

Use the CloudWatch logs agent for EC2/ECs to push logs to CloudWatch Logs

Centralize logging in a separate account, and use Kinesis firehose to move multiple streams of logging to S3 for example.

Log as much as you can even if you don’t immediately use it

Keep logs as long as you can and use them for long-term analysis

#### Collect and aggregate data

In this example, we are showing how CloudWatch works well with AWS X-Ray:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

#### Discern patterns by analysing data

* Analyse data to discern utilisation patterns
* Check the minimum and maximum, not just averages
  * Shows extremes and whether they are growing
* Analyse Elastic Load Balancing access logs and insert custom aggregate metrics into Amazon CloudWatch through Amazon EMR

#### Root access key use case

In this example, we are showing a method for how you could be notified should access keys from the root user be used to make API calls:

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

In this workflow, we would be using CloudTrail to work with CloudWatch in order to consume the logs and then make a custom metric in order to set up a CloudWatch alarm. Further down the line, we can have an Amazon SNS topic set up so that if an alarm is triggered, it sends a message of the topic and we can be notified.

### Automated healing

We are looking at how we can implement automation to help us recover from failure events.&#x20;

#### Use automation to quickly recover

AWS CloudFormation

* You can easily set up an automated action triggered by an alarm to relaunch your infrastructure based on a template.
* The mechanism to launch this would be built in and automated by you, but it provides a method for you to quickly recover full and highly customised architectures
* Will be using cfn-init or cloud-init with CloudFormation&#x20;

AWS Elastic Beanstalk

* A very similar mechanism to CloudFormation but its level of complexity is greatly reduced.
* You still have the ability to recover an application stack but keep in mind the differences between what you could do with the Elastic Beanstalk&#x20;

AWS OpsWorks

* This allows you to do some level of infrastructure management but it uses Chef or Puppet in order to provide you with mechanisms for building and configuring your resources.
* Creating recipes, cookbooks and modules that enable you to quickly recover your application

### Auto-scaling and lifecycle hooks&#x20;

#### Automation with Auto Scaling

Auto-scaling already automates the adding and removing of instances to your Auto-Scaling group based on the scaling policies that you set up.

#### Auto-scaling instance lifecycle

It is important to know the different actions that can be taken during the Auto Scaling process, how they operate, and what can be invoked and suspended.

#### Suspending Auto-scaling services

Knowing how to suspend the launch or terminate actions with Auto-scaling and understanding the reasons for doing so:

* Scaling processes
* Detach an instance
  * Move an instance from one Auto Scaling group to another
  * Testing purposes for running your application
* Standby state
  * Remove the instance from service, troubleshoot or make changes to it, and put it back into service
  * Doesn't actively handle application traffic until you put them back into service
* Launch
  * Disrupts other processes
* Terminate
  * Disrupts other processes
* AlarmNotifications
  * Accepts notifications from Amazon CloudWatch alarms that are associated with the group
* ReplaceUnhealthy
  * Terminates instances that are marked as unhealthy and later creates new instances to replace them
* HealthCheck
  * Checks the health of the instance
* AZRebalannce
  * Balances the number of EC2 instances in the group across the Availability Zones in the region
* AddToLoadBalancer
  * Adds instances to the attached load balancer or target group when they are launched

#### Auto-Scaling termination policies

* Default
  * Ensures instances span Availability Zones evenly for high availability
* Oldest instance
  * Upgrading instances in the Auto Scaling group to a new EC2 instance type&#x20;
* Newest instance
  * Testing a new launch configuration but do not want to keep it in production
  * Can be customised
* AllocationStrategy
  * Gradually replace On-Demand instances of a lower priority type with On-Demand instances of a higher priority type
* OldestlaunchConfig
  * Updating a group and phasing out the instances from a previous configuration
* OldestLaunchTemplate
  * Updating a group and phasing out the instances from a previous configuration
* ClosestToNextInstanceHour
  * Maximise the use of your instances and manage your Amazon EC2 usage costs
  * Can be customised

#### EC2 Auto Scaling group lifecycle hooks

These hooks enable you to put certain actions on hold and trigger other actions to occur at specified stages.&#x20;

Instances can be put in a wait state in a lifecycle hook operation. The maximum amount of time to put an instance in a wait state is 48 hours (default is 1 hour).

When a scale-out event occurs, meaning that an instance or instances are being launched or when a scale-in event, meaning instances are being terminated.&#x20;

The lifecycle hooks allow these actions to be put into a pending state.&#x20;

#### Auto Scaling considerations

* You might need to combine multiple types of auto-scaling
* Your architecture might require more hands scaling using Step Scaling
* Some architectures need to scale on two or more metrics&#x20;
* Try to scale out early and fast
* Use lifecycle hooks

### Test Axioms

* Understand auto-scaling and lifecycle hooks
* Understand logging architectures
* Know how to use various methods for automation to recover
* Have experience with suspending load balancing and auto-scaling processes
