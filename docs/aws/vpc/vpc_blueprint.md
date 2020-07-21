# AWS VPC Blueprint

The VPC reference blueprint mentioned here should serve as a template for your own VPC. 
Of course, you are free to tailor the blueprint to your own requirements.

## Overview

![AWS VPC Blueprint](img/aws_vpc_blueprint.png)

### VPC spans all availability zones

The VPC uses a `/16` CIDR block.
It has subnets in all availability zones provided by a region.

### Internet traffic is routed through an internet gateway

An internet gateway is assigned to the VPC to allow inbound and outbound internet traffic.
Specific route table entries in the route table of each public subnet ensure that internet-bound traffic is routed through the internet gateway.

### Subnets subdivide each availability zone in layers
 
Each availability zone contains three subnets. Each subnet represents a logical layer (from top to bottom):

* __Web Layer__: this public subnet hosts all resources which need to be accessible from the internet
* __Application Layer__: this private subnet hosts all resources which run applications
* __Datastore Layer__: this private subnet hosts all resources like databases, messaging systems etc.
   
### Redundant bastion servers allow secure administration of infrastructure

There is a bastion server in the public web layer subnet of each availability zone, which can be accessed from the Internet via SSH. 
Administrators can connect to the bastion servers via SSH, so that they can access other instances and resources within the VPC.

Since a bastion server acts as the point of entry into your confidential VPC, they need to be properly secured:
 
* SSH access should be restricted to specific source addresses using security groups
* Consider using commercial access gateways like [okta Advanced Server Access](https://www.okta.com/products/advanced-server-access/)  

### Private instances are protected by redundant NAT gateways

There is a separate NAT gateway for each availability zone in the public web layer subnet. 
Each NAT gateway has an elastic IP. 
Private route tables for the private subnets ensure that all traffic from the private subnets is routed to the Internet 
via the NAT gateway of the same availability zone.
