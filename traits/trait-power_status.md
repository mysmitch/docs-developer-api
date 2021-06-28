# Power Status

The `power_status` trait is present for all the device types. It's a `boolean` value and `True` corresponds to the device being `ON` and vice-versa.

> This is different from the [is\_online](../devices/device.md#is_online) state, which represents whether the device is connected to our servers or not. If the device is offline, none of the traits can be changed.

