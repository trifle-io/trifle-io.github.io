---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: About
nav_order: 1
has_children: false
---

Opinionated [Swiss Army knife](https://en.wikipedia.org/wiki/Swiss_Army_knife) of little big tools.

These gems came from necessity of building better solutions to common problems. Tired of using _shitty_ analytics and reading through _shitty_ log output.

These are small and simple. And that is OK. It is not one solution fits all type of things.

All gems from this collection are released under MIT license. You can find details inside of each gem.

___
# `Trifle::Stats`

[![Gem Version](https://badge.fury.io/rb/trifle-stats.svg)](https://rubygems.org/gems/trifle-stats)
[![Ruby](https://github.com/trifle-io/trifle-stats/workflows/Ruby/badge.svg?branch=main)](https://github.com/trifle-io/trifle-stats)
[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/trifle-io/trifle-stats)

Simple analytics backed by Redis, Postgres, MongoDB, Google Analytics, Segment, or whatever.

It gets you from having bunch of these occuring within few minutes
```ruby
Trifle::Stats.track(key: 'event::logs', at: Time.now, values: { count: 1, duration: 2, lines: 241 })
Trifle::Stats.track(key: 'event::logs', at: Time.now, values: { count: 1, duration: 1, lines: 56 })
Trifle::Stats.track(key: 'event::logs', at: Time.now, values: { count: 1, duration: 5, lines: 361 })
```

To being able to say what happened on 25th January 2021.
```ruby
Trifle::Stats.values(key: 'event::logs', from: Time.now, to: Time.now, range: :day)
=> [{2021-01-25 00:00:00 +0100=>{"count"=>3, "duration"=>8, "lines"=>658}}]
```

More [here](/docs/stats/).

---
# `Trifle::Logger`

[![Gem Version](https://badge.fury.io/rb/trifle-logger.svg)](https://rubygems.org/gems/trifle-logger)
[![Ruby](https://github.com/trifle-io/trifle-logger/workflows/Ruby/badge.svg?branch=main)](https://github.com/trifle-io/trifle-logger)
[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/trifle-io/trifle-logger)

Simple log tracer that collects messages and values from your code and returns Hash (at least for now).

It saves you from reading through your standard logger
```ruby
Trifle::Logger.trace('This is important output')
now = Trifle::Logger.trace('And it\'s important to know it happened at') do
  Time.now
end
```

To being able to say what happened on 25th January 2021.
```ruby
[
  {at: 2021-01-25 00:00:00 +0100, message: 'This is important output', state: :success, head: false, meta: false}
  {at: 2021-01-25 00:00:00 +0100, message: 'And it\'s important to know it happened ', state: :success, head: false, meta: false}
  {at: 2021-01-25 00:00:00 +0100, message: '=> 2021-01-25 00:00:00 +0100', state: :success, head: false, meta: true}
]
```

More [here](/docs/logger/).

---
# `Trifle::Env`

[![Gem Version](https://badge.fury.io/rb/trifle-env.svg)](https://rubygems.org/gems/trifle-env)
[![Ruby](https://github.com/trifle-io/trifle-env/workflows/Ruby/badge.svg?branch=main)](https://github.com/trifle-io/trifle-env)
[![Gitpod ready-to-code](https://img.shields.io/badge/Gitpod-ready--to--code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/trifle-io/trifle-env)

Simple encrypted store for environment variables in your database.

Did you ever wish you could update your ENV variables from your UI while storing them encrypted at rest?
```ruby
Trifle::Env['CLIENT_ID'] = 'cool_client_Id'
Trifle::Env['CLIENT_SECRET'] = 'supersecretclientsecret'

require 'oauth2'
client = OAuth2::Client.new(Trifle::Env['CLIENT_ID'], Trifle::Env['CLIENT_SECRET'], site: 'https://example.org')
...


More [here](/docs/env/).
