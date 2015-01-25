SVG is a lot like the rest of the front-end web stack, namely: HTML is a vocabulary for writing physics papers that's been more usefully repurposed as literally anything else, CSS is a language governing visual appearance that won't do layout, and JavaScript, all things considered, is a pretty okay programming language welded onto the most frustrating API on the planet.

And SVG is a language for producing complex visuals that doesn't have an Z-index. It fits right in!

I've collected some gotchas for SVG that you may find helpful:

* The most future-proof way of using SVG is by including it inline. This means you will be parsing XML with the HTML5 parser.
* All of the DOM methods for dealing with SVG are those ugly `NSwhatever()` ones. The same ones Chrome really wants to deprecate.
* Setting the `width` and `height` attributes on the root `<svg>` element in percentages causes the behavior model to completely change. You should avoid using the attributes entirely and just size it in CSS.
* `<title>` is the official SVG equivalent of `alt`. It also brings up a tooltip. AND YOU THOUGHT THAT BEHAVIOR WAS DEAD WITH IE7
* The `<title>`, `<desc>`, and `<foreignObject>` elements automatically swap back to HTML. If you use SVG elements in them, or HTML anywhere else, the HTML5 parser will guess at what you wanted and *get it wrong.*
* Like I mentioned above, SVG doesn't have a Z-index. Instead, it uses the document source order.
* Absolutely none of the SVG drawing programs (Illustrator, Inkscape) produce code that can be pasted right into the HTML, despite what the spec obviously hopes for.
* Want to scale your Scalable Vector Graphics? BREAK OUT THE PADDING-BOTTOM HACK
* Do you prefer camelCase or hyphen-separators? SVG likes both!
* All of the filter stuff is prefixed with `fe`, despite only being children of the `<filter>` element. ???
* All web-based translation services aren't aware of inline SVG and cause the `<text>` to become `<span>` and fall out of the SVG DOM.
* The `xlink:href` stuff is hardcoded. Don't try to change the namespace.
* You know how using `px` for CSS values is generally a bad idea? In SVG, using anything BUT `px` is even worse!
* The `viewBox` attribute is powerful enough to handle every single coordinate-system issue that could arise. This means it is impossible to understand.
* Its built-in accessibility elements are not actually supported by practically any assisstive technology. ARIA helps, thank god.
* There isn't actually a solid fallback to have worse browsers display a PNG instead. This comes close.
* There are four places an SVG can get its final displayed size: the `viewBox`, the CSS height and width, the height and width attributes, and the CSS 2.1 spec. 
* Google Translate wraps everything it changes in `<font>` tags (wtf?), and this causes the HTML5 parser to think you've ended the SVG early. However, Microsoft/Bing's translation service works perfectly. They both support the inline methods of opting out, so we'll have to use `<meta name="google" content="notranslate">`
