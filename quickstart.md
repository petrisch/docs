---
title: Quickstart
layout: default
nav_order: 10
---

# Quickstart

## Installing `nur`

If you are familiar with `rust`/`cargo` you may want to install `nur` using `cargo`:
```shell
> cargo install nur
```

See [installation]({% link installation.md %}) for more details on how to install `nur`.

## Add a `nurfile` to your project

You should add a file called `nurfile` to your project root folder. Add this file to version control
as well, to share your tasks with your team. You can also extend the available tasks locally by
adding an separate `nurfile.local` file to your project.

## Adding some tasks

`nur` tasks are defined by using sub commands to `nur` in your `nurfile`, following the normal
`nu` syntax using `def`. A simple task could look like this:

```shell
def "nur hello" [] {
    print "Hello world"
}
```

You can then use `nur hello` to execute this task. Tasks may call external command like `npm ci`
or `poetry install` or anything else you like. Tasks can also call other tasks, see the
[working with nur]({% link working-with-nur/index.md %}) section for more details.

{: .note }
All tasks will be executed in the directory the `nurfile` was found in. If you place a `nurfile`
in your project root (git root) you will be able to call tasks from anywhere inside the project.
This is useful to always have a reproducible base setup for all your tasks.

### About task names

It is recommended to define and use a naming schema for your tasks. This could for example be 
something like:
 
* `nur install`: Setup the project, install all dependencies
* `nur update`: Update the project, update all dependencies
* `nur test`: Run all tests
* `nur lint`: Lint the project
* `nur run`: Run the project, for web project start the dev server
* etc.

See [best practices]({% link best-practices.md %}) for more details on how to structure your tasks.

## Switching to `nur` from any other task runner

If you are already using a task runner like `just` or `b5` you should read
[switching to nur]({% link switching-to-nur.md %}) to get an overview on how to switch to `nur`.
