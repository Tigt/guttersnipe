# Using Inline SVG

SVG is a language a lot like the rest of the front-end web stack, mainly that it's a collection of mistakes calcified into a spec. (More specifically, HTML is a vocabulary for writing physics papers that's more useful for anything else, CSS governs visual appearance but has refused to do reliable layout for the last two decades, and JavaScript is a programming language of irrelevant quality welded onto the most unreliable API on the planet.)

But they let you reach people in ways no other languages can.

The rest of this post will probably convince you that SVG fits right in. That is, the inline HTML5 dialect of SVG, where you just copy & and paste it right into your `text/html`.

## Gotcha 1: Do not copy & paste your SVG right into your `text/html`

It will work, for a certain definition of "work," but you'll be combining all of HTML's problems with XML's problems, and then the new problems they spawn.

The fundamental issue is that we're parsing XML with the HTML5 parser. The HTML5 parser has been made aware of some of the XML eccentricities that SVG relies on, which means we get this liminal hybrid language that's neither XML or HTML but looks like both. I would link you to the spec for the full details, except if you would actually read that, why are you here?

The interesting bit is that SVG was designed to fail somewhat gracefully if you paste it into your HTML and some ancient browser gets to it. It shows the content of all the explicitly textual elements of SVG, such as its accessible descriptions and actual text-rendering elements, which is pretty good! [This was considered high treason by XMLites,](http://www.xml.com/pub/a/2000/03/15/deviant/#attributes) who *hated* old browsers.

### XML prologs are useless

"Prolog" is XML for stuff you put before the actual document. This raises more than one question but I can't answer any of them.

SVG has its own `<!DOCTYPE>` you could use, but the spec officially discourages using it even in honest-to-god standalone XML documents. If you include this in your inline SVG, browsers as old as Netscape will laugh at you.

Then there's the XML processing instructions, which look like `<?this ?>` and HTML browsers treat like funny comments. These mostly specified character set and stylesheets in XML, which your parent HTML document has nicer ways of doing anyway.

Along with the doctype and processing instructions, your SVG editor is likely to spit out a `<!-- comment -->` prolog telling the world this SVG was made with Illustrator or Inkscape or whatever. I appreciate this like I appreciate the unremovable Apache `Server:` header.

TL:DR; delete everything you paste in before the `<svg>` tag because they are at best intentionally useless.

### Namespace declarations manage to be worse than useless

The HTML5 spec requires that HTML parsers ignore any attribute that looks like `xmlns`, which works great in HTML5-capable browsers. Unfortunately, we still live with IE8, which goes into clown mode upon encountering a namespace declaration. The best-case scenario has namespacing as ignored, unnecessary bytes, so take those ugly things off your SVG.

Putting the `xmlns` attribute on the root `<svg>` does produce `hasLayout` on it. If you don't know what that means, you shouldn't trouble yourself by learning. If you need that for some reason and don't want to do it any other ways, then do what you like.

The HTML5 parser instead just requires that you use `xlink:` as the prefix for the xlink-based attributes, which *you were doing anyway.* Some tutorials and programs also spit out the `ev:` prefix for XML Events, and you can delete that with extreme prejudice.

### XML syntax isn't required

You can write inline SVG like HTML; unquoted attributes, case-insensitivity, and all that. I personally don't, because quoting attributes and lowercase is more consistent and easier in the long run, but as it turns out you may not want to self-close elements because it destroys the DOM in old IE, again. You can shiv `<svg>` to get around this, since you're not accessing the DOM without running JavaScript in the first place (aside from a few CSS hiccups), or you can make all your `<path/>`s into `<path></path>`s. Neither is ideal.

You may also want to expand some self-closing elements to insert the `<title>` and `<desc>` accessibility elements. Speaking of which...

### Implied namespaces

So the `xmlns` attributes are gone, but their effect lingers on. Inside `<title>`, `<desc>`, and `<foreignObject>`, parsers automatically switch back to HTML. `<title>` and `<desc>` never render (you can't even force them to, I've tried), so their inner HTML is mostly for accessibility cues (which is what they were designed for) and maybe some microdata if you like.

`<foreignObject>`, on the other hand, was designed to switch back to HTML rendering. *Supposedly* it was designed for "foreign content that can also be rendered by the user agent" but everyone knew that just meant (X)HTML. Internet Explorers 9 through 11 don't support it, by the way.

These three auto-switching elements are nice, but what happens if you use HTML tags elsewhere in your inline SVG? Well, the HTML5 parser thinks you forgot to put on your `</svg>` and unceremoniously dumps the rest of it. This is also why Google Translate fouls up inline SVG, because it wraps its output in `<span>`s. Try the Microsoft Translator instead.

### The `xml:` attributes

[These](http://www.w3.org/XML/1998/namespace) were *always* bolted to the `xml:` prefix, so that doesn't change. As a refresher, they consist of these:

* `xml:base`
* `xml:space`
* `xml:id`
* `xml:lang`

SVG defined its own `id` attribute, so you can drop `xml:id`.

HTML's `lang` might work, but so far `xml:lang` seems alright.

Whatever you're using `xml:base` to accomplish would be better served with HTML's `<base>`, and I don't even want to think about what happens if the two conflict.

`xml:space` was the XML way of saying `white-space: pre;`, where it would preserve your line breaks, tabs, and spaces, instead of collapsing them to one space max.

SVG doesn't do that! It instead does something in between, which is going to never be what you want. It converts line breaks and tabs into spaces, and then preserves *those.* You might as well just use `<tspan>`s since you'll be using those for faking line breaks and tabs anyway.

## Gotcha 2: SVG is a nightmare to render

### SVG doesn't render consistently

This isn't surprising: SVG merely describes how something **should** look, and it's up to the software and hardware to do the best they can. Raster images just ask you to put some pixels in order. "Vector" images instead ask you to realize the Platonic Ideal of the image, and then bring it earthbound to the pixels onscreen. SVG is even intentionally vague on a few fronts, for implementation optimizations, so you'd expect some antialiasing quirks and pixel-snapping inconsistencies.

[You wouldn't expect the inability to render a gradient consistently, since gradients are Graphics 102, but that's the world we've got.](http://www.quirksmode.org/blog/archives/2014/11/android_gradien.html) And those antialiasing quirks and pixel-snapping inconsistencies? Boy have we got those!

Of course, web developers have learned that visual consistency is at best a tertiary goal, so maybe that won't scare you. And eventually technology will march on enough that we'll stop being able to tell. But for now, using SVG involves [a lot](https://github.com/DmitryBaranovskiy/raphael/issues/175) [of worry](http://simurai.com/blog/2012/03/25/icon-sharpness-limbo/) [about subpixels](http://opticalcortex.com/svg-rendering-in-browsers/). It gets worse: there is a very nasty maybe-a-bug I can't track down for the life of me, where SVGs get Vaseline rubbed all over them from having anything influence their position that's laid out in subpixels. Like, say, if you have a box with `max-width: 100%`, and `height: auto`...

And this is just the very basic, draw-me-a-box-and-stroke-it SVG stuff. Filters? You're on your own.

### There is no good way to position and size inline SVG

Inline SVG has to figure out layout and positioning in the nexus of 3 very complicated systems: the Box Model, CSS Replaced Content, and the original SVG spec. Browsers tried valiantly for a while to make it "just work," but it didn't, they couldn't, and it doesn't.

So, let's say you want to make an SVG that takes up 200 by 350 pixels. That's easy! You just slot those integers into the root `<svg>`'s `height` and `width` attributes.

Wait, you want your *Scalable* Vector Graphics to *scale?* That's going to be a lot trickier. You're not going to like learning how to do this, because it's a series of hacks-on-hacks on a level we haven't seen since CSS refusing to provide a way of telling the browser to just goddamned center a block already. (`margin: auto` was a neat workaround, but why exactly do we still not have `box-align`? [Maybe we can use it in 2025.](http://dev.w3.org/csswg/css-align-3/))

To warn you up front, this is going to require:

* Understanding what on earth CSS means by "replaced content"
* The `padding-bottom` hack ([Which maybe we can stop using in 2025](http://dev.w3.org/csswg/css-sizing/))
* Understanding what on earth the SVG spec means by "user units"
* Dealing with Firefox, Chrome, Safari, Internet Explorer, and Android Stock Browser all doing something different
* Crying, drinking, or both

#### Replaced content

...is the term used for when something renders that *CSS can't totally handle.* Like `<img>`. Or `<video>`, `<iframe>`, `<input>`, and `<svg>`. (It might also be `<table>`, but that is a rabbit hole so deep that no spec determines how exactly tables display without explicitly specifying `table-layout: fixed` first.)

Most of the time you can get away without knowing a whole lot about what CSS *can* do with replaced content because the browser attempts to make it "just work." Images do in fact have pixel dimensions hidden in them, and the browser uses those unless you specify otherwise. And when you say `max-width: 100%` and `height: auto`, the browser can use those original pixel dimensions to figure out what exactly an `auto` height should be by respecting the aspect ratio.

`<iframe>`, being the naturally fluid entity of a web page, has no such guessable aspect ratio. You are probably familiar with how you need some awful JavaScript to get it to size nicely. SVG is sadly similar, but it *seems* like it offers a way out, since it has CSS sizing, the `width` and `height` attributes, and a `viewBox` attribute that basically does set an intrinsic aspect ratio.

BUT IT ONLY SEEMS THAT WAY. [Scroll down on this blog post until you see the demoralizing chart.](http://thatemil.com/blog/2014/04/06/intrinsic-sizing-of-svg-in-responsive-web-design/) Emil explains it in a very patient and thorough way, and if you hate hyperlinks the relevant conclusion there (which seems inescapable) is that you actually cannot make an inline SVG just scale with any combination of attributes or CSS.

([SVG apparently does have intrinsic size when you use the `width` and `height` attributes, but browser support on that is weird.](https://docs.google.com/presentation/d/1POUiroOBbLmXYlQKf0pIR8zVkHWH9jRVN-w8A4aNsIk/mobilepresent?slide=id.g1e19b0d66_230) If you don't supply those attributes, they default to a value of `100%`, which might be what you want anyway. Note that this presentation is mostly about browser behavior that **should** happen, not the current reality.)

Instead, we wrap it in a `<div>`, like *animals.*

#### The `padding-bottom` hack

This has been around for a long time, long enough that the reason it's `padding-bottom` and not `padding-top` is an old IE5 bug. You 

(Maybe just link to http://css-tricks.com/scale-svg/ ?)

And if you thought all that was bizarre, did you know you can nest `<svg>` elements, each with their own `viewBox` and `preserveAspectRatio`s? 

## Gotcha X: There is no way to gracefully degrade to or progressively enhance a regular old image

You cannot scaffold your SVG around a friendly, dependable `<img src="fallback.png">`. Well, you can, but you end up always downloading the fallback, because of the lookahead preparser/preload scanner/bitmap burglar/whatever the hell your browser calls it.

I know browser developers are very attached to their ["single biggest performance improvement browsers have ever made,"](http://www.stevesouders.com/blog/2013/04/26/i/) but they've made it impossible to ever enhance `<img>` in a way that doesn't hogtie developers into committing download sins without cooperation from said developers a la `<picture>`. [Chris Coyier of CSS-Tricks has compiled every trick that developers have tried to fix this.](http://css-tricks.com/svg-fallbacks/) All of them have problems: double downloads, JavaScript dependencies, accessibility snafus...

The compromise I ended up making was this:

```html
<svg ...>
    <switch>
        <g>
            <!-- actual content goes here... -->
        </g>
        <foreignObject>
            <img src="transparent-pixel.gif"
                 alt="Whatever alt text you would need"
                 class="fallback"
                 style="background-image: url('whatever.png')"
                 width="{X}"
                 height="{Y}">
        </foreignObject>
    </switch>
</svg>
```

When inline SVG isn't supported, a small script loops through all the fallback images and replaces the `src` with the `style.backgroundImage` values. This is mostly for Opera Mini and other proxy browsers, but it also helps if anybody is using High-Contrast Mode on IE8 or below.

Experiment: Hiding `<title>`, `<desc>`, and `<text>`.

`<title>` is fine, since non-complying browsers think it's an invalid HTML `<title>`.

Not all CSS properties apply to SVG. BREAK OUT YOUR IMAGE REPLACEMENT TECHNIQUES
* `text-indent: -9999px;`
* `color: transparent/rgba(0,0,0,0);`
* `height/width: 0;`
* `display: absolute;` -- SVG only has display: none and then display: fukkenWhateveryouWriteworks
* `overflow: hidden;`

## How do I hide a bare-minimum accessibility transcript unless inline SVG is on? `<switch>` again? "OPEN IT IN GOD DAMN LYNX IF YOU MUST. CURL IT TO STANDARD OUTPUT AND REEEEAAAAAAD IT."

## With TCP slow start, you get a whopping 85kB in the first second on mobile. Send HTML off of a nice static server, and put something useful in that first 14kB especially.

* Requires HTML5 parser: remember your `<!DOCTYPE html>`

* Works by defining an "SVG context," with a simple `<svg></svg>` element

* Requires CSS sizing set to render consistently in all browsers; [do not use the `height` and `width` attributes](https://docs.google.com/presentation/d/1POUiroOBbLmXYlQKf0pIR8zVkHWH9jRVN-w8A4aNsIk/mobilepresent?slide=id.g1e19b0d66_279). Include a `viewBox` too. Do this:

```html
<svg viewBox="0 0 {X} {Y}" style="width:{X}; height:{Y}">
```

* Currently requires some funky workarounds since it's in a new "namespace"

* bufferedRendering

* Don't use `<metadata>`; that's what `<head>` is for

* You'll need some ARIA help to make it truly accessible

* Use an unqualified `<switch>` for non-SVG supporting browsers

* No z-index.

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
* The `xlink:href` stuff is hardcoded. Don't try to change the namespace.
* You know how using `px` for CSS values is generally a bad idea? In SVG, using anything BUT `px` is even worse!
* The `viewBox` attribute is powerful enough to handle every single coordinate-system issue that could arise. This means it is impossible to understand.
* Its built-in accessibility elements are not actually supported by practically any assisstive technology. ARIA helps, thank god.
* There isn't actually a solid fallback to have worse browsers display a PNG instead. This comes close.
* There are four places an SVG can get its final displayed size: the `viewBox`, the CSS height and width, the height and width attributes, and the CSS 2.1 spec. 
* Google Translate wraps everything it changes in `<font>` tags (wtf?), and this causes the HTML5 parser to think you've ended the SVG early. However, Microsoft/Bing's translation service works perfectly. They both support the inline methods of opting out, so we'll have to use `<meta name="google" content="notranslate">`


Internet Explorer 9-11 `<foreignObject>` fallback:

```html
<switch>
  <foreignObject width="100" height="50" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility">
    <picture>
        <source ...>
        <source ...>
        <img src="default.png" alt="alternate text">
    </picture>
  </foreignObject>

  <g>
    <image xlink:href="default.png"><title>alternate text</title></image>
  </g>
</switch>
```

This works, except IE downloads both images, because of the fucking preparser again. I know it's kosher to hate on IE users, but they're people too, so I'd rather hate browser developers, because they're all smarter and more successful than I am.

Inline SVG CSS boilerplate:

```css
svg {
    vertical-align: middle; /*removes 3px gap at bottom*/
    display: block; /* does the same thing, but with more side-effects, so be sure you know what you want*/
    overflow: hidden; /*Fixed IE9 overflow bug, inline SVG as the root element is something *you'll* know you'll be doing*/
}
html|* > svg { /*analyze this*/
-webkit-transform-origin: 50% 50% 0px;
}
```

You can style the outermost `<svg>` element like any other HTML element, it's only inside that it switches to SVG-world.

Inline SVG, as you have seen, is a pain in the butt, but the good news is that you're not working with the XML version of SVG. Which is worse.