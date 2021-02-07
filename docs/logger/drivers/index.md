---
layout: default
title: Drivers
parent: Trifle::Env
nav_order: 4
has_children: true
---

# Driver

Driver is a wrapper class that persists and retrieves values from backend. It needs to implement:
- `set(key:, value:)` method to store value
- `get(key:)` method to retrieve value
