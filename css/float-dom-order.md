Float DOM Order
------

I don’t even know what is going on, fully, other than `float: left` interfering with the stack order of `float: right` and vice-versa. Essentially you can hack the DOM order of sibling elements purely with `float: left; width: 100%`.
Tested in Chrome, Safari, and FF.

**Code / Example**

http://codepen.io/dangodev/pen/bNvMJB
