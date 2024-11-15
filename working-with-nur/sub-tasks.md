---
title: Sub tasks
parent: Working with nur
layout: default
nav_order: 40
---

# Sub tasks

Like with normal `nu` shell commands `nur` can also handle sub commands and thus sub tasks.

```nushell
def "nur something sub" [] {
    print "The sub task to something"
}
```

You could then just call `nur something sub` to run the sub task. This is a great way to organise your
`nurfile` into different logical parts, for example when using a monorepo.

## Example usage

### Monorepo

The following example shows how to use sub tasks with a monorepo like setup, for a project containing
a frontend (using JS) and a backend (using Python).

```nushell
def "nur frontend install" [] {
    cd src/frontend
    npm ci
}

def "nur backend install" [] {
    cd src/backend
    poetry install
}

def "nur install" [] {
    nur frontend install
    nur backend install
}
```

### Structure by type/domain

Of course you can also structure your commands by type/domain, for example:

```nushell
def "nur db import" [] {
    # ...
}

def "nur db export" [] {
    # ...
}
```

In this case I recommend adding a `db` task to show the help of those sub tasks:

```nushell
def "nur db" [] {
    help nur db
}
```

This will show the help of the `db` sub tasks.
