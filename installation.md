---
title: Installation
layout: default
nav_order: 20
---

# Installing `nur`

{: .important }
You do **not** need to have `nu` shell installed for `nur` to work. `nur` does
include the whole `nu` runtime engine and will run as a standalone command. As of this
it will run on any shell including `bash`, `zsh`, `fish`, `powershell` and or course `nu`
shell. You can use `nur` on Windows, MacOS and Linux.

As of now `nur` is not available using common package managers. This is however no issue as `cargo`
allows you to install packages into your own user directory.

{: .note }
You need to have `cargo` installed for this to work. See [cargo install docs](https://doc.rust-lang.org/cargo/getting-started/installation.html)
for details on getting `cargo` running.

Just run `cargo install nur` to install `nur` for your current user. The `nur` binary will be
added in `$HOME/.cargo/bin` (or `$"($env.HOME)/.cargo/bin"` in `nu` shell). Make sure to add
this to `$PATH` (or `$env.PATH` in `nu` shell).

POSIX shell example (like Bash, zsh, ...):

```nushell
> cargo install nur
> export PATH="$HOME/.cargo/bin:$PATH"  # put this into your .bashrc, .zshrc or similar
> nur --version
```

`nu` shell example:

```nushell
> cargo install nur
> $env.PATH = ($env.PATH | split row (char esep) | prepend [$'($nu.home-path)/bin'])  # put this into $nu.env-path
> nur --version
```

# Alternative installation methods

{: .note }
Both methods will also ensure `nur` is available in your `$PATH`.

## MacOS

For MacOS you can use my unofficial [Homebrew](https://brew.sh/) tap to install `nur`. All necessary
steps are documented in the repository:
[https://github.com/nur-taskrunner/homebrew](https://github.com/nur-taskrunner/homebrew)  
(This will allow you to just use `brew install nur`)

## Windows

For Windows you can just use the provided binaries available in each release version. Note the
`.msi` package might be the easiest to use:
[https://github.com/nur-taskrunner/nur/releases](https://github.com/nur-taskrunner/nur/releases)
