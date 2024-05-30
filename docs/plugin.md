# Context Plugins

A plugin is basically a bunch of predefined callback methods. It implements specific functions by registering a
series of callback methods, such as ESC or GPS speed correction.

## Built-in Plugins

We provide these great plugins:

- [`DTCS`](#leads.plugin.dtcs.DTCS)
- [`ABS`](#leads.plugin.abs.ABS)
- [`EBI`](#leads.plugin.ebi.EBI)
- [`ATBS`](#leads.plugin.atbs.ATBS)
- [`GPSSpeedCorrection`](#leads.plugin.gps_speed_correction.GPSSpeedCorrection) (not ESC)

## ESC Plugins

[`ESCPlugin`](#leads.plugin.plugin.ESCPlugin) is a type of plugin that involves electric stability control. It is just
like regular plugins but will be forcibly disabled when the ESC mode of the context is set to
[`OFF`](#leads.constant.ESCMode.OFF). See [ESC Mode](esc_mode). The 5 built-in plugins listed above are all ESC plugins
except the last one.

## Mount a Plugin

The following example mounts [`DTCS`](#leads.plugin.dtcs.DTCS) to the context.

```python
from leads import LEADS, SystemLiteral, DTCS

context: LEADS = LEADS()
context.plugin(SystemLiteral.DTCS, DTCS())
```

[`SystemLiteral`](#leads.constant.SystemLiteral) contains 4 predefined keys for the 4 ESC plugins.

:::{tip}
The specification stipulates that all plugin keys should consist of capital letters and underscores only.
:::

## Disable a Plugin

In [Mount a Plugin](#mount-a-plugin), we have mounted [`DTCS`](#leads.plugin.dtcs.DTCS) to the context. What if we
want it to be disabled?

```python
from leads import LEADS, SystemLiteral, DTCS

context: LEADS = LEADS()
context.plugin(SystemLiteral.DTCS, DTCS())
context.plugin(SystemLiteral.DTCS).enabled(False)
```