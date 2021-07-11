---
layout: default
title: Redis
parent: Drivers
grand_parent: Trifle::Stats
nav_order: 1
---

# Redis

Redis driver uses `regis` client gem to talk to database. It uses `hincrby`, `hmset` and `hgetall` for interacting with database.

## Configuration

Easiest way is to reuse `Redis.current` connection.

```ruby
Trifle::Stats.configure do |config|
  config.driver = Trifle::Stats::Driver::Redis.new(
    Redis.current
  )
end
```

## Driver

You can either use your current redis client, or pass in instance of custom redis client

```ruby
irb(main):001:0> client = Redis.current
=> #<Redis client v4.3.1 for redis://127.0.0.1:6379/0>
irb(main):002:0> driver = Trifle::Stats::Driver::Redis.new(client)
=> #<Trifle::Stats::Driver::Redis:0x00007ffdc5a4d240 @client=#<Redis client v4.3.1 for redis://127.0.0.1:6379/0>, @prefix="trfl", @separator="::">
```

## Interaction

Once you create instance of a driver, you can use it to `set`, `inc` or `get` your data.

```ruby
irb(main):003:0> driver.get(key: ['test', 'now'])
=> {}

irb(main):004:0> driver.inc(key: ['test', 'now'], count: 1, success: 1, error: 0)
=> {"count"=>1, "success"=>1, "error"=>0}
irb(main):005:0> driver.get(key: ['test', 'now'])
=> {"count"=>1, "success"=>1, "error"=>0}

irb(main):006:0> driver.inc(key: ['test', 'now'], count: 1, success: 0, error: 1, account: { count: 1 })
=> {"count"=>1, "success"=>0, "error"=>1, "account.count"=>1}
irb(main):007:0> driver.get(key: ['test', 'now'])
=> {"count"=>2, "success"=>1, "error"=>1, "account"=>{"count"=>1}}
```

## Performance

`inc` and `set` operations are executed for each key/pair value separately. This may lead to degraded performance on large data set.
