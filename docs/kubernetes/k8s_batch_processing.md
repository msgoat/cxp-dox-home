# Batch processing on Kubernetes

Being capable of running batch processes besides regular applications is one of the key features of 
cloud native application platforms like Kubernetes. 
Jobs are the preferred way to perform administrative tasks on an application level.

A typical administrative task on application level could be:

* running a schema migration on your database
* backing up / restoring your database
* load some data into or extract some data from your database
* pruning some obsolete data from your database
* trigger some housekeeping inside your database or application

From a Kubernetes point of view a job does not differ that much from a regular application:

* it runs as a pod (or group of pods) on Kubernetes like an application does
* it requires a Docker image which contains the batch program or batch script like an application does
* it terminates when the batch processing is done which an application does not!

## Best Practices for batch processing on Kubernetes

### Always keep your batch code separate from your application code

Batch processing can get quite complex and resource consuming: So it's always a good idea to keep 
the batch code inside a standalone program separate from your regular application. Thus, batch processing
cannot influence the performance of your application and vice versa. Besides, keeping batch and application
separate allows you to scale them independently.

### Use existing batch frameworks inside your batch program

Consider using existing batch frameworks like [Java Batch](https://www.baeldung.com/java-ee-7-batch-processing) 
or [Spring Batch](https://spring.io/guides/gs/batch-processing/) in your batch program. These
frameworks do all the heavy lifting (like checkpoint/restart logic) allowing you to focus on your 
specific batch logic.

### Kubernetes is not the right tool when you need complex batch pipelines

Although the Kubernetes job controller is quite capable of managing jobs, it is not a full-blown batch processing engine you might
know from a traditional mainframe with checkpoint/restart or predecessor/successor features.
So if you need to build a complex batch pipeline, you definitely will have to add some commercial 
batch management software which runs on top of Kubernetes.

## Jobs

A `job` represents a one-time task which is automatically started when the job is deployed. After the job has finished,
 all resources acquired by it are released as well (after a certain period of time).

A job may span a single `pod` or a group of `pods`, which may perform the batch processing even in parallel.
 
@see [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)

@see [Job Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#job-v1-batch)

## CronJobs

A `cronjob` represents a `job` on a repeating schedule defined in cron format (hence the name).

Cronjobs are useful for performing periodic and recurring tasks, like database backups and database reorgs.

@see [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

@see [CronJob Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#cronjob-v1-batch)
