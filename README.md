# ModbusRTU and ModbusIP Master-Slave Library for ESP8266/ESP32 v3.0

|If this project is helpful for your projects you can support it by a glass of beer|[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Z38SLGAKGM93S&source=url)|
|---|---|


**Release state: DEVELOPMENT**

Visit [Releases](https://github.com/emelianov/modbus-esp8266/releases) page for stable one.

---

This library allows your ESP8266/ESP32 to communicate via Modbus protocol. The Modbus is a master-slave protocol
used in industrial automation and can be used in other areas, such as home automation.

The Modbus generally uses serial RS-232 or RS-485 as physical layer (then called Modbus Serial) and TCP/IP via Ethernet or WiFi (Modbus IP).

In the current version the library allows the ESP8266/ESP32 operate as a master and/or slave, supporting Modbus IP via wireless network and Modbus RTU over serial. For more information about Modbus see:

* [Modbus (From Wikipedia, the free encyclopedia)](http://pt.wikipedia.org/wiki/Modbus)
* [MODBUS APPLICATION PROTOCOL SPECIFICATION
V1.1b](http://www.modbus.org/docs/Modbus_Application_Protocol_V1_1b.pdf)
* [MODBUS MESSAGING ON TCP/IP IMPLEMENTATION GUIDE
V1.0b](http://www.modbus.org/docs/Modbus_Messaging_Implementation_Guide_V1_0b.pdf)
* [MODBUS over Serial Line
Specification and Implementation Guide
V1.02](http://www.modbus.org/docs/Modbus_over_serial_line_V1_02.pdf)

## Features

* Supported platforms are
  * ESP8266
  * ESP32
* Operates as
  * slave
  * master
* Supports
  * Modbus IP (TCP)
  * Modbus RTU (RS-485)
* Reply exception messages for all supported functions
* Modbus functions supported:
  * 0x01 - Read Coils
  * 0x02 - Read Input Status (Read Discrete Inputs)
  * 0x03 - Read Holding Registers
  * 0x04 - Read Input Registers
  * 0x05 - Write Single Coil
  * 0x06 - Write Single Register
  * 0x0F - Write Multiple Coils
  * 0x10 - Write Multiple Registers
* Callbacks for
  * Master connect (ModbusIP)
  * Master/Slave disconnect (ModbusIP)
  * Read specific Register
  * Write specific Register
  * Slave transaction finish

## Notes

1. The offsets for registers are 0-based. So be careful when setting your supervisory system or your testing software. For example, in [ScadaBR](http://www.scadabr.com.br) offsets are 0-based, then, a register configured as 100 in the library is set to 100 in ScadaBR. On the other hand, in the [CAS Modbus Scanner](http://www.chipkin.com/products/software/modbus-software/cas-modbus-scanner/) offsets are 1-based, so a register configured as 100 in library should be 101 in this software.
2. For API refer [API.md](https://github.com/emelianov/modbus-esp8266/blob/master/API.md)
3. Modbus RTU maximum incoming frame size is determinated by HardwareSerial buffer size. For SoftwareSerial buffer must be set to 256 bytes.
4. Probably it's possible to use ModbusRTU with other AVR boards using <vector> from [ArduinoSTL](https://github.com/mike-matera/ArduinoSTL).
5. RS-485 transivers based on MAX-485 is working on at least up to 115200. XY-017/XY-485 working only up to 9600 for some reason.

## Last Changes

```diff
// 3.0.0-DEVEL
+ ModbusRTU Slave
+ ModbusRTU Master
+ Registers are now shared between Modbus* instances by default
+ Fix functions register count limits to follow Modbus specification (or RX buffer limitations)
+ ModbusRTU examples added
+ CRC tables stored in PROGMEM
+ TX control pin support to work with MAX-485
+ Test multiple Modbus* instances
+ Change to 'uint32_t eventSource()'. Implemented for ModbusRTU and ModbusIP both.
+ Documentation changes
+ Allow to specify local TCP port for Slave (default is 502)
+ Allow to specify TCP port for remote Slave connection (default is 502)
+ Master\Client. Fix crash on Write Multiple Hregs
+ Master\Client. Fix crash on no callback function on read\write remote.
- Add tests.
// ToDo later
- 0x14 - Read File Records function
- 0x15 - Write File Records function
- 0x16 - Write Mask Register function
- 0x17 - Read/Write Registers function
// 2.1.0
+ Slave. Fix error response on write multiple Hregs\Coils
+ Slave. Fix writeCoil() for multiple coils
+ Master. dropTransactions()
+ Master. disconnect()
+ ~ModbusIP()
+ task() cleanup
+ Modify examples
+ Slave. Allow only single incoming master connection per IP
// 2.0.1
+ Master. Fix readCoil\Hreg\Ists\Ireg not read value from slave
+ Fix crash on disconnect with Arduino Core 2.5.x
// 2.0.0
+ Remove memory allocation checking for small blocks as anyway firmware will fail if so low memory available.
+ Change object's list implementation to *std::vector*
+ Modbus class refactoring
+ ModbusIP networking code refactoring and error reporting
+ Global registers storage to share between multiple Modbus* instances
+ Move rest of implementations from Modbus.h
+ Modbus master implementation
+ Move enum constants. E.g. MB_FC_READ_COIL => Modbus::FC_READ_COIL
+ Back to marking private for onSet, onGet, addReg and Reg methods
+ Added callback-related eventSource method, onDisconnect and transaction result callbacks
+ Extend register addressing to 0..65535
+ removeCoil, removeIsts, removeIreg, removeHreg, (removeReg)
+ readCoil, readHreg, readIsts, readIreg
+ push\pullCoil, push\pullHreg, pullIsts, pullIreg
+ pullCoilToIsts, pullHregToIreg, pushIstsToCoil, pushIregToHreg
+ optimize code around std::vector processing
+ extend removeCoil/Hreg/... to remove multiple registers
+ multiple callbacks => memory usage optimization
+ added removeOnSetCoil\... methods
+ added read/write/push/pullCoil/Hreg/Ireg/Ists() parameter to specify Modbus unit id
+ added ability to auto connect to slave. Setting is global. Disabled by default.
```

## Contributions

https://github.com/emelianov/modbus-esp8266

a.m.emelianov@gmail.com

Original version:

https://github.com/andresarmento/modbus-esp8266

https://github.com/andresarmento/modbus-arduino

prof (at) andresarmento (dot) com

## License

The code in this repo is licensed under the BSD New License. See LICENSE.txt for more info.
