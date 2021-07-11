---
layout: default
title: Tags and Artifacts
parent: Trifle::Logger
nav_order: 7
---

# Tags

Simple array where you can add reference to related objects. Then use this in callbacks to reference logger from these objects. Or whatever you decide to do.

```ruby
Trifle::Logger.tag("Item:#{item.id}")
Trifle::Logger.tag('test')
```

# Artifacts

Another simple array that holds list of filenames that you may wanna persist in your callbacks. Or don't, it's up to you!

```ruby
# somewhere in your code
def screenshot
  file = recorder.screenshot!
  Trifle::Logger.artifact(file.split('/').last, file)
end

# later in callback
config.on(:wrapup) do |tracer|
  # Upload artifacts
  tracer.artifacts.each do |artifact|
    entry = Entry.find_by(id: tracer.reference)
    next if entry.nil?

    if Rails.env.development?
      path = "#{Rails.root}/public/artifacts/#{entry.namespace}/#{entry.id}"
      FileUtils.mkdir_p(path)
      FileUtils.mv(artifact, path)
    rescue StandardError => e
      next
    end
  end
  # Delete artifacts
  FileUtils.rm(tracer.artifacts)
end
```
