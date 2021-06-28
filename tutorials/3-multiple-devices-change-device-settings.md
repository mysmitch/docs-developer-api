# Changing Multiple Settings For a Multiple Device

To know more about different devices in general, refer to [devices guide](../devices/device.md)

> This tutorial assumes that you are already familiar with [Tutorial 1](1-single-rgb-colour-brightness-change.md)
>
> ## 0. Get the multiple device\_ids for changing device settings
>
> If a user has `devices`, it will show up in the `GET` `/app/user` route response body. A generic `user_devices` object looks like:
>
> ```text
> {
>     "device_id": "string",
>     "name": "string",
>     "device_type": "string",
>     "home_name": "string",
>     "room_name": "string",
>     "available_traits": [
>     "string"
>     ]
> }
> ```
>
> To know more about the type of device traits refer to [All Traits](../devices/device-all-traits.md)

Copy the relevant `device_id`'s and `available_traits`'s value for next step   


## 1. Trigger a job  to change the device settings

Make a `POST` HTTP request to `/app/job/device` with the `user_id` and `device_id` and the desired `device_settings` \(they should only contain keys that are **present** in the `available_traits` for this device\) in the request body.

> In [Tutorial 1](1-single-rgb-colour-brightness-change.md), you were only passing a single object representing a device with its desired state to the `commands` array. Now, you can simply add more such objects to the `commands` array.

Example: Curl request to change the [power\_status](../traits/trait-power_status.md) of a [Plug](../devices/device-plug.md) to `True` and for a [Light](../devices/device-light.md) Set [brightness](../traits/trait-brightness.md) to `100` and [temperature](../traits/trait-temperature.md) to `6500`

```text
curl -X 'POST' \
  'https://app.api.developer.mysmitch.com/v1/app/job/device' \
  -H 'accept: application/json' \
  -H 'x-api-key: TEST##2a352ddab21c429ca258195c72431041##eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhcHBfaWQiOiJBUFBfODU5N2M1YmM2MDQyNGIxZjg5ZjZiNmM0ODFjOTAyNzUiLCJzY29wZXMiOlsiKiJdfQ.fysS8ecWdsx-eO947QyWeV7cqv_U2gTSvgNdupS_W6k' \
  -H 'Content-Type: application/json' \
  -d '{
  "user-id": "SOME_USER_ID",
  "commands": [
    {
      "device_id": "SOME_PLUG_DEVICE_ID",
      "device_settings": {
        "power_status": false
      }
    },
    {
      "device_id": "SOME_LIGHT_DEVICE_ID",
      "device_settings": {
        "brightness": 100,
        "temperature":6500
      }
    }
  ]
}'
```

Example Response for `200` status code

```text
{
  "status": "success",
  "message": "New Job for changing Device Settings Queued",
  "data": {
    "job_id": "2246c763-56cc-4f8d-970a-7c28dc18bbe5"
  }
}
```

![Swagger-POST-job-multiple-devices-multiple-settings](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-post-multiple-device-multiple-settings.png)

## 2. Check if job has been triggered successfully \(OPTIONAL\)

Use the `job_id` from step [1](3-multiple-devices-change-device-settings.md#1-trigger-a-job--to-change-the-device-settings) and make a `GET` HTTP request to `/app/job` route with the `job_id` in query param. You can check the [job](../jobs/job.md) status from the `task_status` key in the response body.

Example Curl request

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

