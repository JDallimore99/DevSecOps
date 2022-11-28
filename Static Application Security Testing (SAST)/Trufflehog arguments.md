usage: trufflehog [-h] [--json] [--regex] [--rules RULES] [--allow ALLOW]
                  [--entropy DO_ENTROPY] [--since_commit SINCE_COMMIT]
                  [--max_depth MAX_DEPTH] [--branch BRANCH]
                  [-i INCLUDE_PATHS_FILE] [-x EXCLUDE_PATHS_FILE]
                  [--repo_path REPO_PATH] [--cleanup]
                  git_url

Find secrets hidden in the depths of git.
|Argument|Description|
|:---|:----|
|git_url|URL for secret searching|
|-h, --help|show the help message and exit|
|--json|Output in JSON|
|--regex|Enable high signal regex checks|
|--rules RULES|Ignore default regexes and source from JSON file|
|--allow ALLOW|Explicitly allow regexes from JSON list file|
|--entrpy DO_ENTROPY|Enable entropy checks|
|since_commit SINCE_COMMIT|Only scan from a given commit hash|
|max_depth MAX_DEPTH|The max commit depth to go back when searching for secrets|
|--branch BRANCH|Name of the branch to be scanned|
|-i INCLUDE_PATHS_FILE, --include_paths|File with regular expressions (one per line), at least one of which must match a Git object path in order for it to be scanned; lines starting with "#" are treated as comments and are ignored. If empty or not provided (default), all Git object paths are included unless otherwise excluded via --exclude_paths option|
|-x EXCLUDE_PATHS_FILE, -exclude_paths|File with regular expressions (one per line), none of which may match a Git object path in order for it to be scanned; lines starting with "#" are treated as comments and are ignored. If empty or not provided (default), no Git object paths are excluded unless effectively excluded via the --include_paths option.|
|--repo_path REPO_PATH|Path to the cloned repo. If provided, git_url will not be used|
|--cleanup|Clean up all temporay result files|
