# Mi Home Devices Library

Control Mi Home devices via local network or cloud.

## Supported devices

See [DEVICES.md](DEVICES.md) for full list of supported devices.

## Installation

```sh
npm install mi-home-control
```

## Usage

```javascript
const mihome = require('mi-home-control');
```

Some devices aren't supported over local protocol (miIO) so you need to use cloud protocol (MIoT) to control those devices. User and password for [Mi Account](https://account.xiaomi.com/) is required to use cloud protocol.

### Init protocols

```javascript
// local miIO
mihome.miioProtocol.init();

// local Aqara (ZigBee)
mihome.aqaraProtocol.init();

// cloud MIoT
const username = 'email@example.com';
const password = 'password';
await mihome.miCloudProtocol.login(username, password);
```

### Example cloud methods

```javascript
const options = { country: 'us' }; // 'ru', 'us', 'tw', 'sg', 'cn', 'de' (Default: 'cn')
await mihome.miCloudProtocol.getDevices(null, options); // return all devices from your acount with all information (deviceId, token, model ...) to create device in the next step
await mihome.miCloudProtocol.getDevices([deviceId1, deviceId2, ...], options); // get devices information from list ids
await mihome.miCloudProtocol.miioCall(deviceId, method, params, options); // call miio method with params via cloud protocol
```

### Create device

```javascript
const device = mihome.device({
  id: '100000', // required, device id
  model: 'zhimi.aircondition.v1', // required, device model

  address: '192.168.31.13', // miio-device option, local ip address
  token: 'abcdefgfabcdefgfabcdefgfabcdefgf', // miio-device option, device token
  refresh: 30000 // miio-device option, device properties refresh interval in ms
  
  parent: '1234abcd', // aqara-device option, gateway SID for aqara-protocol compatible device
});
device.on('properties', (data) => {
  console.log(data);
});
await device.setPower(true); // call the method
await device.init(); // connect to device and poll for properties
device.destroy();
```

List of properties and methods for specific device can be found at ./devices folder.
