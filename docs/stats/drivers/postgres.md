---
layout: default
title: Postgres
parent: Drivers
grand_parent: Trifle::Stats
nav_order: 2
---

# Postgres

Postgres driver uses `pg` client gem to talk to database. It uses `INSERT` with `ONCONFLICT` resolution and regular `SELECT` for interacting with database.

## Configuration

If you are using `rails` with `pg`, you can reuse your current pg connection.

```ruby
Trifle::Stats.configure do |config|
  config.driver = Trifle::Stats::Driver::Postgres.new(
    ActiveRecord::Base.connection.instance_variable_get('@connection')
  )
end
```

## Driver

You can either use your current pg connection, or pass in instance of custom pg connection.

```ruby
irb(main):001:0> client = PG::Connection.new
=> <PG::Connection:0x00007ff109a6d1f8>
irb(main):002:0> driver = Trifle::Stats::Driver::Postgres.new(client)
=> #<Trifle::Stats::Driver::Postgres:0x00007ff109c88028 @client=#<PG::Connection:0x00007ff109a6d1f8>, @table_name="trifle_stats", @separator="::">
```

## Interaction

Once you create instance of a driver, you can use it to `set`, `inc` or `get` your data.

```ruby
irb(main):003:0> driver.get(key: ['test', 'now'])
=> {}

irb(main):004:0> driver.inc(key: ['test', 'now'], count: 1, success: 1, error: 0)
=> {"count"=>1, "success"=>1, "error"=>0}
irb(main):005:0> driver.get(key: ['test', 'now'])
=> {"count"=>1, "error"=>0, "success"=>1}

irb(main):006:0> driver.inc(key: ['test', 'now'], count: 1, success: 0, error: 1, account: { count: 1 })
=> {"count"=>1, "success"=>0, "error"=>1, "account.count"=>1}
irb(main):007:0> driver.get(key: ['test', 'now'])
=> {"count"=>2, "error"=>1, "success"=>1, "account"=>{"count"=>1}}
```

## Performancee

`inc` and `set` operations are executed for each key/pair value separately. This may lead to degraded performance on large data set.
