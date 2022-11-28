Usage: safety check [OPTIONS]

  Find vulnerabilities in Python dependencies at the target provided.
|Argument|Description|
|:---|:----|
|--key TEXT|API Key for pyup.io's vulnerability database. Can be set as SAFETY_API_KEY environment variable. Default: empty|
|--db TEXT| Path to a local vulnerability database. Default: empty|
|--full-report / --short-report|Full reports include a security advisory (if available). Default: --short-report NOTE: This argument is mutually exclusive with arguments: [bare with values [True, False],output with values ['json', 'bare'], json with values [True, False]].|
|--stdin||
|--r, --file FILENAME||
|-i, --ignore TEXT||
|-o, --output [screen,text,json,bare]||
|-pr, --proxy-protocol [http,https]||
|-ph, --proxy-host TEXT||
|-pp, --proxy-port INTEGER||
|--exit-code / --continue-on-error||
|--policy-file FILENAME ||
|--audit-and-monitor / --disable-audit-and-monitor||
|--project TEXT||
|--save-json TEXT||
|--help||
