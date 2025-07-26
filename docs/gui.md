# Create a GUI Application

## Create a Pot

A [`Pot`](#leads_gui.prototype.Pot) is the root window of an application.

```python
from leads_gui import RuntimeData, Pot

runtime_data = RuntimeData()
pot = Pot(1080, 720, 30, runtime_data)  # 1080x720 30fps
```