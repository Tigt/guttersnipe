---
title: Accessible comics
description: Webcomics have historically been completely inaccessible, but that can change.
---
#Accessible comics#

Webcomics have been, historically, rubbish at accessibility. Absolute trash. You're lucky if you get an empty `alt` attribute.

This is pathetic, but it does have a consolation: it is ridiculously easy to improve it. Even if we make a mistake, it literally cannot become worse; it's all up from here.

##`alt` text##

The first crack would probably be putting a transcription of the comic inside `alt`. However, you shouldn't.

* Search engines nowadays punish you if your `alt` text gets too long, because those SEO assholes made Google hate us for cramming keywords into there.
* Too much text inside `alt` is impossible to navigate well for screen-readers and Braille displays: if they want to reread a specific part of it, they have to start from the beginning again, since there's no way of breaking it up into smaller bits. You can't use HTML inside of `alt`, basically.
* `alt` text is *not a description of the image*. It's an alternative, which is a subtle but important difference. For most images, that means something like `alt="The Prime Minister of China signs the treaty"`, instead of describing his suit, hair, what pen he's using, etc. The context of the image and the point of including it are more important than what it's of.

How long is too long? There isn't a hard number, but a good rule of thumb is to try fitting it into a tweet. How much useful information can you put into the 140 characters?

{% highlight html %}
<img src="http://www.paranatural.net/comics/1420797323-Ch5Pg4.png" alt="Dr. Zarei treats one of Francisco's students, who has been bitten by a spirit. Isabel overhears and is interested by the conversation.">
{% endhighlight %}

A short, simple summary, which lets users quickly learn which comic this is (if they're looking for a specific one, for example), gives search engines information if somebody searches for something like "<kbd>Paranatural Dr. Zarei</kbd>", and prevents screen-readers from struggling to pronounce that ugly URL. Sometimes you won't be able to fit a good summary into just 140 characters, and that's okay! Users and search engines won't suddenly hate you if your summary really needs to be 159 characters long. Just aim for the ballpark.

<cite><a href="http://www.paranatural.net/">Paranatural</a></cite> has the page and chapter number elsewhere on the page, but for a comic like <cite><a href="http://www.casualvillain.com/Unsounded/comic/ch01/ch01_01.html">Unsounded</a></cite>, where it has a very simple page for reading, it would be nice to include that information:

{% highlight html %}
<img src="pageart/ch01_01.jpg" alt="Start of Chapter 1: A Reluctant Escort">
{% endhighlight %}

This is a huge improvement already, because neither comics have any `alt` text at all. This isn't the fault of the authors; their comic publishing systems don't give them the ability to write any.

Of course that would only be half the battle. Even with a capable CMS, we'd still need to encourage comic authors to actually write the thing. The good news is we could ask for a few-sentences summary that would be used when cross-posting to social media, assembled with missing comic metadata for `alt`, and thrown into `<meta name="description">`. We'd want something that invisibly drives authors to write good `alt` text without knowing it.

But that's not quite an accessible comic. What if somebody wants to know what, exactly, Isabel overheard, or who's depicted in that chapter frontispiece?  `alt` may be unsuitable for description of the image, but we still need to have it *somewhere!*.

##`longdesc` to the rescue?##

Well, no. We know `longdesc` has problems. However, we might as well strike a blow for justice:

{% highlight html %}
<img src="comic.png" alt="The Adventures of Popeye, #1: Popeye looks around for some spinach." longdesc="#transcript">
{% endhighlight %}

I know as of HTML5 `longdesc` is supposed to be able to take an `id` like the above, but I can't figure out what the support for doing so is like. We might need `http://the.current/page/the/image/is/on#transcript`, or have the CMS generate a standalone page (blegh). We might be better served with something like this:

{% highlight html %}
<img src="comic.png"
     alt="The Adventures of Popeye, #1: Popeye looks around for some spinach."
     longdesc="#transcript"
     aria-describedby="transcript">
{% endhighlight %}

Or if support of the dual accessibility attributes isn't good enough, we can figure out how to make regular old `<a href="#transcript">` links that aren't ugly enough to make authors want to remove them.

This, of course, assumes we have an actual transcript to point at. I'm pretty sure we want it on the same page as the comic. It gives search engines something to actually look for (finding specific comics in an archive is notoriously difficult in webcomics), second is that it doesn't ask users who need the long description to open a new URL/tab/window (and imagine if it was an offline copy!). Most webcomic transcript plugins do just this, so I'm not alone in this opinion.

##Transcripts##

Something like the following would be nice:

{% highlight html %}
<img src="http://www.beeserker.com/comics/2010-04-03-beeserker.png"
     alt="The Sciencemen spitball ideas for their next science project."
     longdesc="#transcript"
     aria-describedby="transcript">

<aside id="transcript">
    <section id="panel-1">
        <p><i>The sciencemen brainstorm in the lab…</i></p>
        <p><q><cite>First Scienceman:</cite> We should make a robot out of bees…</q></p>
    </section>
    <section id="panel-2">
        <p><q><cite>First Scienceman:</cite> …or perhaps make the robot powered by bees.</q></p>
        <p><q><cite>Second Scienceman:</cite> Better yet!</q></p>
    </section>
    <section id="panel-3">
        <p><i>The second scienceman grasps the shoulder of the first.</i></p>
        <p><q><cite>Second Scienceman:</cite> <em>LET'S MAKE A BEE OUT OF POWER</em></q></p>
    </section>
    <section id="panel-4">
        <p><i>Later!</i></p>
        <p><i>The sciencemen lay dead on the ground as an enormous bee composed of power darkens the sky with wrath.</i></p>
    </section>
</aside>
{% endhighlight %}

Suggestions for marking up a document as simple as a script with HTML's sad vocabulary are appreciated. I'm trying to use `<i>`'s suggested use as "alternative voice" for all it's worth.

I'd love to be able to do something like this instead:

{% highlight html %}
<object type="image/png" data="http://www.beeserker.com/comics/2010-04-03-beeserker.png">
    <section id="panel-1">
        <p><i>The sciencemen brainstorm in the lab…</i></p>
        <p><q><cite>First Scienceman:</cite> We should make a robot out of bees…</q></p>
    </section>
    <section id="panel-2">
        <p><q><cite>First Scienceman:</cite> …or perhaps make the robot powered by bees.</q></p>
        <p><q><cite>Second Scienceman:</cite> Better yet!</q></p>
    </section>
    <section id="panel-3">
        <p><i>The second scienceman grasps the shoulder of the first.</i></p>
        <p><q><cite>Second Scienceman:</cite> <em>LET'S MAKE A BEE OUT OF POWER</em></q></p>
    </section>
    <section id="panel-4">
        <p><i>Later!</i></p>
        <p><i>The sciencemen lay dead on the ground as the enormous bee composed of power darkens the sky with wrath.</i></p>
    </section>
</object>
{% endhighlight %}

But if it were this simple, it would be common practice… right? I've seen [Tantek Celik use this for comics](https://indiewebcamp.com/Falcon#comics), but his intention was to use the enclosed markup for metadata, so I can't be sure this works for accessibility. (Really, everything about `<object>` never did work out like it was supposed to.)

There are existing solutions: [OhNoRobot has been crowdsourcing transcripts](http://www.ohnorobot.com/) for years, and all the major WordPress comic plugins that have been ([Comic Easel](http://comiceasel.com/), [ComicPress](http://comicpress.org/), [Webcomic](http://webcomic.nu/)) let authors and/or readers write their own.

These are a good step in the right direction, but they have some ways to go. Some of them are only plain text, which is better than nothing, but not great. And even the markup ones are produced with a WYSIWYG generator, which I would say is worse than just plain text. And OhNoRobot's is hidden in a link to a different domain entirely.

Ideally we'd have a dedicated interface to take care of the markup for you. Something like this?

<form>
    <legend>Add comic transcript</legend>
    <label>Character: <input></label>
    <label>Dialogue: <input></label>
    <button>Add new line</button>
    <button>Add new panel</button>
    <p>Tip: Use *asterisks* for *italic* and **bold**</p>
</form>

Of course, this was all a bit of an academic exercise, because nobody writes transcripts. You'd have to pay them.

In the future, where cars fly and fusion is cold, your phone could just OCR whatever you point it at. But until then, we'll have to figure out how to make comics accessible by default.

##Using real text in comics##

Ideally, we'd be able to just use some fancy CSS to transform a pile of markup into comics. This would work wonderfully for "accessible by default," but HTML wasn't designed for visual consistency. Bleeding edge CSS modules such as Regions and Shapes might work, but to have comics that work *now*, HTMl is unsuitable.

But SVG is quite suitable: it's a language built for visual consistency and positioning. It'll take some work and bolted-on ARIA to get things working smoothly, but SVG might just be the future of webcomics; even the raster ones.