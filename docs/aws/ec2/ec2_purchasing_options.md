# EC2 Instance Purchasing Options

AWS offers a lot of purchasing options for EC2 instance which allows you to save a lot of money
if chosen well. This page does only provide the most common options. For a full list of options, please
refer to the official documentation.

@see: [Instance purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html)

## On-Demand Instances

`On-demand instances` are available when and as long as you request them. On-demand instances are available as bare-metal or 
as virtual machines running on dedicated or shared hardware. 

* You pay for compute capacity with not long-term commitment.
* You have full control over the instance lifecycle.
* Best suited for applications with short-term, irregular workloads which cannot be interrupted.

@see [On-Demand Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-on-demand-instances.html)

## Reserved Instances

`Reserved instances` are ordered in advance in larger numbers, before they are actually used. Since you commit to
a certain number of instances in advance, they are offered for a lower price compared to on-demand instances.

* You pay significantly less than for on-demand instances.
* You make a commitment to a consistent usage amount (Saving Plans with USD per hour).
* Best suited for applications with long-term constant workloads which cannot be interrupted.

@see [Reserved Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-reserved-instances.html)

## Spot Instances

`Spot instances` allow you to request spare EC2 computing capacity for up to 90% off the on-demand price which can
be reclaimed by AWS at any time. 
The hourly price for a Spot instance is called a Spot price.
A Spot price for each instance type in each availability zone is set by AWS depending on free capacity.

* You pay significantly less than for on-demand instances.
* You bid for a certain price for a mix of instance types from different instance pools: 
as long as the current spot price is below your bid price, AWS will fulfill your request. 
As soon as the current spot price exceeds your bid price, AWS will remove the instance after a 
two-minute notice.
* Best suited for applications with can be interrupted and easily transferred to other instances 
(like worker nodes in a EKS cluster).

Here are the concepts of spot instances:

* __Spot Instance Pool__ is a set of unused EC2 instances with the same instance type, 
operating system, availability zone and network platform.

* __Spot price__ is the price of a spot instance per hour.

* __Spot Instance Request__ requests a set of spot instances from a spot instance pool with a
maximum price you are willing to pay. As long as the maximum price is more than the current spot price,
AWS will fulfill your request.

* __Spot Fleet__ is a set of spot instances based on the criteria you specify (like spot instance pools, max price, target capacity).

* __Spot Instance interruption__ will terminate, stop or hibernate your spot instance when the current spot price
exceeds the maximum price you set in the spot instance request. 
In case of interruption you will be provided with an interruption notification before the spot instance is removed from the pool two minutes later.

!!! tip "Check Spot Instance pricing on a regular base"
    Spot prices may rise or fall by the hour: current spot prices can be checked via the 
    [EC2 console](https://eu-west-1.console.aws.amazon.com/ec2) > Spot Requests > Pricing history.

## Mixing Purchasing Options

### Autoscaling Groups

Autoscaling Groups allows you to manage a weighted mix of on-demand and spot instances.

!!! info "Optimize your EKS costs with spot instances"
    All worker nodes of our CXP EKS cluster are spot instances which significantly reduces
    the TCO for our CXP cluster.
    
@see [Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/AutoScalingGroup.html)

### EC2 Fleet

An `EC2 Fleet` contains the configuration information to launch a fleet or group—of instances. In a single API call, a fleet can launch multiple instance types across multiple Availability Zones, using the On-Demand Instance, Reserved Instance, and Spot Instance purchasing options together.

@see [Launching an EC2 Fleet](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-fleet.html)

### Spot Fleet

A `Spot Fleet` is a collection or fleet of spot instance plus optional on-demand instances.
The Spot Fleet attempts to launch the number of Spot Instances and On-Demand Instances to meet 
the target capacity that you specified in the Spot Fleet request. 
The request for Spot Instances is fulfilled if there is available capacity and the maximum 
price you specified in the request exceeds the current Spot price. 
The Spot Fleet also attempts to maintain its target capacity fleet if your Spot Instances are interrupted. 

@see [How Spot Fleet works](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html)