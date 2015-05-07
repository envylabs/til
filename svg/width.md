If you place two `width` or `height` attributes on an svg, it’ll break the SVG. E.g:

#### Good
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600" width="800" height="600" …
```

#### Bad
```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600" width="800" height="600" width="800" height="600" …
```

Not sure why you would do this, but it’s just interesting that this breaks the file.
