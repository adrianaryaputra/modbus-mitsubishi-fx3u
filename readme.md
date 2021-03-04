# Node.js Modbus Communication for Mitsubishi FX3U PLC
this is a [com-modbus](https://github.com/adrianaryaputra/com-modbus)'s specific device implementation for Mitsubishi FX3U PLC. only `M` and `D` address tested.

## 🧑‍🔧 Installation
`npm i --save git://adrianaryaputra/modbus-mitsubishi-fx3u.git`

## 📖 Usage
```js
const { 
    ModbusHandler, 
    ModbusDevice_FX3U, 
    SerialPort,
} = require('modbus-mitsubishi-fx3u');


// set serial port
const port = new SerialPort('/dev/tty-usbserial1', {
  baudRate: 57600
});


// set modbus handler and device
let modbusHandler = new ModbusHandler({
    msgSendInterval: 100,
    timeout: 100,
    retryCount: 10,
    chunkSizeWord: 4,
});

let fx3u = new ModbusDevice_FX3U({
    modbusHandler,
    modbusId: 1,
    modbusTimeout: 100,
});


// set modbus handler's serial port connection 
modbusHandler.setConnection(port).open();


// read PLC memory: M30 - M46
fx3u.read({
    type: fx3u.type.M, 
    address: 30,
    length: 2, // byte
    priority: 2, // lower have higher priority
    callback: (error, success) => console.log(error, success)
});

// write value: 1234 to PLC memory: D10
fx3u.write({
    type: fx3u.type.D, 
    address: 10,
    value: 1234,
    priority: 1, // lower have higher priority
    callback: (error, success) => console.log(error, success)
});

// pulse a single bit (turn it on then off after certain duration)
fx3u.pulse({
    type: fx3u.type.M, 
    address: 5,
    duration: 200, // ms
    priority: 2, // lower have higher priority
    callback: (error, success) => console.log(error, success)
});
```