# AWS Lambda best practices

Here is a collection of best practices for advanced serverless developers.

## Decouple your lambda functions using an Event Bus like AWS EventBridge

TODO: add text

## Enrich your events with useful and helpful information

In order to ensure a high observability in your serverless application, add additional data to your events which makes
tracking down bugs easier when things should get wrong. Useful and helpful additional data could be:

* The fully qualified name of the lambda function which sent the event
* A unique identifier of the event (correlation ID), if not already provided by the underlying event bus or event broker
* The name of the business domain or bounded context the sending function belongs to
* The name of the business service / operation the sending function represents

## Use Event-Carried State Transfer to avoid unnecessary callbacks between functions

When using event-based communication between your functions, put all state which the consumers of your events might be 
interested in into the events that you fire (event-carried state transfer).

Putting just identifiers of domain objects into your events requires all consuming functions to retrieve the actual domain objects
from the data-owning function. This pull model introduces an unwanted dependency or tight coupling between functions: when the
data-owning function becomes unavailable all functions depending on the data-owner will become dysfunctional as well.

TODO: add image

A far better way is to put all data into the payload of the events that you fire. Thus, the consuming functions are able to process
that data without having to call back the data-owning function. This push model ensures a loose coupling between the functions.

TODO: add image

## Use Functions to transform, not transport

Sometimes, a function only transports data between an event source and an event destination without doing anything with it.
This is a good indication to look for better ways to link the event source and the event destination directly without having
to use a function at all.

TODO: add image

## Apply Orchestration/Choreography using Managed Services

In most non-trivial serverless scenarios, communication between functions must be coordinated using `Orchestration` or `Choreography`
or even both. Regardless which approach you are using, either orchestration or choreography should be handled by a managed service, 
NOT your function code.

!!! danger Avoid putting hard-wired coordination between functions into your function code at any cost!

In `Orchestration`, flow of communication between functions is controlled by a central component called 
an `Orchestrator`. The orchestrator initiates and coordinates interactions between functions, 
all functions only interact with the orchestrator. 

Using AWS Lambda, a popular orchestrator of functions is `AWS Step Functions`.

TODO: add image

`Choreography` on the other hand is a more distributed approach based on collaboration: each function publishes and/or subscribes 
to events and each function reacts to these events independently. The only component that all functions have to rely on it a 
common event broker or event bus which ensures the reliable transport of events between the functions but does NOT 
control the flow of events between the functions in any way.

Using AWS Lambda, a popular common event bus is `AWS EventBridge`.


## Resources:

* [AWS re:Invent 2022 - Best practices for advanced serverless developers (SVS401)](https://youtu.be/PiQ_eZFO2GU)