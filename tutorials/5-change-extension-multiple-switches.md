# Changing multiple switches power status for extension device

To know more about extension devices in general, refer to [extension device guide](../devices/device-extension.md)

> This tutorial assumes that you are already familiar with using the API to get details for a specific user, if not please refer to this [quick-start get details of a single user](../#13-get-details-of-a-single-user)

## 0. Get extension device\_id for changing device settings

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


## 1. Trigger a job  to change multiple switches power\_status

Make a `POST` HTTP request to `/app/job/device` with the `user_id` and `device_id` and the desired `device_settings` \(they should only contain keys that are **present** in the `available_traits` for this device\) in the request body `commands` field. In our case, the device\_settings consists of an array of `switches` object.

Example: Curl request to change first two switches to `OFF` and last two switches to `ON`

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
      "device_id": "SOME_EXTENSION_DEVICE_ID",
      "device_settings": {
        "switches": [
          {
            "switch_id": 1,
            "power_status": false
          },
          {
            "switch_id": 2,
            "power_status": false
          },
          {
            "switch_id": 3,
            "power_status": true
          },
          {
            "switch_id": 4,
            "power_status": true
          }
        ]
      }
    }
  ]
}'
```

Example response for `200` status code

```text
{
  "status": "success",
  "message": "New Job for changing device settings queued",
  "data": {
    "job_id": "2246c763-56cc-4f8d-970a-7c28dc18bbe5"
  }
}
```

![Swagger-POST-job-extension-multiple-switches](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-post-job-extension-multiple-switches.png)

## 2. Check if job has been triggered successfully \(OPTIONAL\)

Use the `job_id` from step [1](5-change-extension-multiple-switches.md#1-trigger-a-job--to-change-the-device-settings) and make a `GET` HTTP Request to `/app/job` route with the `job_id` in query param. You can check the [job](../jobs/job.md) status from the `task_status` key in the response body.

Example curl request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/job?job_id=2246c763-56cc-4f8d-970a-7c28dc18bbe5' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example response for `200` status code

```text
{
  "task_id": "2246c763-56cc-4f8d-970a-7c28dc18bbe5",
  "task_status": "SUCCESS",
  "task_result": true
}
```

![Swagger-GET-job-scene-status](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-job-status.png)

