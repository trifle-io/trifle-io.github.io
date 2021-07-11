---
layout: default
title: Dummy
parent: Drivers
grand_parent: Trifle::Stats
nav_order: 5
---

# Dummy

Not a real driver, just an example how you could write your own driver in 12 lines. This one does, well, nothing useful.

```ruby
irb(main):001:1* class Dummy
irb(main):002:2*   def inc(key:, **values)
irb(main):003:2*     puts "Incrementing #{key} => #{values}"
irb(main):004:1*   end
irb(main):005:2*   def set(key:, **values)
irb(main):006:2*     puts "Setting #{key} => #{values}"
irb(main):007:1*   end
irb(main):008:2*   def get(key:)
irb(main):009:2*     puts "Random for #{key}"
irb(main):010:2*     { count: rand(1000) }
irb(main):011:1*   end
irb(main):012:0> end
=> :get
```

## Configuration & Interaction

Use it with configuration

```ruby
irb(main):013:0> c = Trifle::Stats::Configuration.new
=> #<Trifle::Stats::Configuration:0x00007fe179aed848 @separator="::", @ranges=[:minute, :hour, :day, :week, :month, :quarter, :year], @beginning_of_week=:monday, @time_zone="GMT">

irb(main):014:0> c.driver = Dummy.new
=> #<Dummy:0x00007fe176302ac8>

irb(main):015:0> c.track_ranges = [:minute, :hour]
=> [:minute, :hour]

irb(main):016:0> Trifle::Stats.track(key: 'sample', at: Time.now, values: {count: 1}, config: c)
Incrementing sample::minute::1611696240 => {:count=>1}
Incrementing sample::hour::1611694800 => {:count=>1}
=> [{2021-01-26 21:24:00 +0000=>{:count=>1}}, {2021-01-26 21:00:00 +0000=>{:count=>1}}]

irb(main):017:0> Trifle::Stats.values(key: 'sample', from: Time.now, to: Time.now, range: :hour, config: c)
Random for sample::hour::1611694800
=> [{2021-01-26 21:00:00 +0000=>{:count=>405}}]
```

## Driver & Interaction

```ruby
irb(main):013:0> driver = Dummy.new
=> #<Dummy:0x00007ff6aa9313b0>

irb(main):014:0> driver.get(key: ['test', 'now'])
Random for ["test", "now"]
=> {:count=>644}

irb(main):015:0> driver.inc(key: ['test', 'now'], count: 1, success: 1, error: 0)
Incrementing ["test", "now"] => {:count=>1, :success=>1, :error=>0}
=> nil
irb(main):016:0> driver.get(key: ['test', 'now'])
Random for ["test", "now"]
=> {:count=>260}

irb(main):017:0> driver.inc(key: ['test', 'now'], count: 1, success: 0, error: 1, account: { count: 1 })
Incrementing ["test", "now"] => {:count=>1, :success=>0, :error=>1, :account=>{:count=>1}}
=> nil
irb(main):018:0> driver.get(key: ['test', 'now'])
Random for ["test", "now"]
=> {:count=>959}
```

You can see, not useful at all!
