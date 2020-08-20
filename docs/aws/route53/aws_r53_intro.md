# Introducing AWS Route 53

Amazon Route 53 is an AWS service that represents a highly available and scalable DNS service.

It offers the following capabilities:

* Domain registration
* DNS routing
* Health checking

@see [What is Amazon Route 53?](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html)

## Domain Registration 

AWS Route 53 allows you to register domain names, 
transfer existing domain name registrations between Route 53 and other registrars.

Additionally, you can manage sub domains of an existing domain registered with another registrar.

@see [Registering domain names using Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar.html)

!!! info "Domain __msgoat.eu__ is hosted by Route 53"
    The internet domain __msgoat.eu__ that we are using throughout this Cloud Expert Program is registered
    with Route 53.
    
## DNS Routing

You can use Route 53 as the DNS service for your domain, translating friendly 
domain names like __www.msgoat.eu__ into numeric IP addresses.

### Hosted Zones

In order to do so, Route 53 uses the concept of `hosted zone`s. A hosted zone comes in two flavours:

* __Public hosted zone__: a public hosted zone routes internet traffic to AWS resources like AWS Application
Loadbalancers or EC2 instances. DNS names managed in a public hosted zone are visible to the internet.
* __Private hosted zone__: a private hosted zone allows you to route traffic within a VPC. 
DNS names managed in a private hosted zone are only visible within the AWS network.

@see [Working with hosted zones](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-working-with.html)

### DNS inside EC2 with Route 53 Resolver

Whenever you setup a VPC in AWS, a DNS resolver called `Route 53 Resolver` is automatically enabled and attached to it. 
This DNS resolver answers DNS queries for local VPC domain names for EC2 instances like __ec2-10-1-2-44.compute-1.amazonaws.com__
and records in private hosted zones.

Additionally, the Route 53 Resolver can be configured to accept DNS request from outside 
the AWS network to your VPC or vice versa. This is done by adding forwarding rule to the Route 53 Resolver 
and be adding inbound endpoints and outbound endpoints to your VPC.

@see [Resolving DNS queries between VPCs and your network](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver.html)

### DNS Records

In order to route traffic you need to create DNS records in a hosted zone. 

Here are the most commonly used DNS record types:

| Record Type | Description | Example |
| --- | --- | --- |
| A | Routes traffic to a resource such as an EC2 instance or an Application Load Balancer using IPv4 addresses. | |
| AAAA | dito but with IPv6 addresses | |
| CNAME | Maps DNS queries to this records name to another domain | |
| SOA | A Start of authority (SOA) provides information about a domain and a corresponding hosted zone. Automatically added to a new hosted zone when it is created. | | 
| NS | Identifies the name servers for a hosted zone. Automatically added to a hosted zone when it is created. Need to be copied to other hosted zones if you want to link them. | |

Both A and AAAA records can be `alias records` which are a Route 53-specific extension to DNS.
Alias records work like CNAME records: they map a DNS name to another DNS name. Unlike CNAME records,
alias records can be created for the top node of a DNS namespace as well.

Targets for alias records can be:

* API Gateways
* VPC interface endpoints
* CloudFront distributions
* Elastic Beanstalk environment
* Elastic Load Balancer
* AWS Global Accelerator accelerator
* S3 bucket
* Another record in the same hosted zone

@see [Working with records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/rrsets-working-with.html)

## Health checks and DNS failover

Route 53 offers the possibility to create health checks for targets which are referenced through records
in hosted zones. This allows you to introduce automatic DNS failover, if one of your targets becomes unhealthy.
 
@see [Creating Amazon Route 53 health checks and configuring DNS failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover.html)