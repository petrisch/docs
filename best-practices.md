---
title: Best practices
layout: default
nav_order: 50
---

# Recommendations and best practices

* Add the `nurfile` to git, allow each developer to extend the provided task by using a `nurfile.local` which
  is ignored by your `.gitignore`
* Provide some common tasks on each and every project, for me this would be something like:
    - `nur install`: Setup the project, install all dependencies
    - `nur update`: Ensure everything is up to date
    - `nur run`: Start the project, might run a dev server
    - `nur halt`: Stop the running project
    - `nur test`: Run the tests
    - `nur lint`: Run the linter
    - `nur qa`: Run all QA jobs (like tests + linter)
* When using more complec tasks, think about using [modules]({% link working-with-nur/modular-tasks.md %}) to 
  help you structure your tasks.
* On monorepos provide the same tasks for the whole project but also variants for the different components like
  `nur backend test` and `nur frontend test` which will both be run by `nur test`
* Use sub-tasks to group similar tasks. If you for example have tasks for exporting and importing the DB data
  you may use `nur db export` and `nur db import`
* Create tasks for all reoccurring tasks or tasks multiple people need to run
* Add docs to task (/command) and use typing
* Follow [nu shell guidelines](https://www.nushell.sh/book/style_guide.html) as well
