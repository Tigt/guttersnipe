# Comarxdown #

A better name would be nice, but that's later.

"cmxd" is inspired by Markdown, as an easy-to-write and human-readable plaintext format for comics.

It also steals liberally from [existing comic script convention](http://www.comicbookscriptarchive.com/archive/panel-1/how-to-format-a-comic-script/), the [Fountain comic template](http://antonyjohnston.com/articles/pix/fountain/aj_fountain_comics_template.fountain), and IRC conventions.

It serves as the "canonical representation" for Gutterwright's handling of comic scripts, intended to be easily transformed between the "comic template" that is generated when you feed a script into Gutterwright, and the HTML "script view" in another pane.

## YAML Front Matter & Metadata ##

Well, you tend to want stuff like this.

```yaml
---
title: Title Goes Here
writing:
    - Writer Name A
    - Writer Name B # If you have just a single writer it can stand alone without the listing
art:
    - letters: Letterer Name # These can also take lists if you have more than one of each
    - pencils: Penciller Name
    - inks: Inker Name
    - colors:
        - Colorist Name A
        - Colorist Name B
isbn: haha as if
# other stuff from the RDF/schema.org suggestions
---
```

## Panels & Panelsets ##

Panelsets are delimited like this:

```cmxd
====================================

<!-- contents of a panelset -->

====================================
```

And panels further subdivide:

```cmxd
====================================
# 
====================================
```

You can just type 3 of the appropriate `===` and `---` characters, but full-width formatting is nice.

Inside 

## Word Balloons ##

```cmxd
CHARACTER:
Dialogue!
```

You can influence the type of word balloon by adding verbs:

```cmxd
CHARACTER shouts:
Dialogue!
````

Possible verbs are:

* shouts
* thinks
* transmits
* realizes

You can "connect" balloons like this:

```cmxd
CHARACTER thinks:
Wait, hold on here...
 |
What just happened?
```
