# Communication System

In LEADS, we provide a C/S communication system that is designed for the TCP protocol but can also be extended to other
custom protocols. The philosophy of this system is that sending is the only active operation for both the server and the
client. All other operations including receiving should be done passively through callback methods.

## Server

A [`Server`](#leads.comm.server.server.Server) is a pool that consists of multiple connections. Each connection runs
in a separate thread. You can specify to run the listener in the main thread or another thread when
[starting the server](#start-the-server).

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

:::{tip}
This is a subclass of [`CallbackChain`](#leads.callback.CallbackCahin). Learn more about it [here](callback).
:::

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
[`start_server()`](#leads.comm.server.start_server). If you wish not to block the main thread, set `parallel` to `True`
like the example above.

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

Unlike [`Server`](#leads.comm.server.server.Server), a [`Client`](#leads.comm.client.client.Client) holds only one
connection.

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
[`start_client()`](#leads.comm.client.start_client). If you wish not to block the main thread, set `parallel` to `True`
like the example above.

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

[`ConnectionBase`](#leads.comm.prototype.ConnectionBase) is the abstract base class of
[`Connection`](#leads.comm.prototype.Connection). [`ConnectionBase`](#leads.comm.prototype.ConnectionBase) provides
a set of tools to parse the stream into sentences and deal with the remainder with ease, whereas
[`Connection`](#leads.comm.prototype.Connection) implements the abstract methods and specifies TCP as the underlying
protocol.

The fundamental of the system is built on [`ConnectionBase`](#leads.comm.prototype.ConnectionBase), not
[`Connection`](#leads.comm.prototype.Connection). Therefore, you can use any implementation of
[`ConnectionBase`](#leads.comm.prototype.ConnectionBase) to suit other protocols. Our serial communication system is a
perfect example. See [`SerialConnection`](#leads_comm_serial.connection.SerialConnection).

:::{important}
[`Server`](#leads.comm.server.server.Server) and [`Client`](#leads.comm.client.client.Client) do not support custom
connections. They are hardcoded to use [`Connection`](#leads.comm.prototype.Connection). If you have a custom
implementation, you need to build your own server or client from [`Entity`](#leads.comm.prototype.Entity) like
[`ArduinoProto`](#leads_arduino.arduino_proto.ArduinoProto) in our case.
:::