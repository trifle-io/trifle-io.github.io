---
layout: default
title: Mongo
parent: Drivers
grand_parent: Trifle::Stats
nav_order: 3
---

# Mongo

Mongo driver uses `mongo` client gem to talk to database. It uses `bulk_write` and `find` operations for interacting with database.

## Configuration

If you are using `mongoid` with `mongo`, you can reuse your current mongoid connection.

```ruby
Trifle::Stats.configure do |config|
  config.driver = Trifle::Stats::Driver::Mongo.new(
    Mongoid.client(:default)
  )
end
```

## Driver

You can either use your default Mongoid client, or pass in instance of custom Mongo client.

```ruby
irb(main):001:0> client = Mongoid.client(:default)
=> #<Mongo::Client:0x24080 cluster=#<Cluster topology=Single[localhost:27017] servers=[#<Server address=localhost:27017 STANDALONE pool=#<ConnectionPool size=0 (0-5) used=0 avail=0 pending=0>>]>>
irb(main):002:0> driver = Trifle::Stats::Driver::Mongo.new(client)
=> #<Trifle::Stats::Driver::Mongo:0x00007fea23fb9d10 @client=#<Mongo::Client:0x24080 cluster=#<Cluster topology=Single[localhost:27017] servers=[#<Server address=localhost:27017 STANDALONE pool=#<ConnectionPool size=0 (0-5) used=0 avail=0 pending=0>>]>>, @...
```

## Interaction

Once you create instance of a driver, you can use it to `set`, `inc` or `get` your data.

```ruby
irb(main):003:0> driver.get(key: ['test', 'now'])
=> {}

irb(main):004:0> driver.inc(key: ['test', 'now'], count: 1, success: 1, error: 0)
=> #<Mongo::BulkWrite::Result:0x00007f82b41b34b8 @results={"n_modified"=>0, "n_upserted"=>1, "n_matched"=>0, "n"=>1, "upserted_ids"=>[BSON::ObjectId('60eaa78077d55d15b685182f')]}>
irb(main):005:0> driver.get(key: ['test', 'now'])
=> {"count"=>1, "error"=>0, "success"=>1}

irb(main):006:0> driver.inc(key: ['test', 'now'], count: 1, success: 0, error: 1, account: { count: 1 })
=> #<Mongo::BulkWrite::Result:0x00007f82b4447288 @results={"n_modified"=>1, "n_upserted"=>0, "n_matched"=>1, "n"=>1, "upserted_ids"=>[]}>
irb(main):007:0> driver.get(key: ['test', 'now'])
=> {"count"=>2, "error"=>1, "success"=>1, "account"=>{"count"=>1}}
```

## Performancee

All good here!
