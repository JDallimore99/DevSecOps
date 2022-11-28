usage: bandit [-h] [-r] [-a {file,vuln}] [-n CONTEXT_LINES] [-c CONFIG_FILE] [-p PROFILE] [-t TESTS] [-s SKIPS]
              [-l | --severity-level {all,low,medium,high}] [-i | --confidence-level {all,low,medium,high}]
              [-f {csv,custom,html,json,screen,txt,xml,yaml}] [--msg-template MSG_TEMPLATE] [-o [OUTPUT_FILE]] [-v]
              [-d] [-q] [--ignore-nosec] [-x EXCLUDED_PATHS] [-b BASELINE] [--ini INI_PATH] [--exit-zero] [--version]
              [targets [targets ...]]
           
|Argument|Description|
|:---|:----|
|targets|source file(s) or directory(s) to be tested|
|-h, --help|show help message and exit|
|-r, --recursive|find and process files in subdirectories|
|-a {file,vuln}, --aggregate {file,vuln}|aggregate output by vulnerability (default) or by filename|
|-n CONTEXT_LINES, --number CONTEXT LINES|maximum number of code lines to output for each issue|
|-c CONFIG_FILE, --configfile CONFIG_FILE|optional config file to use for selecting plugins and overriding defaults|
|-p PROFILE, --profile PROFILE|profile to use (defaults to executing all tests)|
|-t TESTS, --tests TESTS|comma-separated list of test IDs to run|
|-s SKIPS, --skip SKIPS|comma-separated list of test IDs to skip|
|-l, --level|report only issues of a given severity level or higher (-l for LOW, -ll for MEDIUM, -lll for HIGH)|
|--severity-level {all,low,medium,high}| report only issues of a given severity level or higher. "all" and "low" are likely to produce the same results, but it is possible for rules to be undefined which will not be listed in "low".|
|-i, --confidence|report only issues of a given confidence level or higher (-i for LOW, -ii for MEDIUM, -iii for HIGH)|
|--confidence-level {all,low,medium,high|report only issues of a given confidence level or higher. "all" and "low" are likely to produce the same results, but it is possible for rules to be undefined which will not be listed in "low".|
|-f [csv,custom,html,json,screen,txt,xml,yaml}, --format {csv,custom,html,json,screen,txt,xml,yaml}|specify output format|
|--msg-template MSG_TEMPLATE|specify output message template (only usable with --format custom), see CUSTOM FORMAT section for list of available values|
|-o [OUTPUT_FILE], --output [OUTPUT_FILE]||
|-v, --verbose||
|-d, --debug||
|-q, --quiet, --silent||
|--ignore-nosec||
|-x EXCLUDED_PATHS, --exclude EXCLUDED_PATHS||
|-b BASELINE, --baseline BASELINE||
|--ini INI-PATHS=||
|--exit-zero||
|--version||
