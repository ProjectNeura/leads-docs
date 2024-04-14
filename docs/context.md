# Context

:::{note}
```python
from leads import *
```
:::

This is where the magic happens. It is the representation of the vehicle in code.

## Create a [`Context`](#leads.Context)

We provide an event-oriented child implementation of [`Context`](#leads.Context): [`LEADS`](#leads.LEADS)

```python
from leads import LEADS


context: LEADS = LEADS(initial_data=None,
                       data_seq_size=100,
                       num_laps_timed=3)
```