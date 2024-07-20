---
title: Define tasks
parent: Working with nur
layout: default
nav_order: 10
---

# Defining `nur` tasks

I highly recommend reading `nu` [custom commands](https://www.nushell.sh/book/custom_commands.html) for more details, but I will try to show you the
most important bits right here. I will use the term "`nur` task" to talk about subcommands to `nur`
in the following section. If you know about `nu`, just know that the tasks are actually really only
normal subcommands.

This means you define `nur` tasks  like `def "nur something"` - which you can then call by using
`nur something`. `nur` tasks can call any other `nu` commands or system command.

The most basic `nur` task could look like this:
```nu-script
def "nur hello-world" [] {
    print "Hello world"
}
```

`nu` commands are defined using the `def` keyword. The command name (`"nur hello-world"` in this case)
is followed by the arguments. Those are written in square brackets (`[` and `]`), see next chapter for
some details on arguments. The command body is then put into curly brackets and can contain any `nu`
script code.

For the most tasks this means your `nur` task will just execute some system commands like `poetry install`
or `npm ci` or `cargo publish` or something similar - but you can also create more complex tasks,
however you like. You may look into the [`nurfile`](https://github.com/ddanier/nur/blob/main/nurfile) of
`nur` itself for some examples.

`nu` commands will use the result of the last line in the command body as the command return value. `nur`
will then automatically print the return values of the task. The importand thing to understand is that `nu`
will see the output of a command as its return value. So this is also true for any command output
written to stdout, the output of the last line in your command will be used as the command result/output
and thus printed by `nur`. Any other commands that have been run in your command function
will be eaten by `nu`, unless you actively use `print` (`command | print` or `print (command)`).

This behaviour of `nu` commands may be strange at first glance, but makes a lot of sense when working
with pipelines the way `nu` does. Having any output produced by each line in a command definition be
redirected mixed output and result in errors handling the results. When using `nu` scripts for
`nur` tasks you need to know about this behaviour and handle any additional output you want to produce
accordingly.

An example using `print`:
```nu-script
def "nur do-something-useful" [] {
    print "We will do something useful now:"
    run-command-1 | print
    print "Now more useful stuff:"    
    run-command-2 | print  # you can also skip the `print` here, as it is the last line
}
```

If your command should not produce any output you can return `null`.

## Adding some arguments to your tasks

`nur` tasks can receive three different kinds of arguments:
* Named, positional arguments: `def "nur taskname" [argument1, argument2] { ... }`
    - Adding a `?` after the parameter name makes it optional
    - Above example provides the variables `$argument1` and `$argument2` in the task
* Flags as parameters: `def "nur taskname" [--argument1: string, --argument-number2: int] { ... }`
    - If you want to have named flags that can actually receive any values, you need to add a type
      (see below for typing)
    - Flags are always optional, default value will be `null` unless defined otherwise
      (see below for default values)
    - Flags will provide variables names without the leading `--`
    - Flags will be available in your task code as variables with all `-` replaced by `_`
    - Above example provides the variables `$argument1` and `$argument_number2` in the task
    - You may provide short version of flags by using `--flag (-f)`
* Boolean/switch flags: `def "nur taskname" [--switch] { ... }`
    - Boolean/switch flags must NOT be typed
    - Those can only receive the values `true`/`false`, with `false` being the default
    - Above example provides the variable `$switch` in the task
* Rest parameters might consume the rest of the arguments: `def "nur taskname" [...rest] { ... }`
    - Above example provides the variable `$rest` in the task

Arguments can (and should) be typed, you can use `argument_name: type` for doing so. A typed
argument could look like this:  
`def "nur taskname" [argument1: string, argument2: int] { ... }`  
(see [parameter types](https://www.nushell.sh/book/custom_commands.html#parameter-types) for a full list of available types)

Also arguments can have a default value, you can use `argument_name = "value"` to set the default value.
An example using a default value could look like this:  
`def "nur taskname" [argument1 = "value", argument2 = 10] { ... }`

Example with different kinds of arguments:
```nu-script
def "nur something" [
    name: string
    optional?: string
    --age (-a): int = 23
    --switch (-s)
] {
    null  # nothing here
}
```

## Adding documentation to your command

You may add documentation by adding commands to your `nur` tasks. See the usage example above and
the `nu` [command documentation](https://www.nushell.sh/book/custom_commands.html#documenting-your-command) section.

Basic rule is that the commend right above your task will be used as a description for that task.
Comments next to any argument will be used to document that argument.

Example task documentation:
```nu-script
# This is the documentation used for your task
# you may use multiple lines
#
# and use empty lines to structure the documentation (as long as it is one comment block)
def "nur something" [
    name: string  # This is used to document the argument "name" 
    --age: int  # This is used to document the argument "age" 
] {
    null  # nothing here
}
```

The above example will generate the following documentation when running `nur --help something` or `nur something --help`:
```shell
❯ nur --help something
This is the documentation used for your task
you may use multiple lines

and use empty lines to structure the documentation (as long as it is one comment block)

Usage:
  > nur something {flags} <name>

Flags:
  --age <Int> - This is used to document the argument "age"
  -h, --help - Display the help message for this command

Parameters:
  name <string>: This is used to document the argument "name"
```

## Using sub tasks for better structure

Like with normal `nu` shell commands `nur` can also handle sub commands and thus sub tasks.

```nu-script
def "nur something sub" [] {
    print "The sub task to something"
}
```

You could then just call `nur something sub` to run the sub task. This is a great way to organise your
`nurfile` into different logical parts, for example when using a monorepo.