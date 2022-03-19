## Debugging: Showing Layers

You can show layers by setting `config.layering.show = true`

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.layering.show = true
end
```

This will show the **found** variables layers. To see **all possible** layers, see: [Debugging Layering Docs]({% link _docs/layering/debugging.md %}).

