![](https://rawgit.com/stathissideris/ditaa/master/doc/images/logo.png)

## DIagrams Through Ascii Art

ditaa is a small command-line utility written in Java, that can
convert diagrams drawn using ascii art ('drawings' that contain
characters that resemble lines like | / - ), into proper bitmap
graphics. This is best illustrated by the following example -- which
also illustrates the benefits of using ditaa in comparison to other
methods :)

```
    +--------+   +-------+    +-------+
    |        | --+ ditaa +--> |       |
    |  Text  |   +-------+    |diagram|
    |Document|   |!magic!|    |       |
    |     {d}|   |       |    |       |
    +---+----+   +-------+    +-------+
        :                         ^
        |       Lots of work      |
        +-------------------------+
```

After conversion using ditaa, the above file becomes:

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/first.png)

ditaa interprets ascci art as a series of open and closed shapes, but
it also uses special markup syntax to increase the possibilities of
shapes and symbols that can be rendered.

ditaa is open source and free software (free as in free speech), since
it is released under the GPL license.

### BUT WHY? Does this thing have any real use?

There are several reasons why I did this:

1. Simply for hack value. I wanted to know if/how it could be done and
   how easily.
2. Aesthetic reasons and legacy formats: there are several old FAQs
   with ASCII diagrams lying out there. At this time and age ascii
   diagrams make my eyes hurt due to their ugliness. ditaa can be used
   to convert them to something nicer. Although ditaa would not be
   able to convert all of them (due to differences in drawing 'style'
   in each case), it could prove useful in the effort of modernising
   some of those documents without too much effort. I also know a lot
   of people that can make an ascii diagram easily, but when it gets
   to using a diagram program, they don't do very well. Maybe this
   utility could help them make good-looking diagrams easily/quickly.
3. Embedding diagrams to text-only formats: There is a number of
   formats that are text-based (html, docbook, LaTeX, programming
   language comments), but when rendered by other software (browsers,
   interpreters, the javadoc tool etc), they can contain images as
   part of their content. If ditaa was intergrated with those tools
   (and I'm planning to do the javadoc bit myself soon), then you
   would have readable/editable diagrams within the text format
   itself, something that would make things much easier. ditaa syntax
   can currently be embedded to HTML.
4. Reusability of "code": Suppose you make a diagram in ascii art and
   you render it with version 0.6b of ditaa. You keep the ascii
   diagram, and then version 0.8 comes out, which features some new
   cool effects. You re-render your old diagram with the new version
   of ditaa, and it looks better, with zero effort! In that sense
   ditaa is a diagram markup language, with very loose syntax.

## Getting it

For Ubuntu, there is a [package](http://packages.ubuntu.com/precise/ditaa).

For MacOSX, you can install via [brew](http://brewformulas.org/Ditaa).

## Usage and syntax

### Command line

You need the latest Java runtimes (JRE) to use ditaa. The best
anti-aliasing can be achieved using Java 1.5 or higher.

To start from the command line, type (where `XXX` is the version number):

`java -jar ditaaXXX.jar`

You will be presented with the command-line options help:

```
 -A,--no-antialias          Turns anti-aliasing off.
 -d,--debug                 Renders the debug grid over the resulting
                            image.
 -E,--no-separation         Prevents the separation of common edges of
                            shapes. You can see the difference below:
```

Before processing:

```
+---------+
| cBLU    |
|         |
|    +----+
|    |cPNK|
|    |    |
+----+----+
```

Common edge separation (default)

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/separation.png)

No separation (with the `-E` option)

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/no_separation.png)

```
-e,--encoding <ENCODING>   The encoding of the input file.
 -h,--html                  In this case the input is an HTML file. The
                            contents of the <pre class="textdiagram"> tags
                            are rendered as diagrams and saved in the
                            images directory and a new HTML file is
                            produced with the appropriate <img> tags.
                            See the HTML section.
    --help                  Prints usage help.
 -o,--overwrite             If the filename of the destination image
                            already exists, an alternative name is chosen.
                            If the overwrite option is selected, the image
                            file is instead overwriten.
 -r,--round-corners         Causes all corners to be rendered as round
                            corners.
 -s,--scale <SCALE>         A natural number that determines the size of
                            the rendered image. The units are fractions of
                            the default size (2.5 renders 1.5 times bigger
                            than the default).
 -S,--no-shadows            Turns off the drop-shadow effect.
 -t,--tabs <TABS>           Tabs are normally interpreted as 8 spaces but
                            it is possible to change that using this
                            option. It is not advisable to use tabs in
                            your diagrams.
 -v,--verbose               Makes ditaa more verbose.
```

### Syntax

#### Round corners

If you use `/` and `\` to connect corners, they are rendered as round corners:

```
/--+
|  |
+--/
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/round_corner.png)

#### Color

Color codes can be used to add color to the diagrams. The syntax of
color codes is

`cXXX`

where XXX is a hex number. The first digit of the number represents
the red compoment of the color, the second digit represents green and
the third blue (good ol' RGB). See below for an example of use of
color codes:

```
/----\ /----\
|c33F| |cC02|
|    | |    |
\----/ \----/

/----\ /----\
|c1FF| |c1AB|
|    | |    |
\----/ \----/
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/color.png)

This can become a bit tedious after a while, so there are (only some
for now) human readable color codes provided:

```
Color codes
/-------------+-------------\
|cRED RED     |cBLU BLU     |
+-------------+-------------+
|cGRE GRE     |cPNK PNK     |
+-------------+-------------+
|cBLK BLK     |cYEL YEL     |
\-------------+-------------/
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/color_codes.png)

As you can see above, if a colored shape contains any text, the color
of the text is adjusted according to the underlying color. If the
undelying color is dark, the text color is changed to white (from the
default black).

Note that color codes only apply if they are within closed shapes, and
they have no effect anywhere outside.

#### Tags

ditaa recognises some tags that change the way a rectangular shape is rendered. All tags are between `{` and `}`. See the reference below:

Document - Symbol representing a document.

```
+-----+
|{d}  |
|     |
|     |
+-----+
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/document.png)


Storage - Symbol representing a form of storage, like a database or a hard disk.
```
+-----+
|{s}  |
|     |
|     |
+-----+
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/storage.png)

Input/Output - Symbol representing input/output.

```
+-----+
|{io} |
|     |
|     |
+-----+
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/io.png)

#### Dashed lines

Any lines that contain either at least one `=` (for horizontal lines)
or at least one `:` (for vertical lines) are rendered as dashed
lines. Only one of those characters can make a whole line dashed, so
this feature "spreads". The rationale behind that is that you only
have to change one character to switch from normal to dashed (and vice
versa), rather than redrawing the whole line/shape. Special symbols
(like document or storage symbols) can also be dashed. See below:

```
----+  /----\  +----+
    :  |    |  :    |
    |  |    |  |{s} |
    v  \-=--+  +----+
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/dashed_demo.png)


#### Point markers

If `*` is encountered on a line (but not at the end of the line), it
is rendered as a special marker, called the point marker (this feature
is still experimental). See below:

```
*----*
|    |      /--*
*    *      |
|    |  -*--+
*----*
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/point_marker.png)


#### Text handling

If the pattern ' o XXXXX' is encountered, where XXXXX is any text, the
'o' is interpreted and rendered as a bullet point. Note that there
must be a space before the 'o' as well as after it. See below:

```
/-----------------\
| Things to do    |
| cGRE            |
| o Cut the grass |
| o Buy jam       |
| o Fix car       |
| o Make website  |
\-----------------/
```

![](https://rawgit.com/stathissideris/ditaa/master/doc/images/bullet.png)

#### HTML mode

When ditaa is run using the `--html` option, the input is an HTML
file. The contents of the `<pre class="textdiagram">` tags are rendered
as diagrams and saved in the images directory and a new HTML file is
produced with the appropriate `<img>` tags.

If the id parameter is present in the `<pre>` tag, its value is used as
the filename of the rendered png. Otherwise a filename of the form
`ditaa_diagram_X.png` is used, where X is a number. Similarly, if there
is no output filename specified, the converted html file is named in
the form of `xxxx_processed.html`, where `xxxx` is the filename of the
original file.

In this mode, files that exist are not generated again, they are just
skipped. You can force overwrite of the files using the `--overwrite`
option.

## Friends and relatives

Here is a list of projects that are related to ditaa and add to its
functionality:

* Mikael Brännström's
  [ditaa-addons](http://ditaa-addons.sourceforge.net/) offers two very
  interesting pieces of functionality: The Textdiagram Javadoc taglet
  allows you to embedd ascii diagrams to Java comments and have them
  rendered as ditaa diagrams in the HTML output. The Ditaa Eps program
  allows you to render to EPS instead of PNG.
* Mathieu Lecarme's [ditaa-web](http://ditaa.sourceforge.net/) allows
  you to install ditaa on your web server and use it as a webservice
  through a HTTP POST request.
* [org-mode](http://orgmode.org/), a major mode for emacs comes with
  support for ditaa. Here's
  [how to use it](http://orgmode.org/worg/org-contrib/babel/languages/ob-doc-ditaa.html).
* There is a [plugin](https://www.dokuwiki.org/plugin:ditaa) for
  [DocuWiki](https://www.dokuwiki.org/dokuwiki#) that allows you to
  render ditaa diagrams in the wiki pages. Written by Dennis Ploeger.
* [Markdeep](https://casual-effects.com/markdeep/) has great support
  for generating diagrams from ASCII art among other things.

Here's a list of projects that are somehow related to ditaa, and could
prove useful/relevant while using it:

* Nadim Khemir's [asciio](http://search.cpan.org/dist/App-Asciio/)
  could be used for producing the ascii diagrams more easily. It would
  require some modifications to its default settings for the diagrams
  to render properly (specifically the characters used for corners
  should all be set to +).
* Markus Gebhard's [JavE](http://www.jave.de/) could be used for the
  same purpose, although I've never tried it.
* If you make sure to keep the lines straight,
  [artist-mode](http://cinsk.github.io//emacs/emacs-artist.html) for
  emacs can be used to produce ditaa-compatible diagrams.

## Contributors

* Stathis Sideris - original author
* Bill Baker - co-maintainer
* John Tsiombikas - beta testing
* Leonidas Tsampros - beta testing

Thanks to Steve Purcell for writing JArgs.

Thanks to Mr. Jericho for writing Jericho HTML Parser.
