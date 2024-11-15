---
title: Background
layout: default
nav_order: 60
---

# Background

## Why I built `nur` + some history

For me `nur` is the next logical step [after I created `b5`](https://medium.com/@david.danier/why-i-wrote-my-own-task-runner-twice-and-why-you-should-care-699d660be16d?postPublishedType=repub).
`b5` is based on running bash code and allowing users to do this in a somewhat ordered matter. Initially `b5`
even was just some bash script, but then eventually I figured bash is just not enough to handle my requirements.
So I switched to using Python, but `b5` was still based on bash, as it would generate bash code and then just
execute the code. One issue I always had with this approach was that again bash isn't that nice to write
complex things without introducing issues everywhere. Look for example at parameter handling.

Then along came `just`, which did implement its own language you could use to write your `justfile`.
This language was inspired by what a `Makefile` would look like, still without the issues `Makefile`'s
impose when using those as your task runner. Also, it did include a very nice way to define task arguments,
parse those, care about validation etc. Still the way `just` works is either to execute the task line
by line (and not having any context between those commands) or define some script language to execute
the full command (meaning using something like bash again). So `just` - at least for me - is a great
step forward, but still not what I had in mind when creating `b5` and what I would like to do with a
task runner. I think this also is the reason `just` calls itself a ["command" runner and not a "task"
runner](https://medium.com/@david.danier/why-you-should-use-a-task-runner-and-not-a-command-runner-seriously-5efb56a6ec63).

Then I came across `nu`, especially the nu shell. This did become my default shell after a while, and
I am using it as of now. `nu` feels nicely designed, has a very structured way to execute commands and
also handle their "response" data (stdout/err) - as everything is structured data there. This is way
better than the original UNIX approach of always passing text data. Also `nu` allows you to have simple
functions, that - as with `just` - handle argument parsing for you. So this did look like the perfect
combination for something like a task runner.

Of course, you could just define some `nu` functions to completely create a task runner and that would
already be better than `b5` or `just`. But this would also mean that every dev using this task runner
would need to switch to `nu` first. So I decided to try the hard route and create my own rust based
cli tool that would parse a `nu` script and then execute tasks defined in this script.

This is what you are seeing here. `nur` will load the `nurfile` defined in your project directory and
then allows you to execute tasks from this file. As it is its own binary you can easily use `nur` from
bash, zsh and possibly even PowerShell - whatever you prefer. Still you will be able to have the `nu`
superpowers inside your defined tasks.

## About the name

`nur` stands for "nu run". Basically it should be "nu run task", which would lead to "nurt" - but then I
decided for just "nur" as:

- `nur` is very fast to type (one less character 💪)
- `nur` is the reverse of `run`, which I like as a side effect 🥳
- and then as a nice and also weird side effect: You could translate "just" to "nur" in german 😂
