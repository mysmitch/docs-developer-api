# Triggering a Scene

To know more about scenes in general, refer [scenes guide](../scenes/scene.md)

> This tutorial assumes that you are already familiar with using the API to get details for a specific user, if not please refer to this [quick-start get details of a single user](../#13-get-details-of-a-single-user)

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

## 1. Trigger the scene

Make a `POST` HTTP Request to `/app/job/scene` with the `user_id` and `scene_id` in the request body.

Example Curl Request

```text
curl -X 'POST' \
  'https://app.api.developer.mysmitch.com/v1/app/job/scene' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
  "user_id": "SOME_USER_ID",
  "scene_id": "SOME_SCENE_ID"
}'
```

Example Response for `200` status code

```text
{
  "status": "success",
  "message": "New Job for triggering Scene  Queued",
  "data": {
    "job_id": "817edc19-532e-48e0-b39e-1570c15f52e6"
  }
}
```

![Swagger-POST-job-scene](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-post-job-scene.png)

## 2. Check if scene has triggered successfully \(OPTIONAL\)

Use the `job_id` from step [1](2-trigger-a-scene.md#1-trigger-the-scene) and make a `GET` HTTP Request to `/app/job` route with the `job_id` in query param. You can check the [job](../jobs/job.md) status from the `task_status` key in the response body.

Example Curl Request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/job?job_id=817edc19-532e-48e0-b39e-1570c15f52e6' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example Response for `200` status code

```text
{
  "task_id": "817edc19-532e-48e0-b39e-1570c15f52e6",
  "task_status": "SUCCESS",
  "task_result": true
}
```

![Swagger-GET-job-scene-status](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-job-status.png)

