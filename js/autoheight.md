Fill remaining height with jQuery
------

Simplest, most effective way to make an element fill remaining height with jQuery, with no magic numbers (be sure to [debounce](http://drupalmotion.com/article/debounce-and-throttle-visual-explanation)):

```
$(window).resize(function() {
  $('my_target_element').height($(window).height() - $('body').height() + $('my_target_element').height());
});
```
