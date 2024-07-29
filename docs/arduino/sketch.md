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