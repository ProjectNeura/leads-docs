# Appendix

This appendix keeps track some tech specs of the critical electronics in subsystems. If a new variant that is not listed
below gets adopted, please inform the VeC Steering Council and let us update this page.

To fully understand how these components work, you are encouraged to learn some Simplified Chinese because nearly all of
them are made or assembled in RPC, thus a lot of documents are only available in Simplified Chinese. Being able to
switch between English and Simplified Chinese would be extremely helpful if you are pursuing a career in Electrical
Engineering.

Last updated: 2025-02-24

(leads_vec_appendix_arduino)=

## Arduino

### Arduino Micro

| K                        | V                                                 |
|--------------------------|---------------------------------------------------|
| Vendor                   | Multiple                                          |
| Specs                    | [Webpage](https://docs.arduino.cc/hardware/micro) |
| Corresponding Vehicle(s) | VeC99, VeC 2.0                                    |
| Installed                | VeC99, VeC 2.0                                    |

### Arduino Nano

| K                        | V                                                |
|--------------------------|--------------------------------------------------|
| Vendor                   | Multiple                                         |
| Specs                    | [Webpage](https://docs.arduino.cc/hardware/nano) |
| Corresponding Vehicle(s) | VeC99                                            |
| Installed                | None                                             |

(leads_vec_appendix_power)=

## Power

(leads_vec_appendix_power_power_supply_module)=

### Power Supply Module

#### MB102 Black Breadboard Power Supply

| K                        | V                                                                                                                                                            |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vendor                   | [Sayal Electronics](https://secure.sayal.com/STORE4/prodetails.php?SKU=257029)                                                                               |
| Specs                    | [AMS1117_datasheet.pdf](../_static/AMS1117_datasheet.pdf), [YwRobot_Breadboard_Power_Supply_MB-V2.pdf](../_static/YwRobot_Breadboard_Power_Supply_MB-V2.pdf) |
| Corresponding Vehicle(s) | VeC99, VeC 2.0                                                                                                                                               |
| Installed                | VeC99                                                                                                                                                        |

(leads_vec_appendix_power_voltage_sensor)=

### Voltage Sensor

#### OSEPP Voltage Sensor Module

| K                        | V                                                                              |
|--------------------------|--------------------------------------------------------------------------------|
| Vendor                   | [Sayal Electronics](https://secure.sayal.com/STORE4/prodetails.php?SKU=246716) |
| Specs                    | [osepp_volt01.pdf](../_static/osepp_volt01.pdf)                                |
| Corresponding Vehicle(s) | VeC99, VeC 2.0                                                                 |
| Installed                | VeC99                                                                          |
| Operating Voltage Output | 3.3V - 5V MAX                                                                  |
| Input Voltage Range      | 0.0245V ~ 25V MAX                                                              |

(leads_vec_appendix_wheel_speed_controller)=

## Wheel Speed Controller

(leads_vec_appendix_wheel_speed_controller_wheel_speed_sensor)=

### Wheel Speed Sensor

Wheel speed sensors are Hall sensors that have digital outputs, also known as Hall switches. You can find a Physics
explanation [here](../_static/Introduction%20and%20Application%20of%20Hall%20Devices.pdf).

#### Velleman Hall Magnetic Switch Module

| K                        | V                                                                              |
|--------------------------|--------------------------------------------------------------------------------|
| Vendor                   | [Sayal Electronics](https://secure.sayal.com/STORE4/prodetails.php?SKU=248124) |
| Specs                    | [vma313_a4v01.pdf](../_static/vma313_a4v01.pdf)                                |
| Corresponding Vehicle(s) | VeC99                                                                          |
| Installed                | VeC99                                                                          |

#### ES3144

| K                        | V                                         |
|--------------------------|-------------------------------------------|
| Vendor                   | Taobao (AliExpress)                       |
| Specs                    | [ES3144_en.pdf](../_static/ES3144_en.pdf) |
| Corresponding Vehicle(s) | VeC 2.0                                   |
| Installed                | None                                      |