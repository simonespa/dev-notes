# Web Development

Both [`picture`](https://caniuse.com/picture) tag and [`srcset` and `sizes`](https://caniuse.com/srcset)  attributes on `img` (or `source`) tags are compatible with all modern browsers.

If an older browser doesn't understand the [`<picture>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture) element, it will gracefully fall back to the [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) element inside of it. If an older browser doesn't understand the `srcset` attribute of the image, it will fall back to the `src` attribute.

The `<picture>` element (and it's `<source>` sub-elements) are best used when you want to do art direction on different sizes and/or aspect ratios of the image. Whereas, the img `srcset` attribute is better used if you want to design for different resolution displays and/or pixel density, that's because the attribute on the `img` tag is much more lightweight.
