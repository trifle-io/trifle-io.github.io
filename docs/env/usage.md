---
layout: default
title: Usage
parent: Trifle::Env
nav_order: 3
---

# Usage

`Trifle::Env` comes with `[]=` and `[]` module methods.

## Get

Classic of all, all you need is to use brackets and provide key for env variable youre trying to get. Can be lowcaps, upcase, symbol or string.

```ruby
Trifle::Env['AUTH_TOKEN']
Trifle::Env['auth_token']
Trifle::Env[:auth_token]
```

## Set

Until you set variable, get will always return `nil`.

```ruby
Trifle::Env['AUTH_TOKEN']
=> nil
Trifle::Env['AUTH_TOKEN'] = 'supersecrettoken'
=> true
Trifle::Env['AUTH_TOKEN']
=> 'supersecrettoken'
```
