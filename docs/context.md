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

(register_the_context)=

## Register the Context

There are some components of the framework requires the context instance that are running in the background such as the
[System Failure Tracer](system_failure_tracer). To simplify the process, avoiding passing the context to every such
component, we provide a registry system through which you only have to register once.

:::{important}
You can only register once.
:::

```python
from leads import LEADS, register_context

context: LEADS = LEADS()
register_context(context)
```

You can then get it by calling [`get_context`](#leads.registry.get_context).

## Get the Registered Context

In [Register the Context](#register-the-context), we have registered the context. Now let's see the getter method.

```python
from leads import LEADS, get_context

context: LEADS | None = get_context()
```

This will return `None` if there is no context registered. You can also require a nonnull context by calling
[`require_context`](#leads.registry.require_context).

```python
from leads import LEADS, require_context

context: LEADS = require_context()
```

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

(esc_mode)=

## ESC Mode

There are four ESC modes: [`STANDARD`](#leads.constant.ESCMode.STANDARD),
[`AGGRESSIVE`](#leads.constant.ESCMode.AGGRESSIVE), [`SPORT`](#leads.constant.ESCMode.SPORT),
[`OFF`](#leads.constant.ESCMode.OFF). They rank from the earliest intervention to no involvement at all. Setting the ESC
mode of the context to [`OFF`](#leads.constant.ESCMode.OFF) will also automatically disable all ESC plugins.

The example below sets the esc mode of the context to [`SPORT`](#leads.constant.ESCMode.SPORT).

```python
from leads import LEADS, ESCMode

context: LEADS = LEADS()
context.esc_mode(ESCMode.SPORT)
```