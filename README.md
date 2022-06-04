# File Integrity Monitoring

## Overview

Node-M2M has a built-in file integrity monitoring (FIM) feature. 

When enabled, your main application and system files from **m2m** modules in any of your remote devices and clients will be monitored in real-time for any unauthorized changes. 

There are two methods you can take advantage of this feature. 

1. Let Node-M2M manage the file monitoring for you.

2. Manage the file monitoring yourself which provides you more flexibility.

<br>

## 1. Let Node-M2M manage the file monitoring.

### Procedure

<br>

### 1. Set the file or directory you want to monitor for changes.

You can monitor any files from any of your clients and devices. 

```js
const m2m = require('m2m');

let device = new m2m.Device(100);

device.connect(() => {

  // set the file you want to monitor
  // monitor 'test' file for unauthorized changes
  device.monFile('./test', (result) => console.log(result));

});
```

Start your application.

```js
$ node device.js
```

<br>

### 2. Login to Node-M2M website and enable FIM

<br>

Before you enable FIM, you can test it first by accessing your device or client where the file will be monitored from the main window when you login. 
 
Once inside the client or device, click *Enable FIM* from the *Manage Application* section.

Select also *email alert* and *active response*. 

<br>

Now, try editing the file you are monitoring from your device or client. In this example make a change on 'test' file from device **100**.

You should see an alert message from the *Manage Application* section on your browser in real-time. You should also receive an email alert from your account email. All these indicate that FIM is working.

If you have enabled active response, your remote client or device will be disabled also for any unauthorized changes. Once disabled, it will be inaccessible from any controlling clients and devices.

Login to your account to re-enable it. 

<br>


To enable FIM, just navigate to the *Manage Security* section on the main menu and select FIM feature.

<br>

## 2. Self-managed file monitoring.

<br>

In this example, we will monitor a file in the *remote device* from a *client* application.

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

