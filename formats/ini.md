# INI

Use the `iniparser` library to read / parse INI configuration, setttings and data files into a hash.

## Usage - `INI.load`, `INI.load_file`


Opt 1) `INI.load` - load from string. Example:

``` ruby
text   = File.read( './planet.ini' )
hash = INI.load( text )
```

Opt 2) `INI.load_file` - load from file (shortcut). Example:

``` ruby
hash = INI.load_file( './planet.ini' )
```


All together now. Example:

``` ruby
require 'iniparser'

text = <<TXT
# comment
; another comment

key1 = hello
key2 : hi!

[section1]
key3 = salut   # end of line comment here

[section2]
key4: hola
blank =
blank2:

[ http://example.com ]
title =  A rose is a rose is a rose, eh?
title2:  A rose is a rose is a rose, eh?   # comment here
; another one here
title3 = A rose is a rose is a rose, eh?
TXT

hash = INI.load( text )
pp hash
```

resulting in:

``` ruby
{"key1"=>"hello",
 "key2"=>"hi!",
 "section1"=>{"key3"=>"salut"},
 "section2"=>{"key4"=>"hola", "blank"=>"", "blank2"=>""},
 "http://example.com"=>
  {"title"=>"A rose is a rose is a rose, eh?",
   "title2"=>"A rose is a rose is a rose, eh?",
   "title3"=>"A rose is a rose is a rose, eh?"}}
```

to access use:

``` ruby
puts hash['key1']
# => 'hello'
puts hash['key2']
# => 'hi!'
puts hash['section1']['key3']
# => 'salut'
puts hash['section2']['key4']
# => 'hola'
puts hash['section2']['blank']
# => ''
puts hash['section2']['blank2']
# => ''      
puts hash['http://example.com']['title']
# => 'A rose is a rose is a rose, eh?'
puts hash['http://example.com']['title2']
# => 'A rose is a rose is a rose, eh?'
puts hash['http://example.com']['title3']
# => 'A rose is a rose is a rose, eh?'
```


## References

- [iniparser library @ GitHub](https://github.com/datatxt/iniparser)
- [INI @ Wikipedia](https://en.wikipedia.org/wiki/INI_file)
