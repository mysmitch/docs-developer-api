# Introduction

All the `POST` API Calls to `/app/job/*` routes trigger a [Celery](https://docs.celeryproject.org/en/stable/) Task and are processed by queue workers and are thus Asynchronous by nature.

> Currently, we are not Automatically Retrying Failed Jobs

## Job Status

Since Jobs are `Asynchronous` , the app can use [/app/job](https://app.api.developer.mysmitch.com/docs#/job/get_current_status_for_a_queued_job_v1_app_job_get) route to get the current status. Example response

```text
{
  "task_id": "817edc19-532e-48e0-b39e-1570c15f52e6",
  "task_status": "SUCCESS",
  "task_result": true
}
```

Here, the `task_status` key will have either one of the values listed below

* PENDING : The job is waiting for execution.
* STARTED : The job has been started.
* SUCCESS : The job is executed successfully.
* FAILURE : The job raised an exception and failed to execute.

## Delayed Jobs

To execute the job after some time \(the current lower limit is `5` seconds and max limit is `86400` seconds\), the app can send the required time in the `delay` field \([API Reference Link](https://app.api.developer.mysmitch.com/docs#/job/create_a_new_job_for_a_device_v1_app_job_device_post)\)

> The job is guaranteed to be executed at some time after the specified `delay`, but not necessarily at that exact time. Reasons could either be that many items are waiting in the queue or there is heavy network latency.

