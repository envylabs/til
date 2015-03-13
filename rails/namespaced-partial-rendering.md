# Rendering Partials from a Namespaced Controller

Given a model:

``` ruby
class Model
  def to_partial_path
    'shared/model'
  end
end
```

And a namespaced controller:

``` ruby
module Admin
  class Models < ApplicationController
    def show
      model = Model.new
      render locals: { model: model }
    end
  end
end
```

And a view:

``` haml
= render model
```

Rails will look for a partial with the path `admin/shared/model`. Even when
changing the return value of `#to_partial_path` to be an absolute path like
`/shared/model`, Rails will still use the path `admin/shared/model`.

While there has been some [discussion around this issue][issue], there hasn't
been a fix merged into Rails.

[issue]: https://github.com/rails/rails/pull/6478

Here's a workaround to modify the behavior of the `#render` helper to allow the
use of absolute paths when defining `#to_partial_path` in an object:

``` ruby
module RenderingHelper
  def render(options={}, locals={}, &block)
    return super unless options.respond_to?(:to_partial_path)

    object = options
    path = object.to_partial_path

    if path.start_with?('/')
      options = { partial: path, object: object, locals: locals }
      view_renderer.render_partial(self, options)
    else
      super
    end
  end
end
```

Place this into `app/helpers/rendering_helper.rb`.
