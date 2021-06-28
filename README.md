# Quick Start

## Quick Start

You can follow along step-by-step. If you have already completed some steps, feel free to skip them.

## 0. Prerequisites

* A Developer Account \(Register for one at  [Developer Portal](https://developer.mysmitch.com) \)
* An App and a `TEST` API Key \(more details at [API Keys](app/api-keys.md)\)
* A tester linked to your App \(more details at [tester](users/user-tester.md)\)

If you haven't completed any of the above prerequisites you can follow this guide [Developer Portal Quick Start](developer-portal/get-started.md)

## 1. API usage

Please make sure you have completed the [first step](./#1-intial-setup) before trying out the API.

> The Interactive API Swagger reference can be found [here](https://app.api.developer.mysmitch.com/docs)

### 1.1 Add your API Key and check if it's valid

Make a `GET` HTTP Request to `app/details` route and check if it returns a proper response, you can verify that the `app_id` is the same as shown in developer portal app page.

Example Curl Request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/details' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example Response for `200` status code

```text
{
  "status": "success",
  "message": "Got App Details",
  "data": {
    "app_id": "YOUR_APP_ID",
    "api_key_type": "TEST",
    "id": SOME_NUMBER
  }
}
```

![Swagger-Add-APi-Key](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger_add_api_key.png)

![Swagger-GET-App-Details](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-app-details.png)

### 1.2 Get list of users for your App

Make a `GET` HTTP Request to `app/users` route

Example Curl Request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/users' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example Response for `200` status code

```text
{
  "status": "success",
  "message": "Got list of users for app",
  "data": [
    {
      "user_id": "SOME_USER_ID",
      "app_join_date": "UTC_TIMESTAMP"
    },
    {
      "user_id": "SOME_USER_ID",
      "app_join_date": "UTC_TIMESTAMP"
    }
  ]
}
```

![Swagger-GET-App-Users](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-app-users.png)

### 1.3 Get details of a single user

Make a `GET` HTTP Request to `app/user` route and pass the `user_id` in query param. The Response Body will have a list of [devices](devices/device.md) in `user_devices` and list of [scenes](scenes/scene.md) in `scenes.`

Example Curl Request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/user?user_id=SOME_USER_ID' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example Response for `200` status code

```text
{
  "status": "string",
  "message": "string",
  "data": [
    {
      "user_id": "string",
      "home_names": [
        {
          "home_name": "string",
          "room_names": [
            "string"
          ]
        }
      ],
      "user_devices": [
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
      ],
      "scenes": [
        {
          "home_name": "string",
          "scene_name": "string",
          "scene_id": "string"
        }
      ],
      "service": "SH"
    }
  ]
```

![Swagger-GET-App-Users-Devices](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-user-user-devices.png)

![Swagger-GET-App-Users-Scenes](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-user-scenes.png)

### 1.4 Get Current State and Online Status of a single device

Make a `GET` HTTP Request to `app/device/details` route and pass the `user_id` and `device_id` in query param.

> The `device_id` must be present for the `user_id` which you got in the [previous](./#13-get-details-of-a-single-user) step for the request to be successful

The Response Body will have a [device](devices/device.md) object and based on the `device_type` the `available_traits` and `current_state` values will change.  
If the device is online \(has an active connection to our servers\) `is_online` will be `True` and vice-versa. The last online timestamp in `UTC` will also be present in `last_online_timestamp` field.

Example Curl Request

```text
curl -X 'GET' \
  'https://app.api.developer.mysmitch.com/v1/app/device/details?user_id=SOME_USER_ID&device_id=SOME_DEVICE_ID' \
  -H 'accept: application/json' \
  -H 'x-api-key: YOUR_API_KEY'
```

Example Response for `200` status code

```text
{
  "status": "string",
  "message": "string",
  "data": {
    "device_id": "string",
    "name": "string",
    "device_type": "string",
    "home_name": "string",
    "room_name": "string",
    "available_traits": [
      "string"
    ],
    "current_state": {
      "power_status": true,
      "brightness": 100,
      "colour": {
        "r": 255,
        "g": 255,
        "b": 255
      },
      "temperature": 6500,
      "switches": [
        {
          "switch_id": 4,
          "power_status": true
        }
      ]
    },
    "is_online": true,
    "last_online_timestamp": "2021-06-04T12:55:27.467Z"
  }
}
```

![Swagger-GET-App-Users-Devices](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/tutorials/swagger-get-device-current-state.png)

### 1.5 Turn a Plug ON

Make a `POST` HTTP Request to `/app/job/device` with the correct combination of `user_id` and `device_id`.  
The Response Body will have a `job_id` which represents the queued job's id field. It can be used to check if the [job](jobs/job.md) has successfully completed or not, using the `/app/job` route.

Example Request Body

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
      "device_id": "PLUG_DEVICE_ID",
      "device_settings": {
        "power_status": true

      }
    }
  ]
}'
```

Example Response Body

```text
{
  "status": "success",
  "message": "New Job for changing Device Settings Queued",
  "data": {
    "job_id": "7aac5282-392f-43f8-a0d4-1f08063164be"
  }
}
```

## 2. Next Steps

Now you can also try out more [tutorials](tutorials/tutorials.md) , where we show you how to change multiple traits for a single device, trigger a scene and change multiple traits for multiple devices in a single request.

