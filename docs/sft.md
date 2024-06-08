(system_failure_tracer)=

# System Failure Tracer

[`SystemFailureTracer`](#leads.sft.SystemFailureTracer) unites the capture of exceptions that are thrown by various
devices into a central tracer. It is a singleton class with the only instance [`SFT`](#leads.sft.SFT). It should be used
only inside devices.

The errors are classified into systems. The systems work as identifiers of the errors. The tracer mainly aims to solve
the inconvenience of determining the status (working or failed) of systems that each consists of multiple devices that
may even overlap.

## Assign Callback Methods

To properly handle the errors, we provide two callback methods: [`on_fail()`](#leads.sft.SystemFailureTracer.on_fail)
and [`on_recover()`](#leads.sft.SystemFailureTracer.on_recover). Neither of them supports chain call. You can assign
them simply by `=`.

```python
from leads import L, SFT, Device, SuspensionEvent


def on_fail(device: Device, event: SuspensionEvent) -> None:
    L.info(f"Device {device.tag()} caused {event.system} to be suspended: {event}")


def on_recover(device: Device, event: SuspensionEvent) -> None:
    L.info(f"Device {device.tag()} caused {event.system} to be recovered: {event}")


SFT.on_fail = on_fail
SFT.on_recover = on_recover
```

## Mark the Device

To use the tracer in a device, the device must be marked first. One device can be assigned to one primary system and
multiple related systems. There is no difference between the two types of systems. The divergence is just a historical
issue. All systems will be equally notified.