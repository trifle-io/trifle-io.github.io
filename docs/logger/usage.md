---
layout: default
title: Usage
parent: Trifle::Logger
nav_order: 3
---

# Usage

`Trifle::Logger` comes with `tracer` and `trace` module methods.

## Tracer

To start tracking you must initialize Tracer first. There are two tracers included, Hash and Null. They store data in structure as their name says so. Duh.

```ruby
Trifle::Logger.tracer = Trifle::Logger::Tracer::Hash.new(key: 'my_trace', meta: {count: 1})
```

Tracer is stored on `Thread.current`. Be aware when multithreading.

## Middleware

While you don't need to include middleware, it gives you automatic tracing and wrapup on every rack call or sidekiq execution. You don't need to initialize tracer every time you want to trace something. Middleware does it automatically for you based on a key/meta you set.

You can read more about each middleware configuration in its own section.

## Trace

Once you initialize Tracer, manually or through middleware, you can start tracing your code.

### Simple

The easiest way to use tracer is to replace all your `puts` or `Rails.logger.info` outputs with `Trifle::Logger.trace` method. This will do what you would expect, store the text with a timestamp.

```ruby
Trifle::Logger.trace('This is sample log message')
Trifle::Logger.trace("This is interpolated time #{Time.now} message")
```

### Head

Sometimes you need to mark a line of a new section in your log. Use `head: true` attribute to mark the line.

```ruby
Trifle::Logger.trace('Initializing connection', head: true)
params = { a: 1 }
Trifle::Logger.trace("Passing params: #{params}")
Rest::Client.post('https://example.com', params)
Trifle::Logger.trace('Done')
```

### Block

You can see how the above example of using params looks, well, bad. For this you can use `Trifle::Logger.trace` with block and assing result of a block to a variable. Logger will automatically include result of a block in your logs.

```ruby
params = Trifle::Logger.trace('Passing params') do
  { a: 1 }
end
```

### Error

Other times you may wanna point out that something caused an error. Pass `state: :error` argument to `trace` method.

```ruby
Trifle::Logger.trace('Connection failed', state: :error)