---
title: Modular tasks
parent: Working with nur
layout: default
nav_order: 60
---

# Modular tasks

`nur` allows you to define helper commands and also any `nu` based modules.

## Using custom commands in your `nurfile`

You can define any command you like and need to use. Just know that subcommands to `"nur"` will
be available as tasks. All other commands will not be available.

```shell
def some-helper [] {
    do-something-useful
}

def "nur something" [] {
    print "Calling some-helper"
    some-helper
}
```

## Organising your `nur` helpers into modules

If your helper commands get more sophisticated you may want to use a `nu` module instead of
putting all of your code into one big `nurfile`. `nur` will automatically add the directory
called `.nur/scripts/` into `$env.NU_LIB_DIRS`. This allows you to define `nu` modules there and
then use those in your `nurfile`.

Basic hello world example:
```shell
# .nur/scripts/hello-world.nu

export def main [] {
    print "Hello world"
}
```

```shell
# nurfile
use hello-world.nu

def "nur hello" [] {
    hello-world
}
```

I recommend reading about [`nu` modules](https://www.nushell.sh/book/modules.html) in the official `nu` documentation.
