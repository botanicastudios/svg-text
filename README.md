# SvgText
Creates multiline SVG `<text>` elements. Can be used on its own, but it is designed primarily for use as a tool within projects that use JavaScript to render or manipulate SVG.

<a href="https://dowjones.github.io/svg-text/">View a demo.</a>

Quick example:
```
import SvgText from 'svg-text';

// Will render a multiline <text> element into the document's first SVG element.
const text = new SvgText({
  text: 'Lorem ipsum dolor <a href="http://wsj.com">sit amet</a>, consectetur adipiscing elit.',
  element: document.querySelector('svg'),
  maxWidth: 100,
  textOverflow: 'ellipsis',
});

console.log(text.bounds);// { x: 0, y: 0, width: 100, height: 20 }
console.log(text.lines);// 2
console.log(text.text);// Reference to the SVG <text> element
```

## SvgText: options

### options.text
`string` The text to render into the `text` element.

### options.element
`SVG element` The element that the `text` element will be appended to.

### options.x|y
`number` Defaults to `0` for both properties.

### options.width|height
`number` Defaults to 'auto' for both properties.

### options.maxWidth|maxHeight
`number` Defaults to 'auto' for both properties.

### options.outerWidth|outerHeight
`number` Constrains the overall width or height, including the margin. For example, if the outerWidth is set to 100 and the margin is set to 10, then the width of the actual text will be constrained to 80 pixels. Defaults to 'auto' for both properties.

### options.maxLines
`number` If not set, possible lines of text are unlimited.

### options.align
`string` Values: "left", "center", "right". Defaults to "left". Relative to `options.x`, so if the value is "right" then the text will appear to the left of x, if "center" then it will appear centered over x, and if "left" then it will appear to the right of x.

### options.verticalAlign
`string` Values: "top", "middle", "bottom". Defaults to "top". Relative to `options.y`, so if the value is "bottom" then the text will appear above y, if "middle" then it will appear centered vertically over y, and if "top" then it will appear below y.

### options.textOverflow
`string` Values: "ellipsis", "clip", or a custom value. A string that is appended to the text if the text overflows the size constraints. "ellipsis" will result in "…" and "clip" in an empty string (""). Examples:
```
options.textOverflow = 'ellipsis';
options.textOverflow = ' »';
```

### options.selectorNamespace
`string` An optional selector that will be prefixed to all style selectors to serve as a namespace so that custom styling will apply only to a specific scope. Example:
```
options.selectorNamespace = 'svg#mySvg';
```

### options.className
`string` An optional class name to attach to the `text` element. This will be used for custom styling.
```
options.className = 'demo';
```

### options.style
`object` Custom styles to apply to the `text` element. Note that because the `text` element is an SVG and not an HTML element, "fill" and not "color" is the correct property to use to color text. Example:
```
options.style = { fill: 'red', font-family: 'serif', };
```

### options.styleElement
`HTMLElement` A `style` element into which custom styles will be written. Mandatory if custom styling is intended. Example:
```
options.style = document.querySelector('svg#mySvg style');
```

### options.attrs
`object` Attributes to attach to the `text` element. Example:
```
options.attrs = { 'data-foo': 'bar' };
```

### options.rect
`object` Attributes to attach to a background `rect` element. If not specified, no `rect` will be drawn. If "x" or "y" is defined, then these values will be added to the SvgText instance's x and y values. In other words, rect.x or rect.y will be treated as offsets or relative positioning values. If "width" or "height" are defined, these values will override any width or height set by the SvgText instance. Example:
```
options.rect = {
  fill: '#9cf',
  rx: 10,
  ry: 10,
  x: 15
};
```

### options.padding
`number|string` An optional value that will add space *inside* of a background `rect` or `text` (if no `rect` is drawn). Examples:
```
options.padding = 10;
options.padding = '15 20';
options.padding = '10px 5px 15px 20px';
```

### options.margin
`number|string` An optional value that will add space *outside* of a background `rect` or `text` (if no `rect` is drawn). Examples:
```
options.margin = 10;
options.margin = '15 20';
options.margin = '10px 5px 15px 20px';
```

## SvgText.forIllustrator()
A static method that prepares a `text` element for opening in Adobe Illustrator (tested in CS6 and CS2014), with the correct font, weight, style, etc. It takes three arguments:
- `textWeb` A text element that serves as a model.
- `textAi` A text element that will be transformed.
- `postScriptFontName` A PostScript font name that Illustrator will recognize.
Example:
```
const svg = document.querySelector('svg');
const text = new SvgText({
  text: 'Hello, world!',
  element: svg,
  style: {
    fontFamily: 'Helvetica',
    fontWeight: 'bold',
  },
});
const svgForAi = svg.cloneNode(true);
const el1 = svg.querySelector(`text[ai-id="${text.uid}"]`);
const el2 = svgForAi.querySelector(`text[ai-id="${text.uid}"]`);
SvgText.forIllustrator(el1, el2, 'Helvetica-Bold');
// Now `svgForAi` can be opened in Illustrator and the text element will render
// correctly with Helvetica Bold.
```

## SvgUtil: methods
Utility methods used by SvgText and exposed because they may be useful in other contexts.

## Future plans
- Add support for right-to-left languages.
- Implement unit testing on all applicable code.
