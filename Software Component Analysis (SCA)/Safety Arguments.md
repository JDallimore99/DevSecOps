Usage: safety check [OPTIONS]
Find vulnerabilities in Python dependencies at the target provided.
|Argument|Description|
|:---|:----|
|--key TEXT|API Key for pyup.io's vulnerability database. Can be set as SAFETY_API_KEY environment variable. Default: empty|
|--db TEXT| Path to a local vulnerability database. Default: empty|
|--full-report / --short-report|Full reports include a security advisory (if available). Default: --short-report NOTE: This argument is mutually exclusive with arguments: [bare with values [True, False],output with values ['json', 'bare'], json with values [True, False]].|
|--stdin|Read input from stdin. NOTE: This argument is mutually exclusive with  arguments: [files].|
|--r, --file FILENAME|Read input from one (or multiple) requirement files. Default: empty NOTE: This argument is mutually exclusive with arguments: [stdin].|
|-i, --ignore TEXT|Ignore one (or multiple) vulnerabilities by ID. Default: empty|
|-o, --output [screen,text,json,bare]||
|-pr, --proxy-protocol [http,https]|Proxy protocol (https or http) --proxy-protocol NOTE: This argument requires the following flags  [proxy_host].|
|-ph, --proxy-host TEXT|Proxy host IP or DNS --proxy-host|
|-pp, --proxy-port INTEGER|Proxy port number --proxy-port NOTE: This argument requires the following flags [proxy_host].|
|--exit-code / --continue-on-error|Output standard exit codes. Default: --exit-code|
|--policy-file FILENAME|Define the policy file to be used|
|--audit-and-monitor / --disable-audit-and-monitor|Send results back to pyup.io for viewing on your dashboard. Requires an API key.|
|--project TEXT|Project to associate this scan with on pyup.io. Defaults to a canonicalized github style name if available, otherwise unknown|
|--save-json TEXT|Path to where output file will be placed, if the path is a directory, Safety will use safety-report.json as filename. Default: empty|
|--help|Show the help message and exit|
