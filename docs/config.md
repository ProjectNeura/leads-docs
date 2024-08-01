# Configuration System

We provide a configuration system to help load local configuration files. We use JSON as the only format. There is a
clear distinction between runtime arguments and configurations. Runtime arguments are usually considered temporary,
meaning they tend to be different every time, while configurations do not often change.

## Load a Configuration File

```python
from leads import ConfigTemplate, load_config

config: ConfigTemplate = load_config("path/to/the/config.json", ConfigTemplate)
```

[`load_config`](#leads.config.registry.load_config) will return the specified type, which is
[`ConfigTemplate`](#leads.config.template.ConfigTemplate) in this case. The configuration object returned is already
[frozen](#readonly-and-writable-configurations).

### Custom Configurations

:::{important}
Custom attribute declarations should always come before calling
[`super().__init__()`](#leads.config.template.ConfigTemplate).
:::

```python
from typing import Any
from leads import ConfigTemplate, load_config


class MyConfig(ConfigTemplate):
    def __init__(self, base: dict[str, Any]) -> None:
        self.custom_config: str = "this is my config"
        super().__init__(base)


config: MyConfig = load_config("path/to/the/config.json", MyConfig)
```

In this example, we declared a new [explicit](#explicit-and-implicit-configurations) configuration `custom_config` with
a default value of `"this is my config"`.

## Readonly and Writable Configurations.

By default, all configurations are readonly, meaning that their values cannot be reassigned once the configuration
object is frozen. If you want them writable, name them after "w_".

```python
from typing import Any
from leads import ConfigTemplate


class MyConfig(ConfigTemplate):
    def __init__(self, base: dict[str, Any]) -> None:
        self.w_writable_config: str = "this is a writable configuration"
        super().__init__(base)
```

:::{tip}
The configuration object will be set to frozen once [`super().__init__()`](#leads.config.template.ConfigTemplate) is
called.
:::

## Explicit and Implicit Configurations

There are two kinds of configurations: explicit and implicit. Explicit configurations refer to the attributes and
implicit configurations are those in the base dictionary.

### Set an Explicit Configuration

```python
from leads import ConfigTemplate

config: ConfigTemplate = ConfigTemplate({})
config.w_debug_level = "INFO"
```

### Get an Explicit Configuration

```python
from leads import ConfigTemplate, L

config: ConfigTemplate = ConfigTemplate({})
L.info(config.w_debug_level)
```

### Set an Implicit Configuration

```python
from leads import ConfigTemplate

config: ConfigTemplate = ConfigTemplate({})
config.set("w_debug_level", "INFO")
# or
config["w_debug_level"] = "INFO"
```

:::{important}
Setting an implicit configuration will also affect the corresponding explicit configuration if there is one. For
example, in the following case prints "INFO".

```python
from leads import ConfigTemplate, L

config: ConfigTemplate = ConfigTemplate({})
config.set("w_debug_level", "INFO")
L.info(config.w_debug_level)
```

:::

### Get an Implicit Configuration

```python
from leads import ConfigTemplate, L

config: ConfigTemplate = ConfigTemplate({})
L.info(config.get("w_debug_level", "default"))
# or
L.info(config["w_debug_level"])
```

### Sync Implicit Configurations to Explicit Configurations

:::{tip}
Modifying the implicit configurations will not affect the explicit configurations. However,
[`load()`](#leads.config.template.ConfigTemplate.load) does call
[`refresh()`](#leads.config.template.ConfigTemplate.refresh) for you.
:::

```python
from leads import ConfigTemplate, L

config: ConfigTemplate = ConfigTemplate({})
config.set("w_debug_level", "INFO")
L.info(config.w_debug_level)
config.refresh()
L.info(config.w_debug_level)
```

```shell
DEBUG
INFO
```

## Register the Configuration Object

Similar to [context](register_the_context), you will have to register the configuration object so that the components
of the framework that work in the background can successfully acquire it.

:::{important}
You can only register once.
:::

```python
from leads import ConfigTemplate, load_config, register_config

register_config(load_config("path/to/the/config.json", ConfigTemplate))
```

## Get the Registered Configuration Object

In [Register the Configuration Object](#register-the-configuration-object), we have registered the context. Now let's
see the getter method.

```python
from leads import ConfigTemplate, get_config

config: ConfigTemplate | None = get_config()
```

This will return `None` if there is no configuration object registered. You can also require a nonnull configuration
object by calling [`require_config`](#leads.config.registry.require_config).

```python
from leads import ConfigTemplate, require_config

config: ConfigTemplate = require_config()
```