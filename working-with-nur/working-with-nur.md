---
title: Working with nur
layout: default
nav_order: 30
has_children: true
---

# Working with `nur`

As shown above you can use subcommands to `"nur"` to add your tasks. This section will give
you some more details and some hints how to do this in the best way possible.

## About the `nurfile`

Your tasks are defined in a file called `nurfile`. This file is a normal `nu` script and may
use `nu` commands to define `nur` tasks. All tasks must be defined as subcommands to `"nur"`, you
still will be able to define any other commands and use those as helpers in your tasks. Only
subcommands to `"nur"` will be exposed by running `nur`.

In addition you may add a file called `nurfile.local` to define personal, additional tasks. I
recommend adding the `nurfile` to git, while `nurfile.local` should be ignored. This allows
each developer to have their own additional set of tasks.

## Some basics about `nur`

`nur` tasks will always be run inside the directory the file `nurfile` was found in. If you
place a `nurfile` in your project root (git root) you will be able to call tasks from anywhere
inside the project. This is useful to always have a reproducible base setup for all your tasks.

`nur` will provide the internal state and config in the variable `$nur`, containing:

* `$nur.run-path`: The path `nur` was executed in
* `$nur.project-path`: The path `nur` executes the tasks in, this means the path the `nurfile` was found
* `$nur.task-name`: The main name of the task being executed, if any
  (Note: If you are running sub task this will *not* include the sub tasks names, use `$env.NUR_TASK_NAME` instead)

`nur` will also set the following ENV variables:

* `NUR_VERSION`: The version of nur being executed (similar to `NU_VERSION`)
* `NUR_TASK_NAME`: The full name of the task being executed, including sub tasks
* `NUR_TASK_CALL`: The full call to the task, including "nur" prefix and all arguments
