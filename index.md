---
title: Home
layout: home
nav_order: 0
---

Welcome to the nur documentation!

# nur - a taskrunner based on `nu` shell

`nur` is a simple, yet very powerful task runner. It borrows ideas from [`b5`](https://github.com/team23/b5)
and [`just`](https://github.com/casey/just), but uses [`nu` shell scripting](https://www.nushell.sh/book/programming_in_nu.md)
to define the tasks. This allows for well-structured tasks while being able to use the super-powers of `nu`
in your tasks.

* [Quickstart]({% link quickstart.md %})
* [How to install nur]({% link installation.md %})
* [Working with nur]({% link working-with-nur/index.md %})

{: .important }
You don't need to use `nu` as your shell to use `nur`. You can use `nur` from any shell, including `bash`,
`zsh`, `fish`, `powershell` and of course `nu` shell. `nur` runs on Windows, MacOS and Linux.
