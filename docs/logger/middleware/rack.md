---
layout: default
title: Rack
parent: Middleware
grand_parent: Trifle::Logger
nav_order: 2
---

# Rack

This middleware wraps any request made with a tracer. Key is taken from _NOPE_ and `request.params` are stored as a `tracer.meta`.

## Configuration

_This middleware is WIP. Pls don't use it!_

Rack does not have much access to rails controller and therefore interacting with context is kinda impossible. There is [Rails](/docs/logger/middleware/rails.html) integration that allows you to do what you are trying to do, but better!
