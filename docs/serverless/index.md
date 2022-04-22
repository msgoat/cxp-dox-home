# Serverless Computing

Running your workload `serverless` on a `Function as a Service` platform (FaaS) is the latest offering of all Cloud
Service Providers promising

* an application development approach which allows you to focus on coding without having to deal with infrastructure
  like networks and servers
* a very cost-efficient way to run workloads since you are only charged when your application is actually used
  (i.e. a `function` of your application is invoked)

@see [Cloud Service Categories - Function as a Service](../cloud/cloud_service_categories.md#function-as-a-service-faas)

## What is serverless computing?

> `Serverless computing` is a method of providing backend services on an as-used basis. 
> Servers are still used, but a company that gets backend services from a serverless vendor is charged based on usage, 
> not a fixed amount of bandwidth or number of servers.

## PROs and CONs of serverless computing

> TODO: add a table with the headlines from the following details chapter

## Advantages of serverless computing

### No server management

Although 'serverless' computing does actually take place on servers, developers never have to deal with the servers.
They are managed by the vendor. This can reduce the investment necessary in DevOps, which lowers expenses, and it also
frees up developers to create and expand their applications without being constrained by server capacity.

### Reduced cost due to pay-as-you-go

As in a 'pay-as-you-go' phone plan, developers are only charged for what they use. Code only runs when backend functions
are needed by the serverless application, and the code automatically scales up as needed. Provisioning is dynamic,
precise, and real-time. Some services are so exact that they break their charges down into 100-millisecond increments.
In contrast, in a traditional 'server-full' architecture, developers have to project in advance how much server capacity
they will need and then purchase that capacity, whether they end up using it or not.

### Serverless is inherently scalable

Applications built with a serverless infrastructure will scale automatically as the user base grows or usage increases.
If a function needs to be run in multiple instances, the vendor's servers will start up, run, and end them as they are
needed, often using containers.
(The function will start up more quickly if it has been run recently – see 'Performance may be affected' below.)
As a result, a serverless application will be able to handle an unusually high number of requests just as well as it can
process a single request from a single user. A traditionally structured application with a fixed amount of server space
can be overwhelmed by a sudden increase in usage.

### Facilitates quick deployments and updates

Using a serverless infrastructure, there is no need to upload code to servers or do any backend configuration in order
to release a working version of an application. Developers can very quickly upload bits of code and release a new
product. They can upload code all at once or one function at a time, since the application is not a single monolithic
stack but rather a collection of functions provisioned by the vendor.

This also makes it possible to quickly update, patch, fix, or add new features to an application. It is not necessary to
make changes to the whole application; instead, developers can update the application one function at a time.

### Better latency through edge locations

Because the application is not hosted on an origin server, its code can be run from anywhere. It is therefore possible,
depending on the vendor used, to run application functions on servers that are close to the end user. This reduces
latency because requests from the user no longer have to travel all the way to an origin server. Cloudflare Workers
enables this kind of serverless latency reduction.

## Disadvantages of serverless computing

### Testing and debugging become more challenging

It is difficult to replicate the serverless environment in order to see how code will actually perform once deployed.
Debugging is more complicated because developers do not have visibility into backend processes, and because the
application is broken up into separate, smaller functions. The Cloudflare Workers Playground is a sandbox that helps
reduce friction in testing and debugging.

### Introduces new security concerns

When vendors run the entire backend, it may not be possible to fully vet their security, which can especially be a
problem for applications that handle personal or sensitive data.

Because companies are not assigned their own discrete physical servers, serverless providers will often be running code
from several of their customers on a single server at any given time. This issue of sharing machinery with other parties
is known as 'multitenancy' – think of several companies trying to lease and work in a single office at the same time.
Multitenancy can affect application performance and, if the multi-tenant servers are not configured properly, could
result in data exposure. Multitenancy has little to no impact for networks that sandbox functions correctly and have
powerful enough infrastructure. For instance, Cloudflare runs a 15-Tbps network with enough excess capacity to mitigate
service degradation, and all serverless functions hosted by Cloudflare run in their own sandbox (via the Chrome V8
engine).

### Not built for long-running processes

This limits the kinds of applications that can cost-effectively run in a serverless architecture. Because serverless
providers charge for the amount of time code is running, it may cost more to run an application with long-running
processes in a serverless infrastructure compared to a traditional one.

### Performance issues through cold starts

Because it's not constantly running, serverless code may need to 'boot up' when it is used. This startup time may
degrade performance. However, if a piece of code is used regularly, the serverless provider will keep it ready to be
activated – a request for this ready-to-go code is called a 'warm start.' A request for code that hasn't been used in a
while is called a 'cold start.'

Cloudflare Workers largely avoids the cold-starting issue by using the Chrome V8 engine, which in most cases is able to
start up and run JavaScript code in under 5 milliseconds. If the code is already running, the response time is under a
millisecond. Learn more about the performance of different serverless platforms.

### Inevitable vendor lock-in

Allowing a vendor to provide all backend services for an application inevitably increases reliance on that vendor.
Setting up a serverless architecture with one vendor can make it difficult to switch vendors if necessary, especially
since each vendor offers slightly different features and workflows. (Cloudflare Workers are easier to migrate because
they are written in JavaScript and written against the widely used service workers API.)

## When to use serverless computing

### tl;dr Most of the time!

TODO continue...

### Mobile first products

Mobile-first products mostly use small-sized messages which trigger business logic with 
a low to medium complexity which can be implemented easily using functions. Additionally,
users of mobile devices are used to a sometimes unreliable mobile network with unpredictable 
latencies.

### Endpoints for standardized protocols

Systems providing endpoints for standardized protocols (like e-mobility ecosystems)

## When NOT to use serverless computing

### Real time systems

Cold starts of functions and an application gateway in front of the functions may introduce
some additional unexpected latency which makes it hard to build "real" real-time systems
based on serverless platforms.

### Resource intensive tasks

Functions are bound to hard limits regarding CPU and RAM usage which makes the serverless
approach not suitable for tasks which either require a lot of computational resources or memory resources.
Virtual machines or even physical servers will be the better way to go for these 
resource-intensive tasks.

### Very mission critical systems

A product based on serverless technology is running on someone else's system that you can not
control. So what happens, if this system or parts of it go down? Additionally, on most
cloud service providers, a serverless platform is highly dependent on other essential 
cloud services (like AWS Lambda on AWS S3) which introduces another point of failure.

### Where you need (more) control

When using the serverless approach, you will definitely lose control over things like
configuration of systems, issue resolution (since the platform is managed by a third party) and
security (e.g. data for regulatory needs). Unfortunately, there is no such thing
like `confidential computing` on a serverless platform yet.

## Serverless on AWS with AWS Lamdba

On AWS, [AWS Lambda](../aws/lambda/lambda_intro.md) is the service which allows you to write serverless applications.

## Serverless on Azure with Azure Functions

On Azure, Azure Functions is the platform to host serverless applications.

## Resources

* [When should you use a Serverless Approach? • Paul Johnston • GOTO 2017](https://youtu.be/1fBbSgJJV_g)
* [Confusion in the Land of the Serverless • Sam Newman • GOTO 2018](https://youtu.be/Y6B3Eqlj9Fw)