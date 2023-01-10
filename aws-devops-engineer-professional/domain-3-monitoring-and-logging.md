---
description: This page will discuss key concepts related to monitoring and logging.
---

# Domain 3: Monitoring and Logging

### Certification Objectives

* Determine how to set up the aggregation, storage, and analysis of logs and metrics.
* Apply concepts required to automate monitoring and event management of an environment.
* Apply concepts required to audit, log and monitor operating systems, infrastructures and applications
* Determine how to implement tagging and other metadata strategies

#### Monitoring and logging with Amazon CloudWatch

Amazon CloudWatch is the default monitoring tool for most AWS resources.&#x20;

* Collect: Metrics and logs
* Monitor: Alarms and dashboards
* Act: Auto scaling and events
* Analyse: Trends and metric math
* Compliance and security

It can be used to monitor the applications you run on AWS. All of these give you system-wide visibility.

Important CloudWatch metrics:

* Metrics are kept for 15 months, the older the data the less granular they become. After 2 weeks (5 min intervals), after months it will be hourly intervals, etc.

Key Metrics for Exam:

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>Amazon CloudWatch metrics</p></figcaption></figure>

Know these ELB metrics:

* **SurgeQueueLength**: This measures the total number of requests queued by your load balancer. An increased metric for surge queue length indicates that the backend systems aren’t able to process incoming requests as fast as the ELB requests being received.
* **SpillOverCount**: When the above happens, the requests get dropped, hence the SpillOverCount.

Elastic Load Balancer Access Log File

ELB publishes as a log file for each load balancer node every five minutes and it will be up to you to understand what kind of information is contained in these logs. What that means for the general system&#x20;

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption><p>Elastic Load Balancer access log file</p></figcaption></figure>

Know these EC2 metrics:

* **StatusCheckFailed**: Reports whether the instance has passed both the instance status check and the system status check at the last minute.
* **CPUCreditUsage**: The number of CPU credits spent by the instance for CPU utilization. One CPU credit equals one vCPU running at 100% utilization for one minute or an equivalent combination of vCPUs, utilization, and time (for example, one vCPU running at 50% utilization for two minutes or two vCPUs running at 25% utilization for two minutes).
* **CPUCreditBalance**: The number of earned CPU credits that an instance has accrued since it was launched or started. For T2 Standard, the CPUCreditBalance also includes the number of launch credits that have been accrued.

Know these HTTP status code metrics:

* **HTTPCode\_Backend\_5xx**: Instances or databases might be at capacity, check their metrics to verify
* **HTTPCode\_ELB\_4xx**: Check instance logs, connections are timing out

Check latency metrics:

* If latency increases during load testing your application might not scale horizontally e.g. nog auto-scaling or DB is bottlenecked or calls to external services are slow.

#### Amazon CloudWatch Logs

With CloudWatch logs, you are able to send log data to CloudWatch from within your EC2 systems. As well as, your on-premise servers.

You can also send logging information coming from your other services, such as CloudTrail logs into CloudWatch logs for consumption. This is all coming from the CloudWatch agent installed on your EC2 resources or on-premises systems.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption><p>Amazon CloudWatch logs</p></figcaption></figure>

In general, all serverless architecture is going to be streaming their logs through the CloudWatch log service.

#### Amazon CloudWatch alarms and events

There are simple threshold alarms that can be set to notify you and trigger automated actions. While the CloudWatch alarm is how you establish your thresholds, the CloudWatch events are a near real-time stream of system events that describe changes in AWS resources.

Using simple rules that you can quickly set up, you can match events and route them to various target functions.&#x20;

#### Amazon CloudWatch rules

The CloudWatch rules match the events to the targets. The rules help you to outline what actions to take and what information is sent when actions trigger the rules.

#### Amazon CloudWatch targets

A target simply receives the events in JSON formatting and then processes the events. Targets can be EC2 instances, AWS Lambda functions, SNS topics and more:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption><p>Amazon CloudWatch targets</p></figcaption></figure>

#### Visualising Amazon CloudWatch

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p>Amazon CloudWatch flowchart example</p></figcaption></figure>

### Additional monitoring and logging services

#### Monitoring with AWS CloudTrail

* Track user activity and API usage
* Log, continuously monitor, and retain account activity related to actions

AWS CloudTrail records the AWS API calls for your account and provides visibility over API activity on the AWS platform.&#x20;

It also records console login events. These events are stored whether they succeed or they fail. All of the API calls going through the cloud are stored in the CloudTrail logs.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>CloudTrail logs</p></figcaption></figure>

#### AWS CloudTrail best practices

* Enable in all regions
* Enable log file validation
* Encrypted logs
* Integrate with Amazon CloudWatch logs
* Centralise logs from all accounts
* Create additional trails as needed

#### AWS X-Ray

* Identify performance bottlenecks and errors
* Pinpoint issues to specific service(s) in your application
* Identify the impact of issues on users of the application
* Visualise the service call graph of your application

Utilising this service, we can track a single call as it makes its way through our entire system, going through the front end, the back end, and eventually making its way to the database. You can see where the issue lies.

**Monitoring with Amazon Kinesis**

* Collect process and analyze real-time streaming data (for quick incident response)
* Logs are ingested via Kinesis data streams or Firehose
* The analysis is done with Kinesis Data Analytics
* Use Kinesis Firehose if you need a fully managed service to transfer data to S3, Redshift, and Elasticsearch of Splunk.
* Use Kinesis data streams if real-time logs are needed. However, it required more effort to set up and manage.
* A good use case to implement Kinesis Firehose is if you centralize CloudWatch log events and move the data to S3 for longer retention and storage

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>Using Kinesis to gather and process logging data</p></figcaption></figure>

#### Logging best practices

* Plan your monitoring strategy working backwards
* Deliver your monitoring resources as part of a group
* Collect four
  * Metric, logs, events and traces
* Monitor everything
  * Services, limits, costs, API interaction, etc
* Leverage other AWS services to enhance observability

### Tagging

AWS allows you to assign metadata to your resources in the form of tags. Each tag is a simple label of customer-defined keys. It can make it easier to manage, search for and filter resources.&#x20;

#### Tagging best practices

* Standardised, case-sensitive format for tags
* Implement automated tools to help manage resource tags
* Favour using too many tags rather than too few
* It's easy to modify tags
* Example: App Version, ENV, DNS Name, App Stack Identifier&#x20;

### Test Axioms

* Know Amazon CloudWatch in-depth
  * Remember that metric data is kept for a period of 15 months
* Know the difference between the logging options and which one is more cost-effective based on the needs
* When you see a question about reporting on an API call, that's likely going to be AWS CloudTrail
* Understand tagging to categorise resources

