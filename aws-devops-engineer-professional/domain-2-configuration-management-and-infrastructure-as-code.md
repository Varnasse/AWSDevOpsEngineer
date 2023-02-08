---
description: >-
  This page will discuss key concepts related to Configuration Management and
  Infrastructure as Code.
---

# Domain 2: Configuration Management and Infrastructure as Code

### Certification Objectives

* Determine deployment services based on deployment needs
* Determine application and infrastructure deployment models based on business needs
* Apply security concepts in the automation of resource provisioning
* Determine how to implement lifecycle hooks on a deployment
* Apply concepts required to manage systems using AWS configuration management tools and services

### AWS CloudFormation

AWS is one of our primary Infrastructures as Code platforms.&#x20;

AWS CloudFormation provides an AWS native solution to codify our infrastructure. This gives us a resource that is cost-effective and scalable to handle a lot of tasks regarding integration and deployment in our pipeline.

For the exam, it is important to know all of the parts that make up a CloudFormation template:

* Parameters
* Mappings
* Conditions
* Resources
* Outputs

AWS CloudFormation template anatomy:

```yaml
AWSTemplateFormatVersion: "version date"
Description:
  String
Metadata:
  template metadata
Parameters:
  set of parameters
Rules:
  set of rules
Mappings:
  set of mappings
Conditions:
  set of conditions
Transform:
  set of transforms
Resources:
  set of resources
Outputs:
  set of outputs
```

#### AWS CloudFormation: Updating Stacks

You need to know how to use change sets for updating stacks, what do they show? Where are they relevant?

**Updates with no interruption**

* The resource is updated without disrupting its operation and without changing its physical name&#x20;

For example, in something like a CloudWatch metric, if we wanted to change the alarm to not alert at 50% but now alert at 70%, we can make the change inside CloudFormation, apply the update, and the alarm is never destroyed.

**Updates with some interruption**

* The resource is updated with some interruption but without changing its physical name&#x20;

If I had an EC2 instance that I needed to resize, I could make the change within CloudFormation, the resource will turn off, resize will happen and the infrastructure will come back online.

**Replacement**

* The resource is recreated and a new physical ID is generated for the resource.#

If I were to change the database engine on my RDS database from something like MySQL to Postgres, I can not make that change in place, the existing will be destroyed and a new instance created.

#### AWS CloudFormation: Update Options

Know how to use the update policy:

* MaxBatchSize: Maximum number of instances that are updated
* MinInstancesInService: Minimum number of instances that must be in service within the Auto Scaling group while old instances are updated
* PauseTime: The amount of time given to instances to start applications.
* WaitOnResourceSignals: The time to wait for the Auto Scaling group to get the required number of valid signals
* SuspendProcesses: This prevents Auto Scaling from interfering with a stack update
* MinSuccessfulInstancesPercent: The percentage of instances in an Auto Scaling rolling update that must signal success for an update to succeed.

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>AWS CloudFormation Update Options</p></figcaption></figure>

#### Layering and Nesting templates

At times, architectures and environments extend past a single CloudFormation template, so it is important to know the methods that exist for managing multi-template environments. This is done by using the chaining and nesting of CloudFormation templates.&#x20;

#### DependsOn

With the exception of a few inherent dependencies, the CloudFormation will manage:

* All stack creation and update actions are attempted in parallel unless other dependencies are specifically outlined.

DependsOn is the simplest and most common of these dependency types.&#x20;

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>DependsOn example</p></figcaption></figure>

In this example, the application instance depends on the launching of the RDS instance. If this was a database engine running inside an EC2 instance, DependsOn would not be the best option to use. This is where WaitCondition comes into play:

#### WaitCondition

WaitCondition and its accompanying WaitConditionHandle focuses on being between two resources with a dependency chain, and are themselves resources in the CloudFormation template.&#x20;

The WaitCondition waits for the WaitConditionHandle to be signalled before it is ready to move on.

#### AWS CloudFormation Template resource attributes:

* **CreationPolicy Attribute**: Define a period of time during which AWS CloudFormation will wait for a signal before marking the specific resource as **Create Completed**. Useful when you want the resource to finish its configuring before proceeding to deploy the next resource e.g. software installation on an EC2 instance.
* **DeletionPolicy Attribute**: Preserve a backup of a resource when its stack is deleted, you can specify the options **retain** or **snapshot**. By default, there is no deletion policy enabled.
* **DependsOn Attribute**: Create an explicit dependency that requires a specified resource to be created before another can begin.
* **Metadata Attribute**: Associate structured data with a resource.
* **UpdatePolicy Attribute**: Define how CloudFormation updates the AWS::AutoScaling::AutoScalingGroup resource.

#### AWS Cloudformation helper scripts:

* **cfn-init**: Executes cfn metadata one time, typically in user data
* **cfn-hup**: Monitors cfn metadata and applies changes when discovered
* **cfn-signal**: Provides completion signal of a CreationPolicy or WaitCondition
* **cfn-get-metadata**: View the metadata that is stored in a CloudFormation stack.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption><p>cfn-hup example</p></figcaption></figure>

Cfn-hup is a way to set CloudFormation on reevaluating the CloudFormation template metadata. Setting the interval at which CloudFormation will reevaluate the metadata is important because updates to the metadata do not automatically trigger a stack update. Using cfn-hup will provide that update ability.

### AWS Elastic Beanstalk

AWS Elastic Beanstalk is a management tool for quickly getting applications deployed in their own AWS infrastructure.

* Automated infrastructure managemnet and code deployment for your application
  * Designed to let developers deploy code
* Includes:
  * Load balancing
  * Health monitoring
  * Auto scaling
  * Application platform management
  * Code deployment

#### AWS Elastic Beanstalk: Components

Important to know the different components of an Elastic Beanstalk build and application. What customisations can you manage and what controls do you have with each one of these areas?

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>AWS Elastic Beanstalk: Components</p></figcaption></figure>

#### AWS Elastic Beanstalk: Environments

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption><p>AWS Elastic Beanstalk: environment example</p></figcaption></figure>

In this example, there are three environments: test, staging and production. Each is configured to use a custom AMI on its underlying EC2 instance.

Having multiple environments per application allows you to run your production system on known good versions of your application, while you perform tests on the new versions in other environments.&#x20;

#### AWS Elastic Beanstalk: Versions

AWS Elastic Beanstalk has many managed update and deployment options

### AWS OpsWorks

AWS OpsWorks is a configuration management service that allows you to configure and operate applications in a cloud environment by using Puppet or Chef.

* Application infrastructure management
* Flexibility to change the configuration of the environment over time
* Three offerings:
  * AWS OpsWorks for Chef Automate
  * AWS OpsWorks for Puppet Enterprise
  * AWS OpsWorks Stacks

<figure><img src="../.gitbook/assets/image (18) (1).png" alt=""><figcaption><p>AWS OpsWorks Cheatsheet</p></figcaption></figure>

It is important to remember that OpsWorks includes automation to scale your application and dynamic configuration to orchestrate changes as your environment scales.

