# Members of the VeC Project

If you are a member of the [VeC Project](https://www.villanovacollege.org/giving/vec-project), you are granted with
access to assets of LEADS VeC (not included as assets of LEADS). Ask your contact for access to the
[LEADS VeC Asset Repository](https://github.com/ProjectNeura/LEADS-VeC-Assets).

## We are Looking for You

We are looking for new members of the VeC Steering Council. To quickly qualify yourself, prepare for these questions.

### Core

- How many contexts can there be?
    - You can create multiple contexts, yet only 1 can be registered.
- Describe the life cycle of a context.
    - Pre-push -> Post-push -> Pre-update -> On-update -> Post-update
- During which periods do plugins come into play?
    - Plugins can affect pre-push, post-push, pre-update, and post-update.
- Which should be registered first, a context or a configuration?
    - The configuration should always be ready before the instantiation of a context.
- What is SFT?
    - SFT, which stands for System Failure Tracer, allows different components to throw errors into a unified manager so
      that those error collection components can retrieve these errors.

### GUI

- Does the GUI main loop run in the main thread?
    - Yes.
- How does the [ProxyCanvas](#leads_gui.proxy.ProxyCanvas) support switching between different widgets?
    - The underlying widget behind all widgets is a canvas. By switching between different widgets, it is basically
      switching between different drawing methods.
- In [CanvasBased](#leads_gui.prototype.CanvaseBased) widgets, are all elements re-rendered in every frame? What is the
  mechanism?
    - No. Only [`dynamic_renderer()`](#leads_gui.prototype.CanvasBased.dynamic_renderer) is called to refresh to widget.
      [`raw_renderer()`](#leads_gui.prototype.CanvasBased.raw_renderer) is only called during initialization.