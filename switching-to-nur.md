---
title: Switching to nur
layout: default
nav_order: 40
---

# Switching to `nur`

Switching to `nur` on a large project or when having many projects can be some hassle. As of this the
recommended workflow is:

1. Create a `nurfile` including all the original tasks but still calling your old task runner  
   (All devs can then either use the old task runner or `nur`)
2. Gradually convert your tasks to be written in `nu` shell script inside the generated `nurfile`
3. When everything is ready, remove the old task runner config and use `nur` from this day forward 👍

To simplify this progress I have created the [`nurify`](https://github.com/nur-taskrunner/nur/blob/main/scripts/nurify.nu) script to generate a `nurfile` from
many existing task runners. You can use this to simplify the first step. `nurify` is written in `nu` shell script,
so you need to use `nu` for this to work.

Usage: Put `nurify.nu` into your `NU_LIB_DIRS`, for example by using `cp nurify.nu $env.NU_LIB_DIRS.0`. Then
update your `nu` config script by adding `use nurify.nu` (you may use `vim $nu.config-path` to edit this file).

{: .note }
_Pull requests to add additional task/command runners to `nurify` are very much welcome!_
