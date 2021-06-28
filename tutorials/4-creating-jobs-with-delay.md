# Creating jobs with delay

To know more about jobs in general, refer to [jobs guide](../jobs/job.md)

> This tutorial assumes that you are already familiar with using the API to get details for a specific user, if not please refer to this [quick-start get details of a single user](../#13-get-details-of-a-single-user)

For this tutorial we will trigger a [scene](../scenes/scene.md) with a [delay](../jobs/job.md#delayed-jobs) of `10 minutes`

## 0. Get scene\_id for triggering

If a user has `scenes`, it will show up in the `GET` `/app/user` route response body. A generic `scene` object looks like:

```text
{
    "home_name": "string",
    "scene_name": "string", 
    "scene_id": "string"
}
```

Copy the `scene_id` value for next step

## 1. Trigger the scene with delay

Make a `POST` HTTP request to `/app/job/scene` with the `user_id` and `scene_id` and `delay` of `600` \(10 minutes in seconds\) in the request body.

Example Curl request

```text
curl -X 'POST' \
  'https://app.api.developer.mysmitch.com/v1/app/job/scene' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
  "user_id": "SOME_USER_ID",
  "scene_id": "SOME_SCENE_ID",
  "delay":600
}'
```

Example response for `200` status code

```text
{
  "status": "success",
  "message": "New Job for triggering scene queued",
  "data": {
    "job_id": "817edc19-532e-48e0-b39e-1570c15f52e6"
  }
}
```

![Swagger-POST-job-scene](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-post-job-scene.png)

## 2. Check if scene has been triggered successfully \(OPTIONAL\)

Use the `job_id` from step [1](4-creating-jobs-with-delay.md#1-trigger-the-scene) and make a `GET` HTTP request to `/app/job` route with the `job_id` in query param. You can check the [job](../jobs/job.md) status from the `task_status` key in the response body.

Example curl request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/job?job_id=817edc19-532e-48e0-b39e-1570c15f52e6' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example response for `200` status code if you check before `10 minutes` have passed since step [1](4-creating-jobs-with-delay.md#1-trigger-the-scene) .

```text
{
  "task_id": "817edc19-532e-48e0-b39e-1570c15f52e6",
  "task_status": "PENDING",
  "task_result": true
}
```

Example response for `200` status code after `10 minutes`

```text
{
  "task_id": "817edc19-532e-48e0-b39e-1570c15f52e6",
  "task_status": "SUCCESS",
  "task_result": true
}
```

![Swagger-GET-job-scene-status](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-job-status.png)

