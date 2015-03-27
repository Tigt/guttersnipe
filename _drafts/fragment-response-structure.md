The plugin generates a `.json` snippet that only contains the data to replace the current content with.

It's named like this:

<pre>
  dir/
    |- page-name.html
    \- page-name.fragment.json
</pre>

It looks like this:

```json
{
    "title": "The title.",
    "fragment": "<article id=\"...\"> <!-- and so on --> </article>",
    "scripts": [
        "/path/to/script.js",
        "/path/to/other-script.js",
        "etc"
    ]
}
```

The idea is that you `XMLHttpRequest` for the fragment, `JSON.parse` it, and then just `innerHTML` the `"fragment"` where you want to put it. Replace the `<title>` with `"title"`, natch.

In the event you somehow need to bring in new metadata other than `<title>`, you have my condolences. Go ahead and add some `"theme-color"` or `"og:garbage"` if you need to.

Any styles specific to the new page should be included inside `"fragment"`'s payload, since you're injecting it anyway and HTML5 couldn't care less about where you do it.

The `"scripts"` array just defines a bunch of URLs to download any specific JS with. Ideally you won't have (m)any of these.
