# Client

LEADS implements a network communication system based on TCP.

:::{seealso}
These codes are in "leads/comm/client".
:::

## Create a Client

:::{important}
You should never manually instantiate a {py:class}`Client`.
:::

```python
from leads.comm import Client, create_client


port: int = 16900

client: Client = create_client(port)
```
