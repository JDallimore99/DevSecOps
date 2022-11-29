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
|-A, --run-all-checks||
|-a, --[no-]assume-routes||
|-e, --escape-html||
|--faster||
|--ignore-model-output||
|--ignore-protected||
|--[no-]index-libs||
|--interprocedural||
|--no-branching||
|--branch-limit LIMIT||
|--parser-timeout SECONDS||
|-r, --report-direct||
|-s meth1,meth2,etc,||
|--safe-methods||
|--sql-safe-methods meth1,meth2,etc||
|--url-safe-methods method1,method2,etc||
|--skip-files file1,path2,etc||
|--only-file file1,path2,etc||
|--[no-]skip-vendor||
|--skip-libs||
|--add-libs-path path1,path2,etc||
|--add-engines-path path1,path2,etc||
|-E, --enable Check1,Check2,etc||
|-t, --test Check1,Check2,etc||
|-x, --except Check1,Check2,etc||
|--add-checks-path path1,path2,etc||

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
|||
|||
|||
|||
|||
|||
|||
|||
|||
