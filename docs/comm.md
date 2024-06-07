# Communication System

In LEADS, we provide a C/S communication system that is designed for TCP/IP protocol but can also be extended to other
custom protocols.

## Server

A [`Server`](#leads.comm.server.server.Server) is a pool that consists of multiple connections.

### Create a Server

:::{tip}
The default port is `16900`.
:::

```python
from leads.comm import create_server, Server

port: int = 9000
server: Server = create_server(port)
```

This example creates a server listening on port `9000`. The port is not yet occupied as the server has not been started.

### Assign Callback Methods

You can pass a [`Callback`](#leads.comm.server.prototype.Callback) object when creating the server.

For example, this code prints every message when it is received.

```python
from typing import override
from leads import L
from leads.comm import create_server, Server, Callback, Service


class MyCallback(Callback):
    @override
    def on_receive(self, service: Service, msg: bytes) -> None:
        L.info(msg.decode())


port: int = 9000
server: Server = create_server(port, MyCallback())
```

### Broadcast to Clients

To send a message to all clients, you can use [`broadcast()`](#leads.comm.server.server.Server.Broadcast) method.

## Client

## Connection