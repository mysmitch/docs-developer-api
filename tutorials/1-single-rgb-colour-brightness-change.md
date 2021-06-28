# Changing Multiple Settings For a Single Device

To know more about lighting devices in general, refer to [light device guide](../devices/device-light.md)

> This tutorial assumes that you are already familiar with using the API to get details for a specific user. If not, please refer to this [quick-start get details of a single user](../#13-get-details-of-a-single-user)

## 0. Get device\_id for changing device settings

If a user has `devices`, it will show up in the `GET` `/app/user` route response body. A generic `user_devices` object looks like:

```text
{
    "device_id": "string",
    "name": "string",
    "device_type": "string",
    "home_name": "string",
    "room_name": "string",
    "available_traits": [
    "string"
    ]
}
```

To know more about the type of device traits, refer to [All Traits](../devices/device-all-traits.md)

Copy the `device_id` and `available_traits` value for next step   


## 1. Trigger a job  to change the device settings

Make a `POST` HTTP request to `/app/job/device` with the `user_id` and `device_id` and the desired `device_settings` \(they should only contain keys that are **present** in the `available_traits` for this device\) in the request body `commands` field.

Example: Curl request to change the [colour](../traits/trait-colour.md) to `Green` and set [brightness](../traits/trait-brightness.md) to `90`

```text
curl -X 'POST' \
  'https://app.api.developer.mysmitch.com/v1/app/job/device' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
  "user_id": "SOME_USER_ID",
  "commands": [
    {
      "device_id": "SOME_LIGHT_DEVICE_ID",
      "device_settings": {
        "colour": {
          "r": 0,
          "g": 255,
          "b": 0
        },
        "brightness":90
      }
    }
  ]
}'
```

Example response for `200` status code

```text
{
  "status": "success",
  "message": "New Job for changing Device Settings Queued",
  "data": {
    "job_id": "2246c763-56cc-4f8d-970a-7c28dc18bbe5"
  }
}
```

![Swagger-POST-job-single-device-multiple-settings](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-post-job-single-device-multiple-settings.png)

## 2. Check if job has been triggered successfully \(OPTIONAL\)

Use the `job_id` from step [1](1-single-rgb-colour-brightness-change.md#1-trigger-a-job--to-change-the-device-settings) and make a `GET` HTTP Request to `/app/job` route with the `job_id` in query param. You can check the [job](../jobs/job.md) status from the `task_status` key in the response body.

Example Curl Request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/job?job_id=2246c763-56cc-4f8d-970a-7c28dc18bbe5' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example Response for `200` status code

```text
{
  "task_id": "2246c763-56cc-4f8d-970a-7c28dc18bbe5",
  "task_status": "SUCCESS",
  "task_result": true
}
```

![Swagger-GET-job-scene-status](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-job-status.png)

