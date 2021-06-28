# API-Keys

There are two types of API Keys - `TEST` and `LIVE`

## API Key Types

### Test API Keys

`TEST` can be used for development phase and can **only** interact with [Tester User](../users/user-tester.md) Accounts and [Devices](../devices/device.md) that have been added to your [App](app.md)

### Live API Keys

Coming soon... they will allow you to interact with any Smitch [User](../users/user.md) once they install your plugin/app

## API Key Scopes

Each API Key can have different scopes \(or permission\) and can be used to only access/change data related to those scopes

## Rate Limits

An [App](app.md) will have a global rate-limit to `200` Requests Per Minute \(subject to change\), which will be shared between all the API Keys linked to that app.  
If your App has hit our rate limit, all API requests will have HTTP `429` response code.

> If you need a rate limit increase, please reach out to us via this [form](https://forms.gle/dksRf2AThBBaU9Gn9)

## Troubleshooting

### Rate Limited - 429 Response Status Code

`429` status code in HTTP response is present, when the app has triggered the rate-limit. In this case the developer doesn't have to do anything as the backend API servers will start accepting requests after a minute.

However, to avoid getting `429` responses, you can try out the following methods: 1. Start caching requests on your end \(basically don't send the same request multiple times within the same minute\) 2. Listen to User and Device Events via use of [Webhooks](../webhook/webhook.md), instead of continuously polling for updates

### Unauthorized - 401 Response Status Code

`401` status code in the HTTP response is present, when the api keys used is not valid.

* Please recheck that the API key prefix is correct, you can find it in the developer portal app settings
* Generate a new API key and use that instead

