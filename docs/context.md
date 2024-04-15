# Context and Events

:::{tip}

```python
from leads import *
```

:::

## Create a Context

```python
from leads import LEADS

context: LEADS = LEADS(initial_data=None,
                       data_seq_size=100,
                       num_laps_timed=3)
```

[`Context`](#leads.context.Context) is where the magic happens. It is the representation of the vehicle in code.

[`LEADS`](#leads.leads.LEADS) is a event-oriented implementation of [`Context`](#leads.context.Context) we provide.

There are 2 phases in a context: push and update. The push phase is when the raw data are inputted. On the other hand,
the update phase is when the data is parsed and affects the context, having been pre-processed.

## Push to the Context

A data container is a collection of data that is fed into the context. We provide a base class:
[`DataContainer`](#leads.data.DataContainer). You can have your own implementation of
[`DataContainer`](#leads.data.DataContainer) where you can add more attributes.

:::{important}
Note that each time you push a data container to the context, it has to be either the same type or inherited from the 
type of the last pushed data container.
:::

```python
from leads import LEADS, DataContainer

context: LEADS = LEADS()
data: DataContainer = DataContainer()  # fill in your data
context.push(data)
```

## Update the Context

After the data is pushed to the context, it will not automatically update, you must manually notify the context to
update.

```python
from leads import LEADS, DataContainer

context: LEADS = LEADS()
data: DataContainer = DataContainer()  # fill in your data
context.push(data)
context.update()
```

## Control the Context using Events

[`EventListener`](#leads.event.EventListener) is the collection of callbacks for context. The following example logs
"Updating..." when [`update()`](#leads.leads.LEADS.update) is called.

```python
from typing import override
from leads import LEADS, EventListener, UpdateEvent, L

class MyEventListener(EventListener):
    @override
    def on_update(self, event: UpdateEvent) -> None:
        L.info("Updating...")

context: LEADS = LEADS()
context.set_event_listener(MyEventListener())
```

There are many methods in [`EventListener`](#leads.event.EventListener). We will use these methods to refer to the
context life cycle.
