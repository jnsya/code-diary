# Bundler

## Questions
- Seperately: require vs require relative vs load vs Rails' autoloading and lazy loading
- What is Rubygems, exactly - what functionality does it provide?
- What's the point of binstubs? Is it literally just that `bin/rsepc` is shorter than `bundle exec rspec`?

## Overview
- `bundler` is a dependency manager.

## Gemfile & Gemfile.lock
- You specify dependencies in the Gemfile. This is a list of gems, with an optional version requirement.
- The `Gemfile.lock` contains a list of every gem + version installed, as well as every gem's gem dependencies + version installed.
  - It differs from the Gemfile because it contains:
    a) versions - the actual version installed.
    b) dependencies - the gems that your primary gems need to have installed, as well as the versions of those dependencies.
- You never manually update the `Gemfile.lock`; only the `Gemfile`.

- `bundle update some_gem` updates a gem's version. That means changing the `Gemfile.lock` (not the `Gemfile`).

## bundle exec
- `bundle exec some_gem` executes the version of the named gem that's defined in the Gemfile.
- `somegem` (with no `bundle exec`) will execute the system-wide version of the named gem (if one exists).
- The advantage of using `bundle exec` is reliability: you know which version of the gem you're executing.

## Sources
- [Bundler docs](https://bundler.io/)
