|Name|Description|
|:---|:---|
|-n, --no-threads|Run checks and file parsing sequentially|
|--[no-]progress|Show progres reports|
|-p, --path PATH|Specify paths to Rails application|
|-q, --[no-]quiet|Suppress informational messages|
|-z, --[no-]exit-on-warning| Exit code is non-zero if warnings found (Default)|
|--[no-]exit-on-error|Exit code is non-zero if errors raised (Default)|
|--ensure-latest|Fail when Brakeman is outdated|
|--ensure-ignore-notes|Fail when an ignored warnings does not include a note|
|-3, --rails3|Force rails 3 mode|
|-4, --rails4|Force rails 4 mode|
|-5, --rails5|Force rails 5 mode|
|-6, --rails6|Force rails 6 mode|
|-7, --rails7|Force rails 7 mode|

Scanning options
|Name|Description|
|:---|:---|
|-A, --run-all-checks|Run all default and optional checks|
|-a, --[no-]assume-routes|Assume all controller methods are actions (Default)|
|-e, --escape-html|Escape HTML by default|
|--faster|Faster, but less accurate scan|
|--ignore-model-output|Consider model attributes XSS-safe|
|--ignore-protected|Consider models with attr_protected safe|
|--[no-]index-libs|Add libraries to call index (Default)|
|--interprocedural|Process method calls to known methods|
|--no-branching|Disable flow sensitivity on conditionals|
|--branch-limit LIMIT|Limit depth of values in branches (-1 for no limit)|
|--parser-timeout SECONDS|Set parse timeout (Default: 10)|
|-r, --report-direct|Only report direct use of untrusted data|
|-s meth1,meth2,etc, --safe-methods|Set methods as safe for unescaped output in views|
|--sql-safe-methods meth1,meth2,etc|Do not warn of SQL if the input is wrapped in a safe method|
|--url-safe-methods method1,method2,etc|Do not warn of XSS if the link_to href parameter is wrapped in a safe method|
|--skip-files file1,path2,etc|Skip processing of these files/directories. Directories are application relative and must end in "/"|
|--only-file file1,path2,etc|Process only these files/directories. Directories are application relative and must end in "/"|
|--[no-]skip-vendor|Skip processing vendor directory (Default)|
|--skip-libs|Skip processing lib directory|
|--add-libs-path path1,path2,etc|An application relative lib directory (ex. app/mailers) to process|
|--add-engines-path path1,path2,etc|Include these engines in the scan|
|-E, --enable Check1,Check2,etc|Enable the specified checks|
|-t, --test Check1,Check2,etc|Only run the specified checks|
|-x, --except Check1,Check2,etc|Skip the specified checks|
|--add-checks-path path1,path2,etc|A directory containing additional out-of-tree checks to run|

Output Options:
|Name|Description|
|:---|:---|
|-d, --debug||
|-f, --format TYPE||
|--css-file CSSFile||
|-i, --ignore-config IGNOREFILE||
|-I, --interactive-ignore||
|l, --[no-]combine-locations||
|--[no-]highlights||
|--[no-]color||
|-m, ---routes||
|--message-limit LENGTH||
|--[no-]pager||
|--table-width WIDTH||
|-o, --output FILE||
|--[no-]seperate-models||
|--[no-]summary||
|--absolute-paths||
|--github-repo USER/REPO[/PATH][@REF]||
|--text-fields fields1,fields2,etc||
|-w, --confidence-level LEVEL||
|--compare FILE||

Configuration Files:
|Name|Description|
|:---|:---|
|-c, --cinfig-file FILE||
|-C, --create-config [FILE]||
|--allow-check-paths-in-config||
|-k, --checks||
|--optional-checks||
|-v, --version||
|--force-scan||
|-h, --help||
