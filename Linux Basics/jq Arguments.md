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
|
