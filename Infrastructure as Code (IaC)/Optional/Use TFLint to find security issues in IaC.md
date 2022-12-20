# Use TFLint to find security issues in IAC
Install TFLint
TFLint is a framework to find possible errors (like illegal instance types) for Major Cloud providers (AWS/Azure/GCP, warn about deprecated syntax, unused declarations and enforce best practices, naming conventions.
https://github.com/terraform-linters/tflint.
Install tflint on the system to perform static analysis on your IaC.
```sh
curl https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash
```
```sh
**Output
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2134  100  2134    0     0  59277      0 --:--:-- --:--:-- --:--:-- 60971
os=linux_amd64


====================================================
Looking up the latest version ...
Downloading TFLint v0.27.0
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   628  100   628    0     0   6097      0 --:--:-- --:--:-- --:--:--  6097
100  9.9M  100  9.9M    0     0  34.6M      0 --:--:-- --:--:-- --:--:-- 34.6M
Download was successfully


====================================================
Unpacking /tmp/tflint.zip ...
Archive:  /tmp/tflint.zip
  inflating: /tmp/tflint             
Installing /tmp/tflint to /usr/local/bin...
'/tmp/tflint' -> '/usr/local/bin/tflint'
tflint installed at /usr/local/bin/ successfully
Cleaning /tmp/tflint.zip and /tmp/tflint ...


====================================================
Current tflint version
TFLint version 0.27.0
```
Successfully installed tflint.
## Download vulnerable infrastructure
Clone an example IaC (terraform) repository with the following command.
```sh
git clone https://gitlab.practical-devsecops.training/pdso/terraform.git
```
Let’s cd into terraform directory.
```sh
cd terraform
```
Check the various options provided by this tool.
```sh
tflint -h
```
```sh
Usage:
  tflint [OPTIONS] [FILE or DIR...]

Application Options:
  -v, --version                                           Print TFLint version
      --langserver                                        Start language server
  -f, --format=[default|json|checkstyle|junit|compact]    Output format (default: default)
  -c, --config=FILE                                       Config file name (default: .tflint.hcl)
      --ignore-module=SOURCE                              Ignore module sources
      --enable-rule=RULE_NAME                             Enable rules from the command line
      --disable-rule=RULE_NAME                            Disable rules from the command line
      --only=RULE_NAME                                    Enable only this rule, disabling all other defaults. Can be specified multiple times
      --enable-plugin=PLUGIN_NAME                         Enable plugins from the command line
      --var-file=FILE                                     Terraform variable file name
      --var='foo=bar'                                     Set a Terraform variable
      --module                                            Inspect modules
      --force                                             Return zero exit status even if issues found
      --no-color                                          Disable colorized output
      --loglevel=[trace|debug|info|warn|error]            Change the loglevel

Help Options:
  -h, --help                                              Show this help message
```
