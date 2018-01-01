# What's Markdown?

Markdown is an easy-to-write and easy-to-read wiki-style markup language that
lets you author web pages in plain text. Example:

``` markdown
## What's `football.db`?

A free and open public domain football database & schema
for use in any (programming) language (e.g. uses plain datasets). Example:

    ### Teams
    
    barcelona, Barcelona|FC Barcelona|Fútbol Club Barcelona, BAR
    madrid,    Real Madrid|Real Madrid CF,                   RMD
    ...
```

becomes

``` html
<h2>What's <code>football.db</code>?</h2>

<p>A free and open public domain football database &amp; schema
for use in any (programming) language (e.g. uses plain datasets). Example:</p>

<pre><code>
### Teams

barcelona, Barcelona|FC Barcelona|Fútbol Club Barcelona, BAR
madrid,    Real Madrid|Real Madrid CF,                   RMD
...
</code></pre>
```


# What's `kramdown`?

`kramdown` is a library (github: [`gettalong/kramdown`](https://github.com/gettalong/kramdown)) that
that lets you convert Markdown (`.md, .mkdwn, .markdown`) to Hypertext (`.html`).
Thanks to [Thomas Leitner](https://github.com/gettalong) from Vienna, Austria
for polishing the library - more than 30+ releases - leading to today's version 1.5.0.

## Usage

Converting your wiki-style plain text markup
to hypertext using kramdown is as easy as:

``` ruby
require 'kramdown'

Kramdown::Document.new( '¡Barça, Barça, Baaarça!' ).to_html

# => "<p>¡Barça, Barça, Baaarça!</p>\n"
```

## Let's create another static site generator in 5 minutes

First lets create a new ruby script and
let's add the `kramdown` gem
plus the `find` standard Ruby library module to help us with finding files. Example:


`./sitegen.rb`:

``` ruby
require 'kramdown'
require 'find'
```

Next let's make a `_site` output folder.

``` ruby
SITE_PATH = './_site'

Dir.mkdir( SITE_PATH ) unless File.exist?( SITE_PATH )
```

Now let's add a `markdown` converter helper method.

``` ruby
def markdown( text )
  Kramdown::Document.new( text ).to_html
end
```

Finally, to wrap up let's loop over all files in the current working folder, that is, `.`
and generate hypertext (`.html`) documents in the `./site` folder
from the markdown (`.md`) source documents.

``` ruby
Find.find('.') do |path|
  if File.extname(path) == '.md'              # e.g. ./index.md => .md
    basename = File.basename(path, '.md')     # e.g. ./index.md => index

    File.open( "#{SITE_PATH}/#{basename}.html", 'w') do |file|
      file.write markdown( File.read( path ) )
    end
  end
end
```

That's it. Create a new markdown file. Example:

`./index.md`:

``` markdown
## What's `football.db`?

A free and open public domain football database & schema
for use in any (programming) language (e.g. uses plain datasets). Example:

    ### Teams
    
    barcelona, Barcelona|FC Barcelona|Fútbol Club Barcelona, BAR
    madrid,    Real Madrid|Real Madrid CF,                   RMD
    ...
```

Run the static site generation machinery e.g. type:

``` ruby
$ ruby ./sitegen.rb
```

That's it. Open up the web page `./_site/index.html` in your browser. 



## Bonus: `kramdown` Command Line Tool

Note: The `kramdown` gem ships with a command line tool named - suprise,
suprise - `kramdown`. Type in your shell:

```
$ kramdown -h
```

resulting in:

```
Command line options:

    -i, --input ARG
          Specify the input format: kramdown (default), html, GFM or markdown
    -o, --output ARG
          Specify one or more output formats separated by commas: html (default), kramdown, latex, pdf or remove_html_tags
    -v, --version
          Show the version of kramdown
    -h, --help
          Show the help

kramdown options:

        --[no-]auto-ids
          Use automatic header ID generation
          
          If this option is `true`, ID values for all headers are automatically
          generated if no ID is explicitly specified.
          
          Default: true
          Used by: HTML/Latex converter

        --[no-]parse-block-html
          Process kramdown syntax in block HTML tags
          
          If this option is `true`, the kramdown parser processes the content of
          block HTML tags as text containing block-level elements. Since this is
          not wanted normally, the default is `false`. It is normally better to
          selectively enable kramdown processing via the markdown attribute.
          
          Default: false
          Used by: kramdown parser

        --[no-]parse-span-html
          Process kramdown syntax in span HTML tags
          
          If this option is `true`, the kramdown parser processes the content of
          span HTML tags as text containing span-level elements.
          
          Default: true
          Used by: kramdown parser

        --footnote-nr ARG
          The number of the first footnote
          
          This option can be used to specify the number that is used for the first
          footnote.
          
          Default: 1
          Used by: HTML converter

        --toc-levels ARG
          Defines the levels that are used for the table of contents
          
          The individual levels can be specified by separating them with commas
          (e.g. 1,2,3) or by using the range syntax (e.g. 1..3). Only the
          specified levels are used for the table of contents.
          
          Default: 1..6
          Used by: HTML/Latex converter

        --smart-quotes ARG
          Defines the HTML entity names or code points for smart quote output
          
          The entities identified by entity name or code point that should be
          used for, in order, a left single quote, a right single quote, a left
          double and a right double quote are specified by separating them with
          commas.
          
          Default: lsquo,rsquo,ldquo,rdquo
          Used by: HTML/Latex converter

        --header-offset ARG
          Sets the output offset for headers
          
          If this option is c (may also be negative) then a header with level n
          will be output as a header with level c+n. If c+n is lower than 1,
          level 1 will be used. If c+n is greater than 6, level 6 will be used.
          
          Default: 0
          Used by: HTML converter, Kramdown converter, Latex converter

          ...
```

Yes, it's well documented. Welcome to the wonders of `kramdown`.
