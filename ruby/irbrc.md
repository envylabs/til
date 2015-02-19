# `.irbrc`

IRB will look for the file `~/.irbrc` when it loads, and load it if it exists.
This is a great place to put helpers that you commonly use in the console.

## Snippets

Here are some useful snippets for your `~/.irbrc`.

## Simplified Prompt

Use a simplified prompt style.

``` ruby
IRB.conf[:PROMPT_MODE] = :SIMPLE
```

## History

Save a history of recent commands.

``` ruby
IRB.conf[:SAVE_HISTORY] = 1000
```

### `#pp`

Autoload the `pp` core library the first time `#pp` is called, and modify its
return value to return `nil`, so you don’t end up with an annoying duplicate of
the data you’ve just printed to the screen.

``` ruby
module AutoloadPrettyPrint
  def pp(*args)
    require 'pp' unless defined?(super)
    super
    nil
  end
end

include AutoloadPrettyPrint
```

### `#o`

A useful way to output intermediate values in the middle of a method chain.
Insert `.o` at the point where you’d like to inspect the current value.

``` ruby
class Object
  def o
    pp self
    self
  end
end
```

For example:

``` ruby
>> users.collect(&:name).o.uniq.sort
```

### Local `.irbrc`

Look for a `.irbrc` file in the current directory, and load it if it exists.
This is a great way to allow for app-specific helpers to be defined.

``` ruby
if File.exists?('.irbrc')
  load './.irbrc'
end
```

## Pry

If your app is using `pry-rails`, the Rails console will use Pry instead of
IRB, and your `~/.irbrc` won’t be loaded by default. To fix that, create the
file `~/.pryrc` with the following contents:

``` ruby
irbrc = File.expand_path('~/.irbrc')

if File.exists?(irbrc)
  load irbrc
end
```
