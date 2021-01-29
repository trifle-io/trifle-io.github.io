---
layout: default
title: Process
parent: Driver
grand_parent: Trifle::Stats
nav_order: 2
---

# Process

Because sometimes you just wanna throw some stats in the current process.

This is simple global `Hash` variable on a process. Grows over time. No expiration. Use with caution.

## Configuration

```ruby
Trifle::Stats.configure do |config|
  config.driver = Trifle::Stats::Driver::Process.new
end
```
