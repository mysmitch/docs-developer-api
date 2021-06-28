# Introduction

Currently, the [Apps](../app/app.md) can only interact with MySmitch devices. Support for other devices\(Smitch Secure\) will be added soon

## Device Object

Each device object has the following properties: `device_id` , `device_type` ,`available_traits`,`is_online`, `last_online_timestamp` ,`power_consumption` and `current_state`.

### device\_id

It is a unique id representing a single device

### device\_type

It represents the type of device and can have one of the following values: `light, plug, extension`

### available\_traits

The available [traits](device-all-traits.md) that are present for this device are based on its hardware support.

### is\_online

This field will be `true` if the device is online \(has active connection to our servers\) and vice-versa

### last\_online\_timestamp

This field will have the timestamp in UTC for which the last known value of `is_online` was `true`

### power\_consumption

Currently, this field will only be present for `plug` devices, refer to more details [here](device-plug.md)

### current\_state

This field will have an object, which has the current `available_traits` values for the given device.

