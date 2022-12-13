# Purpose and Location
The purpose of each tool used in Pipeline and the location it is found in both the pipeline and Chapter
## Software Component Analysis 
### Safety
**Chapter 5.1**
Safety checks your installed dependencies for known security vulnerabilities.
By default, it uses the open Python vulnerability database Safety DB but can be upgraded to use pyup.io’s Safety API using the –key option.
You can find more details about the project at Safety: https://github.com/pyupio/safety
### Pip-audit
**Chapter 5.7**
The pip-audit is a tool for scanning Python environments for packages with known vulnerabilities. It uses the Python Packaging Advisory Database (https://github.com/pypa/advisory-db) via the PyPI JSON API as a source of vulnerability reports.
Source: https://github.com/trailofbits/pip-audit
### RetireJS
**Chapter 5.8**
There are lots of JavaScript libraries for use on the Web and in Node.JS apps. The availability of these libraries greatly simplifies development, but also increases the security risks. Because of rampant abuse of these libraries, attackers are concentrating on these libraries. Realizing the need for awareness, OWASP has included “Using Components with Known Vulnerabilities” as part of the OWASP Top 10 list.
RetireJS helps you in finding the insecure Javascript libraries in your code.
Source: https://github.com/retirejs/retire.js
### 
