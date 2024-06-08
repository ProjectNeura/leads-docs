# Communication System

In LEADS, we provide a C/S communication system that is designed for TCP/IP protocol but can also be extended to other
custom protocols.

## Server

A [`Server`](#leads.comm.server.server.Server) is a pool that consists of multiple connections.

### Create a Server

You should always use [`create_server()`](#leads.comm.server.__init__.create_server) instead of the constructor.

```python
from leads.comm import create_server, Server

port: int = 9000
server: Server = create_server(port)
```

This example creates a server listening on port `9000` (`16900` by default). The port is not yet occupied as the server
has not been started.

### Assign Callback Methods

You can pass a [`Callback`](#leads.comm.prototype.Callback) object when creating the server.

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

### Start the Server

```python
from typing import override
from leads import L
from leads.comm import create_server, Server, Callback, Service, start_server


class MyCallback(Callback):
    @override
    def on_receive(self, service: Service, msg: bytes) -> None:
        L.info(msg.decode())


port: int = 9000
parallel: bool = False
server: Server = create_server(port, MyCallback())
start_server(server, parallel)
```

`parrallel` is `False` by default, meaning that it will run in the same thread in which you call
[`start_server()`](#leads.comm.server.__init__.start_server). If you wish not to block the main thread, set `parallel`
to `True`.

### Broadcast to Clients

To send a message to all clients, you can use [`broadcast()`](#leads.comm.server.server.Server.broadcast) method.

## Client

## Connection