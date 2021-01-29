---
layout: default
title: Configuration
parent: Trifle::Stats
nav_order: 3
---

# Configuration

Configuration allows you to specify:
- `driver` - backend driver used to persist and retrieve data.
- `track_ranges` - list of timeline ranges you would like to track. Value must be list of symbols, defaults to `[:minute, :hour, :day, :week, :month, :quarter, :year]`.
- `separator` - keys can get serialized in backend, separator is used to join these values. Value must be string, defaults to `::`.
- `time_zone` - TZInfo zone to properly generate range for timeline values. Value must be valid TZ string identifier, otherwise it defaults and fallbacks to `'GMT'`.
- `beginning_of_week` - first day of week. Value must be string, defaults to `:monday`.

Gem expecs global configuration to be present. You can do this by creating initializer, or calling it on the beginning of your ruby script.

Custom configuration can be passed as a keyword argument to `Resource` objects and all module methods (`track`, `values`). This way you can pass different driver or ranges for different type of data youre storing - ie set different ranges or set expiration date on your data.

```ruby
configuration = Trifle::Ruby::Configuration.new
configuration.driver = Trifle::Ruby::Driver::Redis.new
configuration.track_ranges = [:day]
configuration.time_zone = 'GMT'
configuration.separator = '#'

# or use different driver
mongo_configuration = Trifle::Ruby::Configuration.new
mongo_configuration.driver = Trifle::Ruby::Driver::MongoDB.new
mongo_configuration.time_zone = 'Asia/Dubai'
```

You can then pass it into module methods.
```ruby
Trifle::Ruby.track(key: 'event#checkout', at: Time.now, values: {count: 1}, config: configuration)

Trifle::Ruby.track(key: 'event#checkout', at: Time.now, values: {count: 1}, config: mongo_configuration)
```
