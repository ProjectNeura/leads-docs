# Communication System

In LEADS, we provide a C/S communication system that is designed for TCP/IP protocol but can also be extended to other
custom protocols. The philosophy of this system is that sending is the only active operation for both server and
client. All other operations including receiving must be done through callback methods.

## Server

A [`Server`](#leads.comm.server.server.Server) is a pool that consists of multiple connections.

### Create a Server

You should always use [`create_server()`](#leads.comm.server.create_server) instead of the constructor.

```python
from leads.comm import create_server, Server

port: int = 9000
server: Server = create_server(port)
```

This example creates a server listening on port `9000` (`16900` by default). The port is not yet occupied as the server
has not been started.

### Assign Callback Methods

You can pass a [`Callback`](#leads.comm.prototype.Callback) object when creating the server.

```python
from typing import override
from leads import L
from leads.comm import create_server, Server, Callback, Service, ConnectionBase


class MyCallback(Callback):
    @override
    def on_initialize(self, service: Service) -> None:
        L.info("Initialized")

    @override
    def on_connect(self, service: Service, connection: ConnectionBase) -> None:
        L.info("Connected")

    @override
    def on_receive(self, service: Service, msg: bytes) -> None:
        L.info(msg.decode())

    @override
    def on_fail(self, service: Service, error: Exception) -> None:
        L.error(repr(error))

    @override
    def on_disconnect(self, service: Service, connection: ConnectionBase) -> None:
        L.info("Disconnected")


server: Server = create_server(callback=MyCallback())
```

### Start the Server

```python
from leads.comm import create_server, Server, start_server

parallel: bool = True
server: Server = create_server()
start_server(server, parallel)
```

`parrallel` is `False` by default, meaning that it will run in the same thread in which you call
[`start_server()`](#leads.comm.server.start_server). If you wish not to block the main thread, set `parallel`
to `True` like the example above.

### Broadcast to Clients

To send a message to all clients, you can use [`broadcast()`](#leads.comm.server.server.Server.broadcast) method.

```python
from leads.comm import create_server, Server, start_server

server: Server = start_server(create_server())
server.broadcast(b"Hello world")
```

### Get the Number of Connections

```python
from leads import L
from leads.comm import create_server, Server, start_server

server: Server = start_server(create_server())
L.info(str(server.num_connections()))
```

### Close the Server

```python
from leads.comm import create_server, Server, start_server

server: Server = start_server(create_server())
server.close()
```

## Client

Unlike a server, a [`Client`](#leads.comm.client.client.Client) holds only one connection.

### Create a Client

You should always use [`create_client()`](#leads.comm.client.create_client) instead of the constructor.

```python
from leads.comm import create_client, Client

port: int = 9000
client: Client = create_client(port)
```

You can use the same callback methods as the [server](#assign-callback-methods).

```python
from typing import override
from leads import L
from leads.comm import create_client, Client, Callback, Service, ConnectionBase


class MyCallback(Callback):
    @override
    def on_initialize(self, service: Service) -> None:
        L.info("Initialized")

    @override
    def on_connect(self, service: Service, connection: ConnectionBase) -> None:
        L.info("Connected")

    @override
    def on_receive(self, service: Service, msg: bytes) -> None:
        L.info(msg.decode())

    @override
    def on_fail(self, service: Service, error: Exception) -> None:
        L.error(repr(error))

    @override
    def on_disconnect(self, service: Service, connection: ConnectionBase) -> None:
        L.info("Disconnected")


client: Client = create_client(callback=MyCallback())
```

### Start the Client

```python
from leads.comm import create_client, Client, start_client

parallel: bool = True
address: str = "127.0.0.1"
client: Client = create_client()
start_client(address, client, parallel)
```

`address` can be a domain or an IP address.

`parrallel` is `False` by default, meaning that it will run in the same thread in which you call
[`start_server()`](#leads.comm.server.start_server). If you wish not to block the main thread, set `parallel`
to `True` like the example above.

### Send a Message

```python
from leads.comm import create_client, Client, start_client

address: str = "127.0.0.1"
client: Client = start_client(address, create_client())
client.send(b"Hello world")
```

### Close the Client

```python
from leads.comm import create_client, Client, start_client

address: str = "127.0.0.1"
client: Client = start_client(address, create_client())
client.close()
```

## Connection