---
layout: default
title: Configuration
parent: Trifle::Logger
nav_order: 2
---

# Configuration

You don't need to use it with Rails, but you still need to run `Trifle::Logger.configure`.

## Tracer

You need to specify what tracer you want to use. For now there is only `Hash` tracer and configuration defaults to it.

```ruby
Trifle::Logger.configure do |config|
  config.tracer_klass = Trifle::Logger::Tracer::Hash
end
```

To persist your trace, you need to implement callback(s). Please read more about [Callbacks](/docs/logger/callbacks.html).
