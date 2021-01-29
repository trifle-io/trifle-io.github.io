---
layout: default
title: Usage
parent: Trifle::Stats
nav_order: 2
---

## Usage

You don't need to use it with Rails, but you still need to run `Trifle::Ruby.configure`. If youre running it with Rails, create `config/initializers/trifle-ruby.rb` and configure the gem.

```ruby
Trifle::Ruby.configure do |config|
  config.driver = Trifle::Ruby::Driver::Redis.new
  config.track_ranges = [:hour, :day]
  config.time_zone = 'Europe/Bratislava'
  config.beginning_of_week = :monday
end
```

### Track values

Available ranges are `:minute`, `:hour`, `:day`, `:week`, `:month`, `:quarter`, `:year`.

Now track your first metrics
```ruby
Trifle::Ruby.track(key: 'event::logs', at: Time.now, values: {count: 1, duration: 2, lines: 241})
=> [{2021-01-25 16:00:00 +0100=>{:count=>1, :duration=>2, :lines=>241}}, {2021-01-25 00:00:00 +0100=>{:count=>1, :duration=>2, :lines=>241}}]
# or do it few more times
Trifle::Ruby.track(key: 'event::logs', at: Time.now, values: {count: 1, duration: 1, lines: 56})
=> [{2021-01-25 16:00:00 +0100=>{:count=>1, :duration=>1, :lines=>56}}, {2021-01-25 00:00:00 +0100=>{:count=>1, :duration=>1, :lines=>56}}]
Trifle::Ruby.track(key: 'event::logs', at: Time.now, values: {count: 1, duration: 5, lines: 361})
=> [{2021-01-25 16:00:00 +0100=>{:count=>1, :duration=>5, :lines=>361}}, {2021-01-25 00:00:00 +0100=>{:count=>1, :duration=>5, :lines=>361}}]
```

You can also store nested counters like
```ruby
Trifle::Ruby.track(key: 'event::logs', at: Time.now, values: {
  count: 1,
  duration: {
    parsing: 21,
    compression: 8,
    upload: 1
  },
  lines: 25432754
})
```

### Get values

Retrieve your values for specific `range`.
```ruby
Trifle::Ruby.values(key: 'event::logs', from: Time.now, to: Time.now, range: :day)
=> [{2021-01-25 00:00:00 +0100=>{"count"=>3, "duration"=>8, "lines"=>658}}]
```