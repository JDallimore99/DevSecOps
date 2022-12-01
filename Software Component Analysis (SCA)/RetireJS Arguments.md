Usage: retire [options]
|Argument|Description|
|:---|:---|
|-h, --help|output usage information|
|-V, --version|output the version number|
|-p, --package|limit node scan to packages where parent is mentioned in package.json (ignore node_modules)|
|-n, --node|Run node dependency scan only|
|-j, --js|Run scan of JavaScript files only|
|-v, --verbose|Show identified files (by default only vulnerable files are shown)|
|-x, --dropexternal|Don't include project provided vulnerability repository|
|-c, --nocache| Don't use local cache|
|--jspath \<path \>|Folder to scan for javascript files|
|--nodepath \<path \>|Folder to scan for node files|
|--path \<path\>|Folder to scan for both|
|--jsrepo \<path/url\>|Local or internal version of repo|
|--noderepo \<path/url\>|Local or internal version of repo|
|--cachedir \<path \>|Path to use for local cache instead of /tmp/.retire-cache|
|--proxy \<url \>|Proxy url (http://some.sever:8080)|
|--outputformat \<format\>|Valid formats: text, json, jsonsimple, depcheck (experimental) and cyclonedx|
|--outputpath \<path\>|File to which output should be written|
|--ignore \<paths\>|Comma delimited list of paths to ignore|
|--ignorefile \<file\>|Custom ignore file, defaults to .retireignore / .retireignore.json|
|--severity \<level\>|Specify the bug severity level from which the process fails. Allowed levels none, low, medium, high, critical. Default: none|
|--exitwith \<code\>|Custom exit code (default: 13) when vulnerabilities are found|
|--colours|Enable color output (console output only)|
|--insecure|Enable fetching remote jsrepo/noderepo files from hosts using an insecure or self-signed SSL (TLS) certificate|
|--cacert \<path\>|Use the specified certificate file to verify the peer used for fetching remote jsrepo/noderepo files|
