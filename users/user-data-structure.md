# Data Model

![Smitch Home user data structure](https://d199xmsg3owom4.cloudfront.net/images/developer_portal/sd/user_homes_rooms_devices.png)

A user's information is stored as shown in the above structure. A user can have multiple homes. This allows the user to configure and manage devices in multiple homes. A user consists of the following:

* `home_names`, an array of homes managed by the user
* `user_devices`, an array of devices belonging to the user
* `scenes`, an array of scenes created by the user

A home belonging to the user consists of the following,

* `home_name`, the name assigned to the home by the user
* `room_names`, the names assigned to the rooms that are present in the home

A room can have multiple devices. But a device can only be present in one place. A user device consists of the following:

* `name`, the name given by the user to the device
* `device_type`, the type of the device.
* `home_name`, the name of the home in which the device is present
* `room_name`, the room inside which the device is present in the home
* `available_traits`, an array of different traits the device can perform
* `device_id`, a unique identifier of the device

Scenes allow the user to assign a desired state to multiple devices at the same time, like turning all the bulbs on or off. A scene, like a room, belongs to a single home and consists of the following:

* `home_name`, the home to which this scene belongs to
* `scene_name`, the name of the scene given by the user
* `scene_id`, a unique identifier of the scene

For example, let us consider a user who has two homes:

```yaml
{
  "user_id": "e2a81a43a2249b41006a1ebb9b625f23df802174a235aa2e41e9f36a0b007ce66e702079c494e9a305699a5c3abc55a12312i3yr87a7sdfs32r131312bd7fbbb",
  "home_names":
    [
      { "home_name": "First Home", "room_names": ["Bedroom", "Living Room"] },
      { "home_name": "Second Home", "room_names": ["Bedroom", "Hall"] },
    ],
  "user_devices":
    [
      {
        "device_id": "f9e2a3bded1d51775b8857bbc83b334f1d5bfda0e03bc23234fae2431241e1",
        "name": "AC",
        "device_type": "plug",
        "home_name": "First Home",
        "room_name": "Bedroom",
        "available_traits": ["power_status", "power_consumption"],
      },
      {
        "device_id": "f9e2a3bded1d51775b8857bbc83b334f7e357396e23e3c0a67dac1uu981hf2",
        "name": "night light",
        "device_type": "light",
        "home_name": "Second Home",
        "room_name": "Hall",
        "available_traits":
          ["power_status", "colour", "brightness", "temperature"],
      },
    ],
  "scenes":
    [
      {
        "home_name": "First Home",
        "scene_name": "Deactivate Lights",
        "scene_id": "e2a81a43a2249b41006a1ebb9b6765435ae2765c435826127baef887994416ad0fcf36ed3133bfdfe046cf7568dd8868746f69e1ee8b76495dd094c16422ed5b",
      },
      {
        "home_name": "Second Home",
        "scene_name": "Active Lights",
        "scene_id": "e2a81a43a2249b41006a1ebb9b67635ae782765c435826127baef887994416ad0fcf36ed3133bfdfe046cf7568dd8868746f69e1ee8b76495dd094c16422ed5b",
      }
    ],
  "service": "SH",
}
```

This data is obtained using the [get details of user API](https://app.api.developer.mysmitch.com/docs#/user/get_details_for_a_single_user_v1_app_user_get). To use this API [create an application](../app/app.md) in your workspace and [add testers](user-tester.md) to the app. Make sure that the testers have a Smitch Home account. You can obtain a list of all the testers present in your app using the [list users API](https://app.api.developer.mysmitch.com/docs#/user/get_list_of_users_installed_app_v1_app_users_get)

Here, the user has two homes, namely `First Home` and `Second Home`. First Home has two rooms, `Bedroom` and `Living Room`. Similarly, Second Home has two rooms, `Bedroom` and `Hall`. As seen here, the same room names can be present in multiple homes but the names of homes will always be unique. Looking at `user_devices` we can see that this user has two devices. The device `AC` is present in the room Bedroom of First Home. We can also see that the device is a plug along with the different traits that this device has. Similarly, the device `night light` is present in the room Hall of Second Home. It is a light which allows us to modify its brightness, colour, colour temperature and also to turn it on/off. Learn more about devices types and device traits [here](../devices/device.md)

The current state of the device can be obtained through [get device details API](https://app.api.developer.mysmitch.com/docs#/device/get_device_details_v1_app_device_details_get). You can modify or set your desired state for one or multiple devices through the [create job for device API](https://app.api.developer.mysmitch.com/docs#/job/create_a_new_job_for_a_device_v1_app_job_device_post). You can also trigger a scene using the [create job for scene API](https://app.api.developer.mysmitch.com/docs#/job/create_a_new_job_for_a_scene_v1_app_job_scene_post). Both the 'create job' APIs will queue your request and revert to you with a `job_id`. You can check if a queued job succeeded or failed using the [get job status API](https://app.api.developer.mysmitch.com/docs#/job/get_current_status_for_a_queued_job_v1_app_job_get)

