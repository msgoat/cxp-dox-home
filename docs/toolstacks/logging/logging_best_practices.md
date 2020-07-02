# Cluster Logging Best Practices

Here's a short list of best practices regarding cluster logging using EFK.

## Make sure everything is logged to console

Writing logfiles to the filesystem does not make sense in a cloud environment,
since you are just wasting precious storage which you will not be able to access
anyway. These are the days to retire the good old file appender. You should
log everything directly to the console (stdout). Everything written to the console
will be picked up by the container runtime which stores these log events as container
logfiles or system logfile, depending on your container runtime configuration.

## Avoid multiline logging

Avoid multiline logging at any cost, since each line will produce a separate log event
which are hard to regroup in Kibana. Even if you have put a lot of effort into
avoiding multiline log messages, stack traces will be still written as multiline
log messages. Consider switching from a text based output format to a JSON output format.

## Use JSON output format

Configure your logging framework to use JSON output format 
(should be possible with any cloud native Java tech stack). 
This ensures that each log entry will produce only one log event even if the 
log entry includes a multiline log message. Besides, JSON log events can be parsed 
and filtered by fields provided by the JSON log event which facilitates 
structured queries on log event payload.​

## Make sure you only log what you have to log

Cloud storage for log events is nearly unlimited but you will have to pay for it!
So make sure you only log what you need to log on application level. 
To reduce the amount of log events routed through the logging stack even further, 
consider adding some filters to FluentBit or FluentD which drop all non-relevant 
log events before they are stored in Elasticsearch. 

## Good housekeeping is a MUST

Consistently apply housekeeping to your log event database by archiving or deleting 
log events in ElasticSearch periodically (which is actually done by the Elasticsearch Curator).

## Always add context information

Add context information to each log event to be able to correlate log events 
during log analysis. 

Useful context information could be:

* the user ID of the currently authenticated user
* the correlation ID of the current HTTP request (like Jaeger Trace-IDs)

Retrieving this context information can be only down within the application.
Many logging frameworks support `Mapped Diagnostic Context`s (MDC), which is the
right way to add context information as key-value pairs to each log event.