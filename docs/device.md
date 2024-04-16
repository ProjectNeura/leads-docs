# Devices and Controllers

:::{tip}

```python
from leads import *
```

:::

[`Device`](#leads.dt.device.Device) is the base class of every device, from sensors to motors.
[`Controller`](#leads.dt.controller.Controller) is a special type of device that can contain child devices. By
introducing controllers, we have a device tree. The tree always starts from a controller node known as the main
controller.

## Register a Controller

```python
from leads import controller, MAIN_CONTROLLER
from leads_emulation import RandomController


@controller(MAIN_CONTROLLER)
class MainController(RandomController):
    pass
```

The example above registers [`RandomController`](#leads_emulation.RandomController) as the main controller.
[`MAIN_CONTROLLER`](#leads.dt.predefined_tags.MAIN_CONTROLLER) is the tag of the controller. In the device tree, every
device have a tag. You can use [these](#leads.dt.predefined_tags) predefined tags or custom strings.

:::{tip}
All tags should be composed of lowercase letters and underscores only. It is not fatal, but it will make your project
more disciplined.
:::

## Pass Arguments to the Controller

To pass arguments to the device constructor, just include them in the decorator.

```python
from leads import controller, MAIN_CONTROLLER
from leads_emulation import RandomController


@controller(MAIN_CONTROLLER, args=(10, 100), kwargs={"skid_possibility": 0.5})
class MainController(RandomController()):
    pass
```

## Register a Device

As the top node of the device tree is always a controller, all devices must have a parent.

```python
from leads import device, controller, MAIN_CONTROLLER, ODOMETER, Odometer
from leads_emulation import RandomController


@controller(MAIN_CONTROLLER)
class MainController(RandomController()):
    pass


@device(ODOMETER, MAIN_CONTROLLER)
class ChildDevice(Odometer):
    pass
```

## Register a Child Controller

Controllers can be child devices of other controllers as well.

```python
from leads import controller, MAIN_CONTROLLER
from leads_emulation import RandomController, SinController


@controller(MAIN_CONTROLLER, args=(10, 100), kwargs={"skid_possibility": 0.5})
class MainController(RandomController()):
    pass


@controller("secondary_controller", MAIN_CONTROLLER)
class SecondaryController(SinController):
    pass
```