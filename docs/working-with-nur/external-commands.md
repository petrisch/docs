---
title: Call external commands
layout: default
nav_order: 20
---

# Calling external commands from `nur`

If you want to run external commands you might run into the issue that `nu` itself provides some
[builtin commands](https://www.nushell.sh/commands/) that might match the name of the command
you want to run. This for example is the case for `sort`, where `nu` has it's own version (see
[sort command](https://www.nushell.sh/commands/docs/sort.html)). Most of the time it makes sense
to use the versions `nu` provides as those implement all the [pipeline improvements](https://www.nushell.sh/book/pipelines.html) of `nu`.
If you want to call the external command and not the builton function by `nu` use `^sort` instead
of `sort` in your `nur` tasks.

The same rule applies to your user defined functions, you would for example provide a function
named `grep` (`def grep [] { ... }`) which could call the `grep` command using `^grep`.

Example calling `ls` and `sort` system commands:
```nu-script
def "nur call-sort" [] {
    ^ls | ^sort
}
```

My recommendation would be to embrace the `nu` builtin commands and use the structured data those
provide and consume as much as possible. See "Some notes about pipelines and how `nu` handles those"
below for some more details on this.

## Provide `nur` tasks for wrapping shell commands

If you want to use a `nur` to run and wrap any normal command - for example to ensure you can run this in
any subdirectory of your project - I recommend using the following schema (using the `poetry`
package manager as an example):

```nu-script
def --wrapped "nur poetry" [...args] {
    poetry ...$args
}
```

The important bit is using `--wrapped`, so the `nu` parser will not try to match flags starting with
`-` into your `nur` task.

See the [docs for def](https://www.nushell.sh/commands/docs/def.html) for some more details.

## Some notes about pipelines and how `nu` handles those

Normal UNIX shells always use text to pass data from `stdout` (or `stderr`) to the next command via
`stdin`. This is pretty easy to implement and a very slim contract to follow. `nu` however works quite
different from this. Instead of passing test when using pipelines it tried to use structured data -
think of this like passing JSON between the different command. This increases the flexibility and
structured way to work with the data in a great way.

For example getting the ID of a running container in docker would look somewhat like this in a normal
UNIX shell:
```shell
docker ps | grep some-name | head -n 1 | awk '{print $1}'
```

This works for most of the cases, but might produce errors for example of a container named
`this-also-contains-some-name-in-its-name` exists. This issue exists as we are parsing
text data, not some actual structured data. So having the name anywhere in a line will result in
that line being used. (Note: I know about `docker ps --filter ...`, this is just to explain the
overall issue of parsing text data)

`nu` works on structured data and provides commands to filter, sort or restructure that data in
any way you like. Also `nu` provides mechanics to import text data into this structured format.
Getting the `docker ps` text data input `nu` can for example be done using `docker ps | from ssv`
("ssv" stands for "space-separated values"), see the [command `from`](https://www.nushell.sh/commands/docs/from.html)
for more possible input formats.

To get the first container matching using the image `some-name` you could use this command:
```nu-script
docker ps | from ssv | where IMAGE == "some-name" | get "CONTAINER ID" | first
```

This is using the [where command](https://www.nushell.sh/commands/docs/where.html) to match only
a single row and then the [get command](https://www.nushell.sh/commands/docs/get.html) to reduce the
row to just one column. There are also many more commands to work with structured data.

This way of working with command data in a very structured form is very much superior to
how normal shells used to work. This is especially good when you are creating more complex
scripts and thus also true for the tasks you will write in your task runner. This is why I did
choose `nu` for creating `nur`.

I recommend reading [thinking in nu](https://www.nushell.sh/book/thinking_in_nu.html#nushell-isn-t-bash) to
get a grasp about this concept and start using `nu` script in `nur` in a very structured way. Also
you may want to read the [`nu` documentation on pipelines](https://www.nushell.sh/book/pipelines.html).