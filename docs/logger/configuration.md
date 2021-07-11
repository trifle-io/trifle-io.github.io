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


## Callbacks

Callback `wrapup` is called when tracer finishes. Use this to persist your data in the structure you prefere.

```ruby
Trifle::Logger.configure do |config|
  config.on(:wrapup) do |tracer|
    LoggerModel.create!(
      key: tracer.key,
      params: tracer.meta,
      data: tracer.data,
      state: tracer.state
    )
  end   
end
```
