#Accessible comics on the internet#

Webcomics have been, historically, rubbish at accessibility. Absolute trash. The blind can't enjoy them at all, the visually impaired and those with reading disabilities don't have a way of double-checking what a bit of hard-to-read font says, and even Google hasn't the foggiest what your comic is actually saying in all that text inside the image.

This is pathetic, but it does have a silver lining: it is ridiculously easy to improve it. Even if you make a mistake, it literally cannot become worse; it's all up from here.

##`alt` text##

The code that displays images in web pages looks like this:

````html
<img src="/link/to/image/file.png" alt="alternative text">
````

The `src=` bit points at where the image file is hosted on the internet. But what does `alt=` do?

If you have good eyesight, and your browser can display the image, and you successfully downloaded the file, the effect of `alt` is invisible. But in case any of those things aren't true, it says "display this text as an alternative to the image."

This gives people with visual and/or cognitive disabilities a lifeline. The most common use is software called a "screen-reader," which does what it sounds like. When it finds an image coded like this:

````html
<img src="vacation_photo.jpg" alt="My dad with the worst sunburn I've ever seen in my life.">
````

It reads out loud something like "Image: My dad with the worst sunburn I've ever seen in my life." If you *didn't* have any `alt` text, it tries to read the filename instead, which wouldn't be too bad in this case ("Image: vacation underscore photo period jay-pee-gee"), but a **lot** of image URLs nowadays are horrible:

````html
<img src="http://40.media.tumblr.com/851db59d0609a1cf52ea79b4090b875f/tumblr_my8q3zLG7f1rrfdb5o1_1280.jpg">
````

Can you imagine what that would sound like? I just have to add `alt` text like this:

````html
<img src="http://40.media.tumblr.com/851db59d0609a1cf52ea79b4090b875f/tumblr_my8q3zLG7f1rrfdb5o1_1280.jpg" alt="A skeleton, wearing Mickey Mouse-pants and holding a wine bottle, laughs with a ghost who is wearing a lampshade and holding a corkscrew.">
````

And we just saved screen-readers from a gibberish hell.

And that's not all that `alt` can do:

* Some use a device that turns web pages into Braille, which can't do anything useful if it finds images without `alt`.
* Users with cognitive disabilities can use screen-readers to help explain images to them.
* Users with *some* vision also use `alt` text to clarify what they're looking at. They might be able to pick out that the image is of a person, or a face, but they'd appreciate knowing if the image is supposed to be showing them something specific.
* Search engines sure as heck can't see, so they use the alt text to figure out what images are about.
* If a user has an unreliable internet connection and it fails to download the image, or their browser doesn't support the image file, or the image file breaks or is moved, they get the `alt` text instead. Not as good, but definitely better than nothing.
* Users who pay money for every byte they download (like people in countries with developing Internet infrastructure, also known as "most of the world") often turn images off by default and choose which ones they want to download. For them, `alt` text can be good enough, and it lets them decide which images *are* worth downloading. For example, if you had an icon that linked to your Twitter page, using `alt="my Twitter page"` would get the job done just as well.

`alt` has a lot of power for just a little bit of effort. There's good and bad ways of writing it, and [you should do a little reading about them](http://webaim.org/techniques/alttext/#intro) if you make websites. But for here, we're concerned with how to best use `alt` for webcomics.

The first guess would probably be putting a transcription of the comic inside `alt`. However, you shouldn't.

* Spammers have often tried to use `alt` to trick search engines, by stuffing in as many words as they can that they think people will search for. Search engines nowadays punish you if your `alt` text gets too long.
* Too much text inside `alt` is impossible to navigate well for screen-readers and Braille displays: if they want to reread a specific part of it, they have to start from the beginning again, since there's no way of breaking it up into smaller bits; you can't use more HTML inside of `alt`.
* `alt` text is *not a description of the image*. It's an alternative, which is a subtle but important difference. For most images, that means something like `alt="The Prime Minister of China signs the treaty"`, instead of describing his suit, hair, what pen he's using, etc. The context of the image and the point of including it are more important than what it's of.

How long is too long? There isn't a hard number, but a good rule of thumb is to try fitting it into a tweet. How much useful information can you put into the 140 characters?

````html
<img src="http://www.paranatural.net/comics/1420797323-Ch5Pg4.png" alt="Dr. Zarei treats one of Francisco's students, who has been bitten by a spirit. Isabel overhears and is interested by the conversation.">
````

A short, simple summary, which lets users quickly learn which comic this is (if they're looking for a specific one, for example), gives search engines information if somebody searches for something like "Paranatural Dr. Zarei", and prevents screen-readers from struggling to pronounce that ugly URL. Sometimes you won't be able to fit a good summary into just 140 characters, and that's okay! Users and search engines won't suddenly hate you if your summary really needs to be 159 characters long. Just aim for the ballpark.

<cite><a href="http://www.paranatural.net/">Paranatural</a></cite> has the page and chapter number elsewhere on the page, but for a comic like <cite><a href="http://www.casualvillain.com/Unsounded/comic/ch01/ch01_01.html">Unsounded</a></cite>, where it has a very simple page for reading, it would be nice to include that information:

````html
<img src="pageart/ch01_01.jpg" alt="Start of Chapter 1: A Reluctant Escort">
````

This is a huge improvement already, because neither comics have any `alt` text to speak of at all. (This isn't the fault of the authors: no comic publishing system I know of gives them the ability to write it.)

But that's not quite an accessible comic. What if somebody wants to know what, exactly, Isabel overheard, or who's depicted in that chapter frontispiece?  `alt` may be unsuitable for description of the image, but we still need to have it *somewhere!*.

##`longdesc` to the rescue?##

There is another attribute we can add to the image code called `longdesc`. When used correctly, it looks something like this:

````html
<img src="link/to/image/file.png" alt="A short alternative to the image" longdesc="link/to/image/description.html">
````

However, `longdesc` [has been proven to be a flawed idea](http://terrillthompson.com/blog/18), and [is almost never used correctly as a result](https://blog.whatwg.org/the-longdesc-lottery).

There are existing solutions: [Ryan North](https://twitter.com/ryanqnorth)'s [OhNoRobot has been crowdsourcing transcripts](http://www.ohnorobot.com/) for years, and all the major WordPress comic plugins that have been ([Comic Easel](http://comiceasel.com/), [ComicPress](http://comicpress.org/), [Webcomic](http://webcomic.nu/)) let authors and/or readers write their own.

These are a good step in the right direction, but they have some ways to go. Too often, the transcripts are buried under lots of other information in the source order, so most users who would need a transcript don't have a good chance of finding it. Some of them are only plain text, which is better than nothing, but not great. And even the markup ones are produced with a WYSIWYG generator, which I would say is worse than just plain text. And OhNoRobot's is hidden in a link to a different domain entirely.
