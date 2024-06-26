# Members of the VeC Project

If you are a member of the [VeC Project](https://www.villanovacollege.org/giving/vec-project), you are granted with
access to assets of LEADS VeC (not included as assets of LEADS). Ask your contact for access to the
[LEADS VeC Asset Repository](https://github.com/ProjectNeura/LEADS-VeC-Assets).

Over time, LEADS has grown into a giant project spanning multiple fields. Extensions of LEADS, such as LEADS Jarvis and
Project Thor, are no longer limited to the field of software engineering, but range from data science, deep learning,
artificial intelligence, to physical engineering and electronic engineering. It is not a simple task to fully understand
and be familiar with the entire LEADS. Therefore, LEADS is divided into multiple sub-warehouses according to the fields
involved. Each warehouse has an independent management system, but they are all guided and supervised by the VeC Steering
Council.

## We are Looking for You

We are looking for new members of the VeC Steering Council. To quickly qualify yourself, prepare for these questions.

### Core

1. How many contexts can there be?
    - You can create multiple contexts, yet only 1 can be registered.
2. Describe the life cycle of a context.
    - Pre-push -> Post-push -> Pre-update -> On-update -> Post-update
3. During which periods do plugins come into play?
    - Plugins can affect pre-push, post-push, pre-update, and post-update.
4. Which should be registered first, a context or a configuration?
    - The configuration should always be ready before the instantiation of a context.
5. What is SFT?
    - SFT, which stands for System Failure Tracer, allows different components to throw errors into a unified manager so
      that those error collection components can retrieve these errors.
6. How are devices updated, actively or passively?
    - Regular devices are normally updated actively, meaning that whenever [`read()`](#leads.dt.device.Device.read) is
      called, it will acquire a new value.
    - Shadow devices (devices that run in separate threads) are usually updated passively, meaning that it only
      updates when notified. For example, Arduino boards are updated when a message is received. Every time
      [`read()`](#leads.dt.device.Device.read) is called, it will return that last updated value.

### GUI

1. Does the GUI main loop run in the main thread?
    - Yes.
2. How does the [ProxyCanvas](#leads_gui.proxy.ProxyCanvas) support switching between different widgets?
    - The underlying widget behind all widgets is a canvas. By switching between different widgets, it is basically
      switching between different drawing methods.
3. In [CanvasBased](#leads_gui.prototype.CanvaseBased) widgets, are all elements re-rendered in every frame? What is the
  mechanism?
    - No. Only [`dynamic_renderer()`](#leads_gui.prototype.CanvasBased.dynamic_renderer) is called to refresh to widget.
      [`raw_renderer()`](#leads_gui.prototype.CanvasBased.raw_renderer) is only called during initialization.