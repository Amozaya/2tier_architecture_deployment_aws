## why do we need monitoring?

* Organisation can catch problems early
* Can help to identify security risks
* Analyze capacity issues

## Four Golden Signals of Monitoring

* Latency - check the time it takes to serve the request (respond time)
* Traffic - the total number of requests across the network (user demand)
* Errors - how many of the requests have failed
* Saturation - the load on your network and servers

## What is Alert Management?

Alert Management monitors the "health" of your systems to check if there are any abnormal activitues, and takes actions which are defined by us. The actions might be notifying us by email about the potential problem or using ASG in order to reduce the load on the system.

**Simple Notification Service (SNS)**

SNS is a cloud service that delivers a warning notification about the fault on the system to subsribing recipient.

**Simple Queue Service (SQS)**

Helpt you to manage the requests by ensuring they are being processed in order (First Come - First Served)

**Cloud Watch**

CloudWatch used for monitoring and managing services and resources that are in use. 


## How to create a SNS

1. In AWS Console search for `SNS`
2. Go to `Topics`
3. Click on `Create topic`
4. As Type use `Standard`
5. Name your topic, for example `oleg-tech221-cpu-topic`
6. Click on `Create topic` at the bottom
7. Once topic created, you need to add subscription. Click on `Create subscription`
8. In `Protocol` select Email, in order to send an email notification
9. In `Endpoint` enter your destination email.
10. Click on `Create Subcription`. You will be required to go to your email account and verify it.

## Create an alarm

1. Start your `EC2 instance`
2. Once it's running, go to `Action` -> `Monitor and Troubleshoot` -> `Manage CloudWatch alarm`
3. You can either `Create an Alarm` or use `Edit an alarm` to use an excisting alarm
4. In `Alarm notification` select the SNS topic you created
5. In `Alarm thresholds` chose what you want to monitor:
    * `Group Samples by` - select `Average`
    * `Type of data to sample` - CPU Utilization
    * `Alarm when` - select condition, for example `>=`
    * `Percent` - value at which alarm will be triggered, for example `50` (%)
6. Click `Create`
7. Now you will have an alarm on your EC2 instance, and if your CPU usage goes above 50% it will trigger an alarm and will send an email with a warning