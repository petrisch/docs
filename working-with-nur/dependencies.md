---
title: Dependencies
parent: Working with nur
layout: default
nav_order: 30
---

# Task dependencies

## Using dependencies

When working with tasks you often have tasks that depend on other task to being run. Tools like `make` or `just`
have a special syntax to define dependencies between tasks, `nur` does not have this special syntax. Instead
`nur` allows you to call your dependencies like you normally would inside any shell.

### Example

Let's say you have a task called `nur qa` that calls the `nur test` and `nur lint` tasks.

```shell
def "nur test" [] { }
def "nur lint" [] { }

def "nur qa" [] {
    nur lint
    nur test
}
```

You can now run `nur qa` and both `nur test` and `nur lint` will be run.

{: .note }
Calling `nur ...` inside a task will **not** execute a new `nur` process, instead everything will run just
inside the current `nur` process. If you want to call `nur` as an external command you can use `^nur ...` instead.

## Dependency order

With this very simple and basic concept for running dependencies you can call dependencies at any place in your
task. So you could also call a dependency at the end of the task or in the middle of the task.

```shell
def "nur publish" [] { }

def "nur safe-publish" [] {
    cargo check
    nur publish
}
```

**Note:** When calling dependencies at the end of the task, please keep in mind that the return values and
output of the task will be what the last line in your task produces, see
[Defining `nur` tasks]({% link working-with-nur/tasks.md %}#task-output-return-value).

## Passing parameters to dependencies

As dependencies are normal `nur` task calls you can pass any parameter to them, like you normally would.

```shell
def "nur publish" [
    --dry-run,
] { }

def "nur extra-safe-publish" [] {
    cargo check
    nur publish --dry-run
    nur publish
}
```
