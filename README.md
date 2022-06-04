# File Integrity Monitoring

## Overview

Node-M2M has a built-in file integrity monitoring (FIM) feature. 

When enabled, your main application and system files from **m2m** modules in any of your remote devices and clients will be monitored in real-time for any unauthorized changes. 

There are two methods you can take advantage of this feature. 

1. Let Node-M2M manage the file monitoring for you.

2. Manage the file monitoring yourself which provides you more flexibility.

<br>

### 1. Let Node-M2M manage the file monitoring.

<br>

### Procedure

<br>

#### 1. Set the file or directory you want to monitor for changes.

```js
'use strict';

const m2m = require('m2m');

let device = new m2m.Device(100);

device.connect(() => {

  // set the file you want to monitor
  // we will monitor 'test' for unauthorized changes
  device.monFile('./test', (result) => console.log(result));

});
```

Start your application.

```js
$ node device.js
```

<br>

#### 2. Login to Node-M2M website and enable FIM

<br>

Before you enable FIM, you can test it first by accessing your device or client from the main menu. From the *Manage Application* section, click *Enable FIM* button. Select *email alert* and *active response*.

Now, try editing the file you want to manage from your device or client. 

You should see an alert message within the *Manage Application* section on your browser in real-time. You should also receive an email alert from your account email. All these indicate that FIM is working.

If you enabled active response, your remote client or device will be disabled. Once disabled, it will be inaccessible.

To permanently enable FIM, just navigate to the *Manage Security* section on the main menu and select FIM feature.

<br>

### 2. Self-managed file monitoring.

<br>

In this example, we will monitor a file from a *remote device* using a *client*.

<br>

### Device setup

<br>

#### 1. Set the file or directory you want to monitor for changes.

Save the code below as device.js.
```js
'use strict';

const m2m = require('m2m');

let server = new m2m.Device(100);

server.connect(() => {
  device.setData('file-watching', (data) => {
    // set the file you want to monitor
    // we will monitor 'myFile.txt' for unauthorized changes
    let v = device.monFileSync('myFile.txt');
    data.send(v);
  });
});
```

Start your device application.

```js
$ node device.js
```

<br>

### Client setup

<br>

#### 1. Watch the data from the remote device using a channel api.

Save the code below as client.js.
```js
'use strict';

const m2m = require('m2m');

let client = new m2m.Client();

server.connect(() => {
  // watch data using the regular .watchData() method.
  client.watchData({id:100, channel:'file-watching'}, (data) => {
    console.log('file-watching', data); // file-watching 1654331542210 myFile.txt
    // perform your action here
  });
});
```

Start your client application.

```js
$ node client.js
```

Everytime someone attempts to change the file 'myFile.txt' from the remote device, the client will receive an alert in real-time. 

You can also enable FIM and at the same time perform any custom actions you may wish to further protect your system. The combination of these two methods provides additional protection on your systems making it more resilient from any malicious attacks. 

