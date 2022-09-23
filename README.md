# Sway
This is a fork of [Sway](https://swaywm.org), an [i3](https://i3wm.org/)-compatible [Wayland](http://wayland.freedesktop.org/) compositor. It introduces per-container color schemes.

Please refer to the [Sway GitHub](https://github.com/swaywm/sway/) for docs and related material.

To compile from source, follow the same procedure as Sway [here](https://github.com/swaywm/sway#compiling-from-source).

## Usage
See original [commit](https://github.com/swaywm/sway/commit/bb90e54ddd99e558363248a1f9359fcfd50a42e5)

Per-container color schemes allow a number of use-cases to be
implemented, particularly giving visual feedback to the user based on
specific properties of a view. This change adds that functionality in a
minimalistic manner:

We extend the client.<class> commands to accept container context, i.e.
work with criteria. sway_container is extended to carry its own local
border color classes configuration. Upon creation as well as
configuration changes a per-container merged color configuration is
pre-computed for rendering using a new helper function
container_update_border_colors(). Where previously the global config
structures were accessed directly, the pre-computed local per-container
colors are now used for rendering. To ease detection whether a
per-container configuration exists for a particular color class as
well as merging of configurations, the configuration structure is
switched to pointers referencing dynamically allocated buffers. That way
only pointer updates are needed to pre-compute color configurations per
container.

Command client.<class> is extended with subcommands "default" to remove
per-container configuration and "global" to explicitly address the
global configuration.

Test plan:
```
client.focused #000000 #000000 #ffffff #000000 #000000
for_window [app_id="foo"] client.focused #000000 #900000 #ffffff #000000 #000000
```
Start app foo and notice that the title bar is red when focused.

## Credits
- Proof of concept, from [michaelweiser](https://github.com/michaelweiser/sway/tree/window-colours)
- Swaywm, from [emersion](https://github.com/emersion)