Instead of doing something like:

```ruby
rescue => exception
  error = CustomError.new(exception.message)
  error.set_backtrace(exception.backtrace)
  raise error
end
```

There's a 3rd argument to `raise` that allows you to set the backtrace:

```ruby
rescue => exception
  raise CustomError, exception.message, exception.backtrace
end
```
