---
layout: default
title: Configuration
parent: Trifle::Env
nav_order: 2
---

# Configuration

You don't need to use it with Rails, but you still need to run `Trifle::Env.configure`.

Configuration allows you to specify:
- `driver` - backend driver used to store and retrieve env variables.
- `secret_key` - 32 char long string used for encryption.

If youre running it with Rails, create `config/initializers/trifle.rb` and configure the gem.

```ruby
Trifle::Env.configure do |config|
  config.driver = Trifle::Env::Driver::Redis.new
  config.secret_key = ENV['ENV_SECRET_KEY']
end
```
