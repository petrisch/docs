---
title: Installation
layout: default
nav_order: 10
---

# Installing `nur`

As of now `nur` is not available using common package managers. This is however no issue as `cargo`
allows you to install packages into your own user directory.

**Note:** You need to have `cargo` installed for this to work. See [cargo install docs](https://doc.rust-lang.org/cargo/getting-started/installation.html)
for details on getting `cargo` running.

Just run `cargo install nur` to install `nur` for your current user. The `nur` binary will be
added in `$HOME/.cargo/bin` (or `$"($env.HOME)/.cargo/bin"` in `nu` shell). Make sure to add
this to `$PATH` (or `$env.PATH` in `nu` shell).

Shell example (like Bash, zsh, ...):
```shell
> cargo install nur
> export PATH="$HOME/.cargo/bin:$PATH"  # put this into your .bashrc, .zshrc or similar
> nur --version
```

`nu` shell example:
```shell
> cargo install nur
> $env.PATH = ($env.PATH | split row (char esep) | prepend [$'($nu.home-path)/bin'])  # put this into $nu.env-path
> nur --version
```

**Important:** You do **not** need to have `nu` shell installed for `nur` to work. `nur` does
include the whole `nu` runtime engine and will run as a standalone command.

# Alternative installation methods

## MacOS

For MacOS you can use my unofficial [Homebrew](https://brew.sh/) tap to install `nur`. All necessary
steps are documented in the repository:
[https://github.com/ddanier/nur-homebrew](https://github.com/ddanier/nur-homebrew)
(This will allow you to just use `brew install nur`)

## Windows 

For Windows you can just use the provided binaries available in each release version. Note the
`.msi` package might be the easiest to use:
[https://github.com/ddanier/nur/releases](https://github.com/ddanier/nur/releases)
