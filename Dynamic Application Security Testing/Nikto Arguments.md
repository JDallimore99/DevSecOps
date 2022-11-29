|Argument|Extension (if applicable)|Description|
|:----|:---|:---|
|-ask+||Whether to ask about submitting updates|
||yes|Ask about each (default)|
||no|Don't ask, don't send|
||auto|Don't ask, just send|
|-Cgidirs+||Scan these CGI dirs: "none", "all", or values like "/cgi/ /cgi-a/"|
|-config+||Use this config file|
|-Display+||Turn on/off display outputs|
||1|Show redirects|
||2|Show cookes received|
||3|Show all 200/OK responses|
||4|Show URLs which require authentication|
||D|Debug output|
||E|Display all HTTP errors|
||P|Print progress to STDOUT|
||S|Scrub output of IPs and hostnames|
||V|Verbose output|
|-dbcheck||Check database and other key files for syntax errors|
|-evasion+|Encoding technique:||
||1|Random URI encoding (non-UTF8)|
||2|Directory self-reference (/./)|
||3|Premature URL ending|
||4|Prepend long random string|
||5|Fake parameter|
||6|TAB as request spacer|
||7|Change the case of the URL|
||8|Use windows directory seperator (\)|
||A|Use a carriage return (0x0d) as a request spacer|
||B|Use binary value 0x0b as a request spacer|
|-Format+|Save file (-o) fomrat:||
||csv|Comma-seperated-value|
||json|JSON Format|
||htm|HTML Format|
||nbe|Nessus NBE Format|
||sql|Generic SQL (see docs for schema)|
||txt|Plain text|
||xml|XML Format|
||(if not specified the format will be taken from the file extension passed to -output)||
|-Help|||
|-host+|||
|-404code|||
|-404string|||
|-id+|||
|-key+|||
|-list-plugins|||
|-maxtime+|||
|-mutate+|||
||1||
||2||
||3||
||4||
||5||
||6||
|-mutate-options|||
|-nointeractive|||
|-nolookup|||
|-nossl|||
|-no404|||
|-Option|||
|-output+|||
|-Pause+|||
|-Plugins+|||
|-port+|||
|-RSAcert+|||
|-root+|||
|-Save|||
|-ssl|||
|-Tuning+|||
||1||
||2||
||3||
||4||
||5||
||6||
||7||
||8||
||9||
||0||
||a||
||b||
||c||
||d||
||e||
||x||
|-timout+|||
|-Userdbs|||
|-useragent|||
|-until|||
|-update|||
|url+|||
|-useproxy|||
|-Version|||
|-vhost+|||
