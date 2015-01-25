#Outline#

1. Showoffy intro
  * Now that I have your attention, let's talk about webcomics!
2. What we've currently got
  * Influenced by comic strips or comic books; designed for the printed page
  * They're presented as monolithic images stuffed into `<img src>`. This isn't great.
    - Inaccessible: nobody in comics uses `alt` correctly but it wouldn't work anyway
    - Uncrawable: well Google can tell you what the comic's number is, as if you knew that
    - Unextendable without heavyweight tech like OCR and image analysis
    - Transcripts attempt to alleviate this, but are second-class citizens and rarely-used
    - Miserable on phones
    - Zoom and pan is largely unavoidable the way comics are currently set up. And we KNOW people hate vertical & horizontal scrolling together.
    - Generally terrible navigation designed for mouse pointers
    - Existing comics that adapt to mobile
      - Dinosaur Comics
      - xkcd (note the faked title text)
      - Smackjeeves Viewer, which is an approach Marvel is taking
      - All of these are merely retrofits. In fact, the "guided view" doesn't need the "full size" at all
3. Responsive comics
  * Existing solutions to responsive comics
    - The examples
    - Canvas-in, not content-out
    - Adapting existing comics made for the printed page
    - Panel relationships are too important to be treated like line breaks. (BONE example)
    - The page model is too important to leave behind; sudden reveals, etc.
  * Newspaper Sunday strips
    - A solution to a problem much like responsive web design
    - The fatal flaw of lack of creator control
    - If we want to design content-out, what is our content?
  * Panelsets
    - Platinum Grit & dA experiment
    - Creator-defined transitions where a "jump" exists anyway in the "Z" reading order we use
    - The page is valuable
  * The Elastic Canvas
    - The Infinite Canvas
    - The Infinite Canvas's philosophy contrasted with Responsive Web Design
    - The code for the Elastic Canvas
    - Some large comics can never be made small; this allows for that (Murder scene example)
  * Kid Radd segue to SVG comics (because there is nothing new under the sun, somebody came up with my solution many years earlier)
4. SVG comics
  * What Kid Radd did
    - Markup: comic as document
    - Text as text
    - Performance concerns
    - Animations & Scripting
  * Text as text
    - Accessible, crawlable, usable
    - Transcripts are already a thing, this solves the "Write twice" problem and the issue of where to put the transcripts (accessible enough for those who need them, unobtrusive enough for those who don't... just let the comic speak for itself)
  * Performance concerns
    - Responsive images
    - Separation of concerns
  * Animations & Scripting
    - What we DON'T want is some of the nonsense that comes out of Marvel's "motion comics"
  * Accessibility
    - SVG accessibility isn't perfect, but it's far better than `alt`
    - The `<title>` element shows up as a tooltip and as a result, with any popularity of SVG comics, will be turned into a joke much like how `alt` was also hover text for a while
    - `<title>` and `<desc>` thankfully [auto-switch to the HTML namespace according to the spec](https://html.spec.whatwg.org/multipage/syntax.html#html-integration-point), so we can leverage the usual semantics if necessary
    - Ideally we could have comics that transform into a radio drama; unfortunately aural CSS is a joke. But we'll see what we can do.


Putting the “Web” into Webcomics
================================

*attention-grabbing, tech-demo intro*

A figure walks through Zac Gorman rain in a city. *splish splish splish* follows SVG sound effects (Consider a rain loop.)

Tall shot of cigarette glowing, smoke rising (from both cigarette and chimneys), a couple lights burning, and the rain falling

He waits at a crosswalk.

Close up on a walk/don't walk sign, with the Don't Walk symbol slowly "burning"

"Replace panel" with the Walk symbol burning in its place

Figure looking up at the sign

Starting to walk, side view of him stepping onto the pavement

"Replace panel" with a car suddenly in front of him, tearing through the street, figure falling over

Back view, butt on sidewalk

Get up

Look left, expand panel to left

Look right, expand panel to right

Walk forward in expanded panel

5 "Revealing panels" step in for the next five clicks, with the figure walking up to his door in the last one. (The figure is in each panel, but each panel shows a single scene "through" their grille) 

View from inside: opens door, comes in, closes door

View from behind: opens closet

(Turns on closet lamp)

Skeleton replace panel; musical sting ( https://www.youtube.com/watch?v=ONQUwP0Fvsk )

"Now that I have your attention, let's talk about webcomics!"

--------

Comics on the internet, right? They've been the the same thing since forever: "Here's some image files for you to look at!"

(*Witches & Stitches* - a bunch of JPGs you could grab off Compuserve; *T.H.E. Fox* a bunch of GIFs on Usenet & Quantum Link; *Where the Buffalo Roam* - published through FTP & Usenet)

Even on the Web, the files were just slotted into `<img>` tags. You see, the idea is to use the 'Net until you get a *real* publisher, yeah? If you drew for print, you had the hard work already done when the syndicates picked you up!

I'm not *sure* when that way of thinking changed, but I have a few guesses why it did.

(A newspaper building on fire, Diamond Comics with tentacles strangling a comic book store, a shiny storefront saying "Publish for free!" "Reach millions of people!")

But you're probably sick of hearing that one.

It was a while before someone wanted to see what the Web could do that paper couldn't. In his own words:

(Argon Zark!)

>"[...]designed to take advantage of the RGB nature of computer monitors, HTML hyperlinks, GIF animations, JavaScript, CSS layering, dHTML animation, and more recently, Flash animation to enhance the story." cite=(https://www.linkedin.com/in/charleyparker)

The more things change, the more they stay the same, huh? Go ahead and give Argon Zark a try to see what it did with all those 90's buzzwords.

And really, Argon Zark is about as modern as any other webcomic today. Comics have since advanced in filesize, but they still fire up Flash to do anything fancier than hover text.

Webcomics are still stuck in Web 2.0. Except, I'm not sure they were ever part of Web 1.0: they take the "text" out of hypertext.  They're still just "Here's some image files for you to look at!"

Reading webcomics on mobile is really bad. It's the old pan 'n zoom song 'n dance we strive to leave behind. But with the panels locked in an opaque bitmap like that, it's all you can do.

However, try visiting [Dinosaur Comics](http://qwantz.com/index.php) on your phone. Since its panels are always the same size, it's automatically chopped up and presented in a mobile-friendly manner. But most comics insist on a *little* more variety in their paneling.

<img src="http://31.media.tumblr.com/27dc44a0528ca407755d16520e0add90/tumblr_inline_na3qxxSfOB1r95oxx.png" alt="Dinosaur Comics has been rearranging its panels on the mobile site for years.">

Web design today is adapting content to work on an unknowable spectrum of screen shapes and sizes. It seems like a very modern problem--but comics have actually run this gauntlet before.

<img src="http://31.media.tumblr.com/046bf6c29597049dc1cacf294c7de78b/tumblr_inline_na3qbh7PfE1r95oxx.png" alt="Didn't newspaper strips mandate flexible Sunday panel layouts so they could scale the comic to fit in different amounts of space?">

http://i.imgur.com/pnDRKk1.png

http://i.imgur.com/Up0xV4J.png

http://i.imgur.com/cSlemKH.png

Behold the Sunday Strip Layout: a newspaper standard to ensure editors could fit as many comics as possible into a variety of sizes and aspect ratios. You couldn't even count on them printing in color.

There are existing attempts at "responsive comics" that are spiritual successors to this approach. They're more fluid than the Sunday grid--flowing panels into arbitrary containers much like text.

There are experiments that flow panels into whatever width they're given:

[Responsive flexbox comic](http://demosthenes.info/blog/778/A-Responsive-Graphic-Novel-In-Flexbox)
[Arhanta Responsive Comics](http://arhantacomics.com/olimax-deep-blue)
[Sly Mongoose - A Responsive Comic](http://www.slideshare.net/pablodefendini/sly-mongoose-a-responsive-comic)
[Capow Framework for responsive comics](http://www.capow.net/)

Sure, you can wrap panels like text, but unlike text, paneling *lives* on its spatial relationships. A paragraph doesn't change its meaning when wrapped at 40 or 80 characters. But panels can be connected by sightlines, visual repetition, reinforcing reading direction, proximity... You wouldn't put *new* paragraph breaks in a novel when adapting it for small screens, would you?

Those of you who've read Scott McCloud also know that comic storytelling is hidden in the gutters. You can't mess with those without consequences.

And don't get me started on solutions that hide or crop panels depending on screen size. You don't hide content. M-dot sites learned that hard.

We've been taught *content-first* is the only way to future-proof against devices we haven't even dreamed of. These responsive comics are canvas-in.

So, let's analyze the content. What's the fundamental unit of a comic? We can get atomic and say "a panel," but I'd say sets of panels are closer to what we want. Nothing necessarily big or complex; you can get comedic timing and dramatic tension out of just two panels.

Unlike most of the problems facing modern web design, adapting comics to a variety of display sizes and aspect ratios has historical precedent. 

Fans of Bill Watterson probably just clutched their hearts, but this time, it's the authors deciding where the panels break. This is a hard concept to visualize, but you don't have to. It's what I've been doing this entire time.

This [deviantArt experiment](http://balak01.deviantart.com/art/about-DIGITAL-COMICS-111966969) and [Platinum Grit](http://www.platinumgrit.com/issue20.htm) both did this. Give those two links a read.

When is a design perfect? When there's nothing else to take away. By presenting "panelsets" individually, you focus on the immediate storytelling, and open up new space with sudden transitions, incremental changes, etc.

Maybe we didn't want an [Infinite Canvas](http://scottmccloud.com/4-inventions/canvas/); we want an elastic one, that's only as big as it needs to be.

We've learned that panning and zooming around a document isn't fun, and an "infinite" canvas promises that experience on every screen. Content parity, I guess, but...

![The first infinite canvas comic is horribly mangled and impossible to read comfortably on mobile browsers.](https://31.media.tumblr.com/dd8e5d7d316e091dece271c224e12325/tumblr_inline_ncp1tzTcjQ1r95oxx.png)

Of particular irony is that bit about "load each portion on a need-to-see basis." You know, kind of like pages.

Viewing these early thought experiments on what actually happened to our computers quickly abolishes some assumptions.

Scott lays down the ideals of the infinite canvas, but misses that most don't serve the story. In his own words:

<blockquote cite="http://scottmccloud.com/4-inventions/canvas/index.html">
Print cartoonists (myself included) make a constant series of compromises in pacing and design to stuff our stories into pages. We add and subtract panels, restrict size variation, break reading flow, and rarely if ever vary the distance between panels for fear of wasting paper. Without such restrictions, though, every one of those choices can be made *exclusively* on behalf of *the needs of the story*.
</blockquote>

Scott was thinking in the wrong direction. We don't want an **infinite** canvas. We want an **elastic** canvas, one only as big as it needs to be. So how big is that? How small can we subdivide comics before it hurts more than it helps? How about panels?

Panels are the atoms of comics. You can smaller pieces that stand alone (like a freefloating electron), but they're emphasized by what they lack. A word balloon floating in space implies a panel, but the reader searches for the missing context. And a single panel can serve as a perfectly fine comic on its own with the addition of something that suggests the passage of time, like dialogue, sound effects, or even something as simple as speed lines.

But where panels shine most is when they work with one another. Our atoms join together to create molecules, a wholly new substance altogether. *This* is what we should be emphasizing; instead of broadsiding the reader with every single panel at once winging off in as many directions as possible, we'll present panel arrangements as the story demands.

Molecules can join to produce things larger than themselves, of course, but they're more loosely coupled then, and the exact ways they relate to each other depend a lot on context. Context that internet publishing can't depend on: 4k pixel supermonitors, cheap obsolete Androids, living room TVs, screen-reading accessibility software, black & white e-readers, smart watches, and everything in between. Differences as minuscule as users zooming into web pages or having browser toolbars installed can and do break any layout micromanaging.

<blockquote cite="http://scottmccloud.com/4-inventions/canvas/index.html">
Both display resolution and bandwidth has limited the size of the big comics maps that this design strategy depends on. We're not out of the woods yet, but Moore's Law still seems to be on track.
</blockquote>

Scott yearns for a future where computers can handle whatever creators throw at them, but forgets about the audience's ability to do the same. That's the real limitation. *We* don't want an infinite canvas: it's exhausting. There's no pauses, no bookmarks, no breathers. Pages "refresh" our focus and give us regular breaks. The book replaced the scroll for a variety of reasons, including this one: the ability to pause.

Physical pages do force limitations on comics that aren't ideal. But abolishing them is a terrible idea. We now have the ability to define our **own** pages, instead of budgeting space and storytelling concerns against each other.

If a joke needs only 3 panels, then by god *only use those 3 panels*.

<blockquote cite="http://scottmccloud.com/1-webcomics/icst/icst-4/icst-4.html">
**Sustained rhythm** is a natural *corollary* of narrative subdivision. When artists escape the need to chop their stories up for paper and ink, it becomes possible to build moods through sustained, uninterrupted opening monologue from <a href="http://scottmccloud.com/1-webcomics/zot/zot-06/zot-06.html">Zot! Online Week #6</a>.
</blockquote>

Scott nails that comics use space to signify time passing. But space can and is subdivided to provide more punctuated travel, like doorways. How about pages? Instead of using space to signify time, we can also just... use time.

<blockquote cite="http://scottmccloud.com/1-webcomics/icst/icst-4/icst-4.html">Perhaps most importantly, by maintaining that *single unbroken reading line* that those strange new trailing scrolls afford--we may be on the "trail" of a new form of sequential art--that recaptures the *pure expression* of comics' *pre-print ancestors*[...]
</blockquote>

I don't think the Bayeaux Tapestry is somehow "purer" than what we have today, and it's *definitely* less readable, even if translated.

A long time ago, <a href="http://en.wikipedia.org/wiki/Word_divider#Scriptio_continua">the written word wasn't broken up whatsoever</a>. No spaces. No punctuation. Just a stream of characters.

These ancient pieces are legendarily difficult to read. Nobody's hearkening back to a time of "pure expression" when everything was one big run-on sentence. Whatever "purity" even means in this context (naivety is my best guess).

<blockquote cite="http://scottmccloud.com/4-inventions/canvas/index.html">
I imagined all of the book’s 1200-plus panels visible at once—from a great “distance” as it were—allowing the reader to then zoom in and start navigating from one panel to the next.
</blockquote>

Wow, no. I don't want to look at all the panels at once. Pasting all of a book's pages to a wall doesn't improve it. If you're unsure if you like that sort of thing, <a href="http://e-merl.com/pocom.htm">try this example of what McCloud's gunning for</a>.

I don't know about you, but I hate it. It provides absolutely no value to me, the reader. Do we need complicated panel constructions to best tell the story? After all, the most effective transition in film is still just the simple, straightforward cut.

I'm not interested in what we can do with *more*. Adding more is easy. I want to see what we can do with less.

Of particular irony is this proposed improvement to having to load these huge hypercomics all at once, as the web tends to do:

<blockquote cite="http://scottmccloud.com/1-webcomics/icst/icst-1/icst-1.html">
We already know from *games* that large continuous *navigatable landscapes* can be created that load each portion on a *need-to-see* basis.
</blockquote>

What, like pages?

Look at <a href="http://balak01.deviantart.com/art/about-DIGITAL-COMICS-111966969">this</a>.

Now how about <a href="http://www.platinumgrit.com/issue20.htm">this</a>?

So that's what I'm doing here. The tech is pretty simple.

I went on a big Progressive Enhancement kick awhile back. It was so intense I planned to support browsers that couldn't handle forms. That was pretty silly, but my refusal to deal with JavaScript (influenced wholly by not understanding it) led me to an interesting pure CSS slideshow setup, which you're using right now.

The code is simple and as old as CSS2.1:

    html, body, #viewer, #panelset {
        height: 100%;
        width: 100%;
        margin: 0;
        padding: 0;
    }

    #viewer { overflow: hidden; }
    #panelset { position: relative; }

    .navlink {
        display: block;
        position: absolute;
        top: 0;
        bottom: 0;
    }
    .next { right: 0; }
    .prev { left: 0; }

This pure CSS approach allows for needed flexibility: undo a few lines inside `@media print { }` and bam: 

To my surprise, this technique had already existed, for the exact same purpose. Ever heard of [*Kid Radd*](http://www.bgreco.net/kidradd.htm)?

*Kid Radd* was a sprite comic (and imo, the exception that proved the rule about them being terrible) that was made in the bad old days of IE dominance and presentational/proprietary markup. As a result, it's decayed in modern browsers, but it still works. ([Check out the creator's original technical explanation for a real blast from the past.](http://www.bgreco.net/kidradd/making1.htm))

It's also, as you may notice, made of markup. The text is real text! The foreground and background images are independent! JavaScript triggers animations! It has sound! It's not just a comic; it's a **DOM**ic!

I'll let you finish cringing.

If we peer past the forbidden old gods of `<frameset>`, `<table background=image.gif>`, and `onClick`, we see something very exciting.

HTML was designed to mark up documents. *Kid Radd* treats the comic as said document. In this past, there's bits of the present. Semantics, accessibility, flexible images, bandwidth concerns. You like those, right?

It's possible to overlay regular text on images, like Capow and Olimax above do, but fitting text into a speech balloon and having it scale nicely isn't really feasible. HTML wasn't designed for exact typesetting. Flow text into rectangles? Sure. Flow text into an arbitrary shape that doesn't break when zooming? Well, there's `vmax`, and CSS Regions might become real within the next decade. Right now, it's either ugly hacks (`vmax`, `white-space: pre`, and a lot of hoping) or science fiction.

It's doable, for sure, you're just stuck with severely constrained design. Creators will just stick to images with baked-in text.

Today, we have much more powerful tools to work with. You know what's great at accessible text with robust layout and artistic treatment? SVG.

It's definitely possible to make comics in pure SVG, but artists today are most comfortable with bitmaps, and many art styles are poorly suited to vector graphics. Thankfully, SVG provides a way to embed bitmaps with the `<image />` element. Separation of the word balloons as SVG `<text>` overlaid on a `<path />` brings us excellent graphical control while exposing the text to search engines, user agents, assistive technology, machine translation...

Separating the art from the words also allows for some interesting performance considerations. Aggressive JPEG compression is less noticeable on photographic images, but grimy on text and word balloons. Separating the two lets us crush one and GZIP the other.

You can even embed with `<foreignObject>`, which I've been doing for the code samples and such. Not bad, eh?

DOMics allow for fancy stuff like animations, interactivity, hyperlinks, sound, and other whiz-bangery. Usage of those elements is often overdone, but as techniques mature, interesting things could be done. Imagine hiding word balloons to look at the art unhindered, subtle environmental loops, depth of field...

Now, here's a problem: The only programs that even have a hope of helping creators make comics that are a fusion of SVG, CSS, responsive images, HTML, and JavaScript are browsers. Unless somebody wants to port Chromium into Inkscape (hint: don't), we'll need a web app. Not a service. There's too much pain possible there. (It would be *really cool* to let people download a single `.html` file that holds a full application, but apparently there's grief when crossing the network and filesystem streams. Just ask [TiddlyWiki](http://tiddlywiki.com/#GettingStarted))

There is, of course, the snag that SVG is very well-supported, but [the long tail of older browsers](http://caniuse.com/#feat=svg-html5) need fallbacks better than we've got.

In the XML world we never got (which, don't get me wrong, I'm glad about), the fallback was easy and robust; stick an `<img>` in `<desc>`, wrap all `<text>` in `CDATA`, and inline SVG wouldn't even hiccup in old browsers. (I tried this. Works great in old browsers, modern ones hate it.)

In the world we did get, we need IE conditional comments, a `canvas` renderer for Android, a JavaScript solution for Opera Mini... the Internet's best and brightest couldn't find a good way to fallback to even a simple .png across all browsers without double downloads, JavaScript required, Flash installed... 

The goddamned speculative preloader ruins about 80% of the solutions. So glad about that.

My artist side, though, tells my developer side to go shove it. Other implementers may require a fallback for IE8, since the dirty secret of webcomics is that they get read at work a lot.

SVG is pretty legit, as far as XML-derived web standards go, and it's only getting better, if [Tab Atkins' blog](http://www.xanthir.com/b4RR0) is to be believed.

#Why Accessibility?

* Yes, the blind
* But also the vision-impaired
* Reading disabilities
* The illiterate--like small children
* Language barriers--vocal and visual don't always go hand-in-hand
* Word lookups for vocabulary gaps
* Brains are many and varied--a text description can help those with difficulties (a Show/Hide toggle could work nicely, just `display: none` or `block` brings the `<title>` and `<desc>` tags in and out
* SVG lets you apply user stylesheets, for those who could have reading difficulties. Imagine: `.word-balloon: color: white; background: black` for those who require inverted text to read comfortably, or `font-family: dyslexic sans`. This part often rankles creators: who is a reader to change their work of art? Well, they know their capabilities better than you do, and either they can do what they can to read your comic, or they just skip it entirely. You're losing control, but gaining readers. And you never actually had that control in the first place: people experience colors, context, vocabularies, moods, and all other components of your work differently. They read even locked-tight images at a variety of distances, zoom levels, eyeglass prescriptions, visual capabilities, and screen capabilities. The web takes away absolute control by the creator, and that's a strength. Better a reader can make their own Large Type edition so you don't have to, right?
* The small amount of speech CSS that exists can make screen readers more pleasant. And if you dislike the sad state of support for speech styles, the new Synthesized Speech API lets us try to do even better. (And exposes it to the QA of abled readers, who can double-check accuracy for you)
* Increased accessibility for those who need it ends up benefiting everyone; have you ever rolled luggage up a wheelchair ramp? Temporary lack of ability visits us all, with activities, or sickness, or age, etc. And if you just don't know what a panel is supposed to be, for whatever reason...
* You want more people to enjoy your work, right? Why limit your audience to the abled?

#Why Extensibility?
* Text is powerful in subtle ways.
* Search engines can find topics a comic discusses, you can find all comics with a certain character, find a specific phrase...
* Machine translation
* Copy & paste
* Ctrl+F
* Right-click context menus
* Easy to produce tools that consume and work with it; blacklisting, gzipping, user styles, etc.
* CSS and JavaScript hooks
* Even the wimpiest, most underpowered programs can at least read it
* We can slot in whatever future web standards get cooked up
* Inspect element

#Performance

Now, unfortunately, SVG isn't the greatest drawing tool. But watch the size of this converted Robbie and Bobby strip from its original PNG to saucy SVG:

Clean linework like this is a breeze for SVG. And comics often do utilize simple, clean lines.

News will get better here; SVG2 is working on supporting variable-width strokes, which should make artwork in SVG a first-class citizen.

For bitmap artwork, separating the word balloons allows for increased tolerance of lossy compression. High-detail artwork can lower the JPEG quality and keep the balloons crisp, restricted PNG palettes can ignore the colors needed for the balloons and SFX, and gutters & empty spacing can be made with code instead of additional pixels

Remember to GZIP

This does result in more, smaller requests, so remember to keepalive the connection. Progressively render, without huge images timing out on poor connections

`bufferedRendering`?

Cache invalidation