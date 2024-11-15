---
title: Advanced
parent: Working with nur
layout: default
nav_order: 70
---

# Advanced

## Low level `nur` usage

When using your own commands as utilities and maybe even pack those into modules, it may come in handy to
run those commands one by one - either for debugging or to just get some small portion of your `nurfile`
run in rare occasions.

Do to this via your shell you can use `--commands`/`-c`, given the command `some-helper` from above you can
for example only run this command by using `nur -c "some-helper"`. I personally sometimes use this to only
run certain parts of my install/setup tasks to speed things up. Of course you only should ever do this when you
absolutely know what you are doing.

If you feel even more adventurous you may use `nur --enter-shell` to open up the `nu` shell powering `nur`
with everything initializes. This is particularly useful for debugging your `nurfile` and should not be used
for anything else. Note the shell will not use your normal `nu` shell setup, if you are using `nu` shell yourself
outside of `nur`. This is due to the fact that `nur` will - by design - not read your global `env.nu` and
`config.nu` to ensure `nur` works the same on every devs machine. Instead those files will only be project
specific, see below.

## Provide `env.nu` and `config.nu` for project specific setup

Like [with `nu`](https://www.nushell.sh/book/configuration.html) you can have your own environment
and configuration files in `nur`. Unlike `nu` those don't live in your `$HOME` folder but can be put into the
project and as of this into version control. This also means you can have different configurations for
different projects.

`nur` will load those files if they exist:

- `.nur/env.nu` for the environment
- `.nur/config.nu` for the configuration

The recommended usage is to put environment changes like changes to `$env.NU_LIB_DIRS` into `env.nur`.
After this file was loaded, those changes will already be active, allowing you to, for example, `source` or
`use` modules from additional paths. Then you may use the `config.nu` to add a project specific, but global,
configuration.

See the [`nu` documentation on `env.nu` and `config.nu` files](https://www.nushell.sh/book/configuration.html#nushell-configuration-with-env-nu-and-config-nu)
for some more insights. You may use the [default variants of both files](https://github.com/nur-taskrunner/nur/tree/main/src/nu-scripts)
as the base to do any modifications.

## Using aliases

You may use `nu` aliases to create shorter versions of tasks. As this most of the time is a personal optimisation
I recommend to set aliases only in your personal `nurfile.local`.

Example:

```nushell
# Install task defined by nurfile
def "nur install" [] {
    # ...
}

# Personal alias
alias "nur i" = nur install
```

This will allow you to use `nur i` instead of `nur install` to run the `nur install` task.

## Advanced topics and further reading

You may also look into those `nu` topics:

- [Commands](https://www.nushell.sh/book/custom_commands.html)
- [Variables](https://www.nushell.sh/book/variables_and_subexpressions.html) (also covers immutable/mutable variables)
- [Operators](https://www.nushell.sh/book/operators.html)
- [Control flow](https://www.nushell.sh/book/control_flow.html)
- [Builtin commands](https://www.nushell.sh/commands/)
- [Modules](https://www.nushell.sh/book/modules.html)
