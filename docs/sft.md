(system_failure_tracer)=

# System Failure Tracer

[`SystemFailureTracer`](#leads.sft.SystemFailureTracer) unites the capture of exceptions that are thrown by various
devices into a central tracer. It is a singleton class with the only instance [`SFT`](#leads.sft.SFT). It should be used
only inside devices.

The errors are classified into systems. The systems work as identifiers of the errors. The tracer mainly aims to solve
the inconvenience of determining the status (working or failed) of systems that each consists of multiple devices that
may even overlap.

## Assign Callback Methods

To properly handle the errors, we provide 4 callback methods: [`on_fail()`](#leads.sft.SystemFailureTracer.on_fail),
[`on_recover()`](#leads.sft.SystemFailureTracer.on_recover),
[`on_device_fail()`](#leads.sft.SystemFailureTracer.on_device_fail), and
[`on_device_recover()`](#leads.sft.SystemFailureTracer.on_device_recover). Neither of them supports chain call. You can
assign them simply by `=`.

```python
from leads import L, SFT, Device, SuspensionEvent


def on_fail(event: SuspensionEvent) -> None:
    L.info(f"System {event.system} raised an error: {event.cause}")


def on_recover(event: SuspensionEvent) -> None:
    L.info(f"System {event.system} recovered")


def on_device_fail(device: Device, error: str | Exception) -> None:
    L.info(f"Device {device} raised an error: {error}")


def on_device_recover(device: Device) -> None:
    L.info(f"Device {device} recovered")


SFT.on_fail = on_fail
SFT.on_recover = on_recover
SFT.on_device_fail = on_device_fail
SFT.on_device_recover = on_device_recover
```

## Mark the Device

To use the tracer in a device, the device must be marked first. One device can be assigned to one primary system and
multiple related systems. There is no difference between the two types of systems. The divergence is just a historical
issue. All systems will be equally notified.

:::{tip}
It is recommended to mark the device in
[`initialize()`](#leads.dt.device.Device.initialize).
:::

```python
from typing import override
from leads import controller, MAIN_CONTROLLER, mark_device
from leads_emulation import RandomController


@controller(MAIN_CONTROLLER)
class MainController(RandomController):
    @override
    def initialize(self, *parent_tags: str) -> None:
        mark_device(self, "SYSTEM_A", "SYSTEM_B", "SYSTEM_C")
        super().initialize(*parent_tags)
```