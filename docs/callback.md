(callback)=

# Callback

Callback is a philosophy that is commonly used in LEADS. Some come in the form of callback objects, while others come
in the form of single functions. This article only discusses the former, which are more common. If you encounter the
latter, they are usually explained in the specific usage.

## Callback Chain

Registration of a callback is generally not a replacement. A newly registered callback is usually added as a new node to
the callback chain. This callback chain is similar to a linked list.

LEADS implements this fundamental structure as [`CallbackChain`](#leads.callback.CallbackChain). It provides the
capability to be bound to another [`CallbackChain`](#leads.callback.CallbackChain). It should be used as a template
only. The following example defines a specific callback class and a use case.

```python
from leads import CallbackChain


class MyCallback(CallbackChain):
    def on_my_callback_is_called(self, name: str) -> None:
        ...


class MyBusiness(object):
    def __init__(self) -> None:
        self._callback: MyCallback | None = None

    def set_callback(self, callback: MyCallback) -> None:
        callback.bind_chain(self._callback)
        self._callback = callback

    def do_something(self, name: str) -> None:
        if self._callback:
            self._callback.on_my_callback_is_called(name)
```

Now, the user can pass its callback object to the business.

```python
from typing import override
from leads import CallbackChain, L


class MyCallback(CallbackChain):
    def on_my_callback_is_called(self, name: str) -> None:
        ...


class MyBusiness(object):
    def __init__(self) -> None:
        self._callback: MyCallback | None = None

    def set_callback(self, callback: MyCallback) -> None:
        callback.bind_chain(self._callback)
        self._callback = callback

    def do_something(self, name: str) -> None:
        if self._callback:
            self._callback.on_my_callback_is_called(name)


class UserCallbackA(MyCallback):
    @override
    def on_my_callback_is_called(self, name: str) -> None:
        self.super(name=name)
        L.info(f"My callback is called with an argument name={name}")


if __name__ == '__main__':
    my_business = MyBusiness()
    my_business.set_callback(UserCallbackA())
    my_business.do_something("test_case_1")
```

:::{important}
It is advised to always call [`super()`](#leads.callback.CallbackChain.super) unless you want to override all previously
declared methods.
:::

Now suppose another user wants to register another callback object from somewhere else.

```python
from typing import override
from leads import CallbackChain, L


class MyCallback(CallbackChain):
    def on_my_callback_is_called(self, name: str) -> None:
        ...


class MyBusiness(object):
    def __init__(self) -> None:
        self._callback: MyCallback | None = None

    def set_callback(self, callback: MyCallback) -> None:
        callback.bind_chain(self._callback)
        self._callback = callback

    def do_something(self, name: str) -> None:
        if self._callback:
            self._callback.on_my_callback_is_called(name)


class UserCallbackA(MyCallback):
    @override
    def on_my_callback_is_called(self, name: str) -> None:
        self.super(name=name)
        L.info(f"My callback is called with an argument name={name}")


class UserCallbackB(MyCallback):
    @override
    def on_my_callback_is_called(self, name: str) -> None:
        self.super(name=name)
        L.info(f"User B's callback is called with an argument name={name}")


if __name__ == '__main__':
    my_business = MyBusiness()
    my_business.set_callback(UserCallbackA())
    my_business.set_callback(UserCallbackB())
    my_business.do_something("test_case_2")
```

The output should be printing both "My callback is called with an argument name=test_case_2" and "User B's callback is
called with an argument name=test_case_2".

## Existing Callback Classes

The two main callback classes are [`EventListener`](#leads.event.EventListener) and
[`Callback`](#leads.comm.prototype.Callback).