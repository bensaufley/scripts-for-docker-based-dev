# (Semi-)Universal Scripts for Docker-based Development

Inspired by GitHub's [Scripts to Rule Them All] pattern, this repo contains a
set of scripts that can be used for a standardized dev experience when working
in a Docker environment.

## Philosophy

I've bounced around projects (and started many of my own—"the first bite of an
apple" and all) and found that ramp-up was a constant burden. Leaving and coming
back to a project often meant completely reacquainting with the project's
dependencies, whether or not in Docker (though of course Docker makes this
easier, which is why this is a repo of Docker-centric scripts). I wanted to
create a set of scripts that could be used, at least as a starting point, across
projects to make the ramp-up process easier.

## Scripts

Scripts can be invoked from the root of the project with `script/[name]`. They
should all respond to `-h` for help.

- `bootstrap`: Check for dependencies and, if they're not present, alert and
  explain where to get them.

  Note: a more opinionated script could install them, but
  that starts to get into mucking with the user's system, which can cause
  problems. Sometimes there's more than one way to install a dependency, for
  example.
- `setup`: Set up the Docker environment from scratch. Will not use cache.
  Should do things like build all images, migrate db, etc. Will not obliterate
  volumes if they already exist. Calls `script/bootstrap` and `script/migrate`.
- `update`: Update the Docker environment. Will use cache. Will also do things
  like migrate db, etc. Will not obliterate volumes if they already exist. Calls
  `script/bootstrap` and `script/migrate`. Essentially behaves as `script/setup`
  if run from a clean environment.
- `migrate`: Spins up database and runs migration script. By default, this
  script will output an error but not fail because who knows if your use case
  even uses migrations.
- `server`: Runs `script/update` and then starts the Docker environment.
- `test`: Runs `script/update` and then runs the test suite.
- `console`: Runs `script/update` and then starts a console in the Docker
  environment.
- `cmd`: Runs `script/update` and then runs arbitrary commands in the Docker
  environment.

[scripts to rule them all]: https://github.com/github/scripts-to-rule-them-all
