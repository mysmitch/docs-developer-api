# Plugs

Generally consists of Devices which are Plugs and has two sub-types `plug-6A` and `plug-16A`  
These devices also have the power consumption usage recorded in `kWH` for the last 30 days, and can be found in `power_consumption` field inside the `data` key of [Get Device Details](../#14-get-current-state-and-online-status-of-a-single-device)

Each object inside the `power_consumption` array has three more fields  
1. total - Total Power Usage in kWH

1. date - Date when power usage was recorded in `YYYY-MM-DD` format
2. slot\_wise - This is a floating point integer array of `96` length \(15 min slots\), the first slot \(`0` index\) represents the first 15mins of a Day in `UTC`, i.e 00:00-00:15 \(`HH:MM`\), second slot\(`1` index\) represents 00:15-00:30.

> The `slot_wise` array will always have 96 values. If the device is offline for the complete slot duration, than for that index in the slot\_wise array, the power usage value will be `0`.

