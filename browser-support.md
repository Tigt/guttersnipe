# Browser support #

The framework should support all browsers that can handle inline SVG inside HTML5. Fallbacks for those that don't are given below:

## No inline SVG support ##

The good news is that as of this writing, [~88% of browsers support inline SVG](http://caniuse.com/#feat=svg-html5). The suspects are:

* Internet Explorer 8 and below (particularly critical, as most webcomics get read in the workplace. Guess what else gets used at the workplace?)
* Opera Mini
* Android stock browser 2.3 and below
* Various ancient versions of just about everything else
* Whatever CanIUse's data doesn't cover

Elegant fallback solutions have all been thoroughly ruined by modern browsers' lookahead preparser (or whatever you want to call it), so the inline SVG fallback has two stages, in the spirit of progressive enhancement:

An unqualified [`<switch>` element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/switch) is used at the top level inside the initial `<svg>` element, like so:

````html
<svg>
  <switch>
    <g id="actual-comic-data">
      <!-- ... -->
    </g>
    
    <foreignObject id="fallback-container">
      <div id="fallback" style="background-image: url('{whatever.png}');
                                height: {Y}px;
                                width: {X}px;"></div>
    </foreignObject>
  </switch>
</svg>
````

Using a `<div>` with a `background-image` is an unfortunate workaround to avoid the preparser. Downloading both the SVG's embedded image data *and* the fallback image data is unacceptable, so this is what we have to do.

However, this is where the second part of the fallback comes into play. A small JavaScript function will run and [test for inline SVG support](https://github.com/Modernizr/Modernizr/blob/924c7611c170ef2dc502582e5079507aff61e388/feature-detects/svg/inline.js). If the test fails, it will transform the `<div>` into a real `<img>` element, and set its `src` to what the `background-image` previously had. This part of the fallback caters to:

* Proxy browsers such as Opera Mini, which doesn't support inline SVG and skips CSS background images by default to save bandwidth
* Users of assistive technology on systems that don't support inline SVG, to at least let them know there's an image there.
* Users who have engaged high-contrast mode or other visual accessibility helpers, which often disable CSS background images.

Exposing the accessible information from the `<title>` and `<desc>` elements should be involved, but I'll need to do (or commission) some testing to figure out how best to do that.

Generating the fallback PNGs is slightly trickier. Authors are unlikely to <kbd>PrntScrn</kbd> them one at a time, and even less likely to enjoy doing it. [Existing](https://www.npmjs.com/package/svg2png) [solutions](https://github.com/filamentgroup/grunticon) can generate fallbacks using [PhantomJS](http://phantomjs.org/), which should ensure that the fallback PNGs look similar to what a modern browser would produce.

The existing problem left to solve is dealing with the text left from inside the SVG: `<text>` and `<desc>` will dump their contents onto the page. `<title>` is instead assumed to be invalid and additional HTML `<title>` elements, and are discarded automatically.

## Vertical Centering ##

Flexbox support is [getting better all the time](http://caniuse.com/#feat=flexbox), but it's still a bit complicated and messy. This framework instead relies on [CSS Table Layout](http://caniuse.com/#feat=css-table), which is supported down to Interet Explorer 7. The only quirk to watch out for is [an old Firefox bug](https://bugzilla.mozilla.org/show_bug.cgi?id=63895), which has been fixed since version 31. The visual layout otherwise relies on ancient properties such as `overflow: hidden`, which should be rock-solid.

Browsers that don't support `display: table` instead get the comic images butted up against the top, which is ugly but not catastrophic.

Other vertical centering solutions were considered and discarded for a variety of reasons. Most of them were thrown out because they required knowing the dimensions of the comic, or the surrounding box, or both. The one most often used today involves `translate(-50%, -50%)`. This tended to result in odd rendering issues when combined with SVG, such as text artifacts. It also has poor browser support.
