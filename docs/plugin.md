# Context Plugins

:::{tip}

```python
from leads import *
```

:::

A plugin is basically a bunch of predefined callback methods. It implements specific functions by registering a
series of callback methods, such as ESC or GPS speed correction.

## Built-in Plugins

We provide these great plugins:

- [`DTCS`](#leads.plugin.dtcs.DTCS)
- [`ABS`](#leads.plugin.abs.ABS)
- [`EBI`](#leads.plugin.ebi.EBI)
- [`ATBS`](#leads.plugin.atbs.ATBS)
- [`GPSSpeedCorrection`](#leads.plugin.gps_speed_correction.GPSSpeedCorrection)