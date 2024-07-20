---
title: Quickstart
layout: default
nav_order: 20
---

# Quickstart

## Installing `nur`

See [installation]({% link installation.md %}) for details on how to install `nur`.

## Quick overview and example

`nur` allows you to execute tasks defined in a file called `nurfile`. It will look through your
current working directory and all its parents to look for this file. When it has found the `nurfile`
it will change to the directory the file was found in and then `source` the file into `nu` script.
You can define tasks like this:

```nu-script
# Just tell anybody or the "world" hello
def "nur hello" [
    name: string = "world"  # The name to say hello to
] {
    print $"hello ($name)"
}
```

The important bit is that you define your tasks as subcommands for "nur". If you then execute
`nur hello` it will print "hello world", meaning it did execute the task `hello` in your `nurfile`.
You can also use `nur --help` to get some details on how to use `nur` and `nur --help hello` to
see what this `hello` task accepts as parameters.

You may also pass arguments to your `nur` tasks, like using `nur hello bob` to pass "bob"
as the name to the "hello" task. This supports all parameter variants normal `nu` scripts could also
handle. You may use `nur --help <task-name>` to see the help for an available command.

Your tasks then can do whatever you want them to do in `nu` script. This allows for very structured
usage of for example docker to run/manage your project needs. But it can also execute simple commands
like you would normally do in your shell (like `npm ci` or something). `nur` is not tied to any
programming language, packaging system or anything. As in the end the `nurfile` is basically a
normal `nu` script you can put into this whatever you like.

I recommend reading "Working with `nur`" below to get an overview how to use `nur`. Also I
recommend reading the `nu` documentation about  [custom commands](https://www.nushell.sh/book/custom_commands.html) for details on how to
define `nu` commands (and `nur` tasks) and at least read through the [nu quick tour](https://www.nushell.sh/book/quick_tour.html)
to understand some basics and benefits about `nu` scripting.

## Add a `nurfile` to your project

You should add the `nurfile` to your project's version control system. This will allow you to
easily share your tasks with your team. You can also extend the available tasks locally by
adding an separate `nurfile.local` file to your project.

The `nurfile` should be placed in the root of your project.

### About task names

It is recommended to define and use a naming schema for your tasks. This could for example be 
something like:
 
* `nur install`: Setup the project, install all dependencies
* `nur update`: Update the project, update all dependencies
* `nur test`: Run all tests
* `nur lint`: Lint the project
* `nur run`: Run the project, for web project start the dev server
