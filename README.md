# sensor
Defines a common interface for sensors.
   * [License](LICENSE.md)
   * [Changelog](CHANGELOG.md)
   * [Contributing guide](CONTRIBUTING.md)

# Description
This library defines what a *Sensor* interface should look like, enabling higher level code to abstract out sensor specifics and design against this interface.

## Installation
CMake is used to build this library, which is exported as a library target called *sensor*. The header is added as:

```
#include "sensor/sensor.h"
```

The library can be also be compiled stand-alone using the CMake idiom of creating a *build* directory and then, from within that directory issuing:

```
cmake ..
make
```

This will build the library and an example called *example*, which has source code located in *examples/example.cc*.

## Namespace
This library is within the namespace *bfs*.

## Class / Methods

**struct SensorConfig** defines a structure to configure the sensor. The data fields are:

| Name | Description |
| --- | --- |
| int16_t sampling_rate_hz | The sampling rate in Hz. |
| int32_t dev_opt | For I2C communication, this should be the I2C address. For SPI communication, this should be the chip select pin. For serial communication, this is the baud rate. |
| std::variant<TwoWire *, SPIClass *, HardwareSerial *> port | A pointer to the interface to communicate with the sensor |

```C++
/*
* An MPU-9250 IMU on I2C with an address of 0x68
* and a sampling rate of 100 Hz
*/
SensorConfig mpu9250_config = {
  .sampling_rate_hz = 100,
  .dev_opt = 0x68,
  .port = &Wire
};
/*
* A BME-280 pressure transducer on SPI, CS pin 10
* and a sampling rate of 100 Hz
*/
SensorConfig bme280_config = {
  .sampling_rate_hz = 100,
  .dev_opt = 10,
  .port = &SPI
};
/*
* A uBlox GNSS Serial4, baudrate 921600
* and a sampling rate of 10 Hz
*/
SensorConfig gnss_config = {
  .sampling_rate_hz = 10,
  .dev_opt = 921600,
  .port = &Serial4
};

```

**Sensor** Concepts are used to define what a *Sensor* compliant object looks like and provide a means to templating against a *Sensor* interface. The required methods are:

**bool Config(const SensorConfig &ref)** Sets the sensor configuration. Note that this should just set the configuration options in the sensor driver, the configuration is actually applied to the sensor in the *Init* method. Configuration options should be checked for validity. True is returned on receiving a valid configuration, false on an invalid configuration.

**bool Init()** This method should establish communication with the sensor, configure the sensor, and zero biases. True is returned on successfully initializing the sensor.

**bool Read()** This method should get new data from the sensor. True is returned if new data is received.