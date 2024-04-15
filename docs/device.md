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