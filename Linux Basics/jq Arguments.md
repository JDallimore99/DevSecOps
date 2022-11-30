jq - commandline JSON processor [version 1.6]

Usage:  jq [options] <jq filter> [file...]
  jq [options] --args <jq filter> [strings...]
  jq [options] --jsonargs <jq filter> [JSON_TEXTS...]

jq is a tool for processing JSON inputs, applying the given filter to
its JSON text inputs and producing the filter's results as JSON on
standard output.

The simplest filter is .(dot), which copies jq's input to its output
unmodified (except for formatting, but note that IEEE754 is used
for number representation internally, with all that implies).

For more advanced filters see the jq(1) manpage ("man jq")
and/or https://stedolan.github.io/jq

Example:

  $ echo '{"foo": 0}' | jq .
  {
    "foo": 0
  }
|Argument|Description|
|:---|:----|
|-c|compact instead of pretty-printed output|
|-n|use 'null' as the single input value|
|-e|set the exit status code based on the output|
|-s|read (slurp) all inputs into an array; apply filter to it;|
|-r|output raw strings, not JSON texts;|
|-R|read raw strings, not JSON texts;|
|-C|colorize JSON|
|-M|monochrome (don't colorize JSON)
|-S|sort keys of objects on output|
|--tab|use tabs for indentation|
|-- arg a v|set variable $a to value <v>|
|--slurpfile a f|set variable $a to an array of JSON texts read from <f>|
|--rawfile a f|set variable $a to a string consisting of the contents of the <f>|
|--args|remaining arguments are string arguments, not files|
|--jsonargs|remaining arguments are JSON arguments, not files|
|--|terminates argument processing|
  
Named arguments are also available as $ARGS.named[], while
positional arguments are available as $ARGS.positional[].

See the manpage for more options.