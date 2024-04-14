# Create a GUI Application

LEADS implements a device tree system for each language variant. The device tree is an abstract layer between hardware
platforms and the controlling program.

:::{note}

```python
from leads_gui import *
```

:::

## Inheritance

`Device` is the prototype of every device including controllers.

`ShadowDevice`, which inherits from `Device`, provides multithread abilities (not included in the C++ version).