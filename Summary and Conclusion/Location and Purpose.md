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

### Dependency-Check
**Chapter 5.13**  
Dependency-Check is SCA tool that attempts to detect publicly disclosed vulnerabilities contained within a project’s dependencies.
You can find more details about the project at https://github.com/jeremylong/DependencyCheck.

### Snyk 
**Chapter 5.17**  
Dependency analysis tool or SCA is used to find vulnerabilities in open source dependencies and it supports multiple programming languages such as Ruby, Python, NodeJS, Java, Go, .NET. It’s easy to use, a developer can use it on his/her machine using the command line or DevOps team can embed it into CI/CD pipeline. (Source: https://snyk.io)
You can find more details about the project at https://github.com/snyk/snyk.

### NPM audit
**Chapter 5.21**  
NPM has security build in since npm@6, this command allow you to analyze dependencies in your existing code to identify insecure dependencies, have a good security report, and also easy to implement it into CI/CD pipeline without install any packages except NPM itself.

### AuditJS
**Chapter 5.22**  
AuditJS Audits an NPM package.json file to identify known vulnerabilities. It uses the OSS Index v3 REST API from Sonatype to identify known vulnerabilities and outdated package versions.

### bundler-audit
**Chapter 5.24**  
Bundler-audit provides rake tasks for checking the code and for updating its vulnerability database

### chelsea
**Chapter 5.28**  
Chelsea is a CLI application written in Ruby, designed to allow you to scan your RubyGem powered projects and report on any vulnerabilities in your third party dependencies. It is powered by Sonatype’s OSS Index.

## Static Application Security Testing (SAST)
### TruffleHog
**Chapter 6.1**  
TruflleHog is a tool that searches through git repositories for secrets, digging deep into commit history and branches. This tool is useful in finding the secrets accidentally committed to the repo.
You can find more details about the project at https://github.com/dxa4481/truffleHog.

## Bandit
**Chapter 6.8**  
The Bandit is a tool designed to find common security issues in Python code.
To do this Bandit, processes each file, builds an AST, and runs appropriate plugins against the AST nodes. Once Bandit has finished scanning all the files it generates a report.
Bandit was originally developed within the OpenStack Security Project and later rehomed to PyCQA.
You can find more details about the project at https://github.com/PyCQA/bandit.

## FindSecBugs
**Chapter 6.23**  
The SpotBugs plugin for security audits of Java web applications and Android applications.
You can find more details about the project at https://github.com/find-sec-bugs/find-sec-bugs

## njsscan
**Chapter 6.24**  
njsscan is a semantic aware SAST tool that can find insecure code patterns in your Node.js applications using a simple pattern matcher from libsast and pattern search tool like semgrep (lightweight static analysis for many languages).
You can find more details about the project at https://github.com/ajinabraham/njsscan.

## pylint
**Chapter 6.25**  
Pylint is a static code analysis tool for Python which looks for programming errors, helps enforce a coding standard, sniffs out code smells and offers simple refactoring suggestions.
You can find more details about the project at https://github.com/PyCQA/pylint.

## Brakeman
**Chapter 6.26**  
Brakeman is Static Analysis tool for Rails application to find vulnerabilities, Fast and Flexible tools with a very good report and fit to embed it in CI/CD pipeline.
You can find more details about the project at https://brakemanscanner.org/.
