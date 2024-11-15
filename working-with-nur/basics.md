---
title: Basics
parent: Working with nur
layout: default
nav_order: 10
---

# Basics

## The `nurfile`

Your tasks are defined in a file called `nurfile`. This file is a normal `nu` script and may
use `nu` commands to define `nur` tasks. All tasks must be defined as subcommands to `"nur"`, you
still will be able to define any other commands and use those as helpers in your tasks. Only
subcommands to `"nur"` will be exposed by running `nur`. All other commands will only be available
in your `nurfile`.

A simple "hello world" task could look like this:

```nushell
def "nur hello" [] {
    print "Hello world"
}
```

`nur` tasks will always be run inside the directory the file `nurfile` was found in. If you
place a `nurfile` in your project root (git root) you will be able to call tasks from anywhere
inside the project. This is useful to always have a reproducible base setup for all your tasks.

In addition you may add a file called `nurfile.local` to define personal, additional tasks. I
recommend adding the `nurfile` to git, while `nurfile.local` should be ignored. This allows
each developer to have their own additional set of tasks.

{: .tldr }

> - Add a `nurfile` to your project root, this is just a normal `nu` script
> - `nur` tasks need to be defined as subcommands to `"nur"`, like `def "nur something" [] { ... }`
> - Tasks will always be executed in the directory the `nurfile` was found in, `nur` will `cd` into that directory

## Environment provided by `nur`

`nur` will provide the internal state and config in the variable `$nur`, containing:

- `$nur.run-path`: The path `nur` was executed in
- `$nur.project-path`: The path `nur` executes the tasks in, this means the path the `nurfile` was found
- `$nur.task-name`: The main name of the task being executed, if any
  (Note: If you are running sub task this will _not_ include the sub tasks names, use `$env.NUR_TASK_NAME` instead)

`nur` will also set the following ENV variables:

- `NUR_VERSION`: The version of nur being executed (similar to `NU_VERSION`)
- `NUR_TASK_NAME`: The full name of the task being executed, including sub tasks
- `NUR_TASK_CALL`: The full call to the task, including "nur" prefix and all arguments
