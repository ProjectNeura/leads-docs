# LEADS Arduino

LEADS Arduino is a submodule of LEADS. It is the corresponding firmware library for Arduino boards.

:::{important}
Since version `1.1.0`, we have introduced modern C / C++ workflow to LEADS Arduino with the help of PlatformIO. Earlier
versions are not recommended for bugs and instability.
:::

Install LEADS Arduino through the library manager in the Arduino IDE.

![leads-arduino.png](../_static/leads-arduino.png)

Although we try our best to keep the Python version consistent, the flexibility of Python is something that C / C++
cannot match in any way. We had to compromise with these constraints and ensure functionality through subtle API
differences.