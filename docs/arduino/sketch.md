# Sketch with LEADS Arduino

On this page, you will learn how to create a sketch with our framework.

## Peer

In LEADS Arduino, `Peer` represents the host that is connected through the hardware serial. You should not operate with
`Serial` yourself because `Peer` does it for you.

```cpp
#include "LEADS.h"

Peer P{};

void setup() {
    P.initializeAsRoot();
}

void loop() {
    P.write("Hello World");
}
```

### Customize the Baud Rate

You may specify the baud rate just like `Serial.begin()`.

```cpp
#include "LEADS.h"

Peer P{115200};

void setup() {
    P.initializeAsRoot();
}

void loop() {
    P.write("Hello World");
}
```

### Customize the Separator

You may specify the separator that is used to split the stream into messages.

```cpp
#include "LEADS.h"

Peer P{9600, "end;"};

void setup() {
    P.initializeAsRoot();
}

void loop() {
    P.write("Hello World");
}
```

## Example Sketch

Please find the sketches used in LEADS VeC [here](https://github.com/ProjectNeura/LEADS/tree/main/arduino).

The following example utilizes a voltage sensor.

```cpp
#include "LEADS.h"

const int PIN_VOT[] = {A0};

Peer P{};
VoltageSensor VOT{30000.0, 7500.0, ArrayList<int>(PIN_VOT)};

void setup() {
    P.initializeAsRoot();
    VOT.initializeAsRoot();
}

void loop() {
    returnFloat(P, VOLTAGE_SENSOR, VOT.read());
    delay(100);
}
```

## Cooperate on the Host's End with Python

In the example above, we have created a sketch for the Arduino. Now we extend this example and receive the readings on
the host's end.

[`ArduinoProto`](#leads_arduino.arduino_proto.ArduinoProto) standardizes the communication between the Arduino and the
host.

```python
from typing import override

from leads import controller, MAIN_CONTROLLER, require_config, VOLTAGE_SENSOR, device
from leads_arduino import ArduinoProto, VoltageSensor
from leads_gui import Config

config: Config = require_config()
BAUD_RATE: int = config.get("baud_rate", 9600)
POWER_CONTROLLER_PORT: str = config.get("power_controller_port", "COM3")


@controller("pc", MAIN_CONTROLLER, (POWER_CONTROLLER_PORT, BAUD_RATE))
class PowerController(ArduinoProto):
    @override
    def read(self) -> dict[str, float]:
        return {"voltage": self.device(VOLTAGE_SENSOR).read()}


@device(VOLTAGE_SENSOR, "pc")
class BatteryVoltageSensor(VoltageSensor):
    pass
```

What it does is updating every child device with incoming messages. Each child device may choose to accept or not to
accept the update. This determination is usually done by tag identifiers which are specified in `returnFloat()` in the
example above.