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

**Sensor** Concepts are used to define what a *Sensor* compliant object looks like and provide a means to templating against a *Sensor* interface. The two required methods are:

**bool Init()** This method should establish communication with the sensor, configure the sensor, and zero biases. True is returned on successfully initializing the sensor.

**bool Read()** This method should get new data from the sensor. True is returned if new data is received.