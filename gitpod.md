---
layout: page
title: Gitpod ready
nav_order: 5
has_children: false
---

# Gitpod

This gems come Gitpod ready. If you wanna try and get your hands dirty with Trifle, click the bedge in certain gem and watch magic happening. Badge like this [![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/trifle-io/trifle-stats)


It launches from custom base image that includes Redis, MongoDB, Postgres & MariaDB. This should give you enough playground to launch `./bin/console` and start messing around. You can see the Gitpod image in the [hub](https://hub.docker.com/r/trifle/gitpod).

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`.
