---
title: State Settings
nav_text: State
categories: config
order: 9
---

You can configure where UFO stores state. UFO only stores state specifically if you're using the [Base Docker Image feature]({% link _docs/features/base-docker-image.md %}).

## Example

By default, UFO will create a `ufo` CloudFormation stack and create a managed s3 bucket to store the state. You can use your own s3 bucket, though, by configuring this setting:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  # When state.bucket is not set ufo creates a managed s3 bucket
  config.state.bucket = "my-existing-bucket" # Set to use existing bucket.
  # config.state.managed = true # false will disable creation of managed bucket entirely
  # config.state.storage = "s3" # s3 or file
end
```

It's probably easier to leave the defaults, so you do not have to remember to create an s3 bucket.

{% include config/reference/header.md %}
{% include config/reference/state.md %}
{% include config/reference/footer.md %}
