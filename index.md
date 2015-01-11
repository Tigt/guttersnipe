Guttersnipe
===========

* Gutterpipe (`_|`)
* `_snipe`
* Gutterwright
* `{gutter:snipe;}`
* `<gutter-snipe />`
* Guttertype (for SVG text fx, guidelines, etc.)
* Gutterswipe (touch controls...?)
* Gutterball

Guttersnipe is a bunch of helpers for publishing comics on the internet better than it's been done before. You can use as many or as few of these helpers as you wish.

* A JavaScript object: `_snipe`, which performs a great deal of little things like keyboard & touch navigation, SVG manipulation, AJAXing in comics to skip style recalc, etc.

* A SVG style guide and vocabulary for semantically-rich comics
** Guttersnipe's Comicana: use &lsaquo; and &rsaquo; for translated speech, etc.

* An HTML template & CSS framework for responsive, user-friendly reading

* A static-site generator plugin for assembling the above

* A bunch of documentation for best practices, tips, and other information for publishing comics on the internet

Guttersnipe supports all HTML5-capable browsers, and also IE8. I wish we didn't have to, but as it turns out, most webcomics are read at work. Most IE8 is also at work. The SVG fallbacks are good old .PNGs, and the layout is done with CSS2.1 technologies (most notably CSS table display properties).

I wouldn't exactly call HTML table layout algorithms "sophisticated," but at least they *exist*. Until flexbox becomes standard practice, it's literally the only layout math browsers support. (Floats etc. were not intended for layout, and it shows.) All other attempts at vertical centering had either browser support issues, unfortunate side effects, or both (SVGs and 2D transforms really shouldn't mix if not absolutely required, as it turns out). And using flexbox's grandaddy is pretty painless.

````
.container {
  display: table;
  table-layout: fixed;
  empty-cells: hide;
  border-collapse: collapse;
  border-spacing: 0; /*these properties may not be strictly necessary*/
}

.item {
  display: table-cell;
  vertical-align: middle;
}
````

Seems kind of familiar, doesn't it?

Make sure to *never remove* `table-layout: fixed`, as that will make behavior impossible to predict across browsers and reduce performance. (You really don't want the automatic layout to happen, anyway.)

A tip: HTML table columns can be styled with the `<col>` and `<colgroup>` elements, which don't really accept content. I can't imagine you have a whole lot of void elements floating around on your page that you could press into service, so consider using `:before` and `:after` to fake some up.

The only gotcha is absolute positioning inside `display: table` in Firefox 30 & below (a 12-year-old bug!), but you can use wrapper elements to get around that. The way we use it works around even that, though: http://davidwalsh.name/table-cell-position-absolute
