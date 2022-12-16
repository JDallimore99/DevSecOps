# Purpose and Location
The purpose of each tool used in Pipeline and the location it is found in both the pipeline and Chapter
## Software Component Analysis 
### Safety
**Chapter 5.1**  
Safety checks your installed dependencies for known security vulnerabilities.
By default, it uses the open Python vulnerability database Safety DB but can be upgraded to use pyup.io’s Safety API using the –key option.
You can find more details about the project at Safety: https://github.com/pyupio/safety
CICD Stage: Test

### Pip-audit
**Chapter 5.7**  
The pip-audit is a tool for scanning Python environments for packages with known vulnerabilities. It uses the Python Packaging Advisory Database (https://github.com/pypa/advisory-db) via the PyPI JSON API as a source of vulnerability reports.
Source: https://github.com/trailofbits/pip-audit
CICD Stage: Test

### RetireJS
**Chapter 5.8**  
There are lots of JavaScript libraries for use on the Web and in Node.JS apps. The availability of these libraries greatly simplifies development, but also increases the security risks. Because of rampant abuse of these libraries, attackers are concentrating on these libraries. Realizing the need for awareness, OWASP has included “Using Components with Known Vulnerabilities” as part of the OWASP Top 10 list.
RetireJS helps you in finding the insecure Javascript libraries in your code.
Source: https://github.com/retirejs/retire.js
CICD Stage: Test
### Dependency-Check
**Chapter 5.13**  
Dependency-Check is SCA tool that attempts to detect publicly disclosed vulnerabilities contained within a project’s dependencies.
You can find more details about the project at https://github.com/jeremylong/DependencyCheck.
CICD Stage: Test

### Snyk 
**Chapter 5.17**  
Dependency analysis tool or SCA is used to find vulnerabilities in open source dependencies and it supports multiple programming languages such as Ruby, Python, NodeJS, Java, Go, .NET. It’s easy to use, a developer can use it on his/her machine using the command line or DevOps team can embed it into CI/CD pipeline. (Source: https://snyk.io)
You can find more details about the project at https://github.com/snyk/snyk.
CICD Stage: Test

### NPM audit
**Chapter 5.21**  
NPM has security build in since npm@6, this command allow you to analyze dependencies in your existing code to identify insecure dependencies, have a good security report, and also easy to implement it into CI/CD pipeline without install any packages except NPM itself.
CICD Stage: Test

### AuditJS
**Chapter 5.22**  
AuditJS Audits an NPM package.json file to identify known vulnerabilities. It uses the OSS Index v3 REST API from Sonatype to identify known vulnerabilities and outdated package versions.
CICD Stage: Test

### bundler-audit
**Chapter 5.24**  
Bundler-audit provides rake tasks for checking the code and for updating its vulnerability database
CICD Stage: Test

### chelsea
**Chapter 5.28**  
Chelsea is a CLI application written in Ruby, designed to allow you to scan your RubyGem powered projects and report on any vulnerabilities in your third party dependencies. It is powered by Sonatype’s OSS Index.
CICD Stage: Test

## Static Application Security Testing (SAST)
### TruffleHog
**Chapter 6.1**  
TruflleHog is a tool that searches through git repositories for secrets, digging deep into commit history and branches. This tool is useful in finding the secrets accidentally committed to the repo.
You can find more details about the project at https://github.com/dxa4481/truffleHog.
CICD Stage: Build

### Bandit
**Chapter 6.8**  
The Bandit is a tool designed to find common security issues in Python code.
To do this Bandit, processes each file, builds an AST, and runs appropriate plugins against the AST nodes. Once Bandit has finished scanning all the files it generates a report.
Bandit was originally developed within the OpenStack Security Project and later rehomed to PyCQA.
You can find more details about the project at https://github.com/PyCQA/bandit.
CICD Stage: Build  
### Gosec
**Chapter 6.14**
GoSec inspects source code for security problems by scanning the Go AST, also can be configured to only run a subset of rules, to exclude certain file paths, and produce reports in different formats You can find more details about the project at https://github.com/securego/gosec
CICD Stage: Build  
### Semgrep
**Chapter 6.18**
Semgrep is a fast, open-source, static analysis tool that excels at expressing code standards — without complicated queries — and surfacing bugs early in the development flow. Precise rules look like the code you’re searching; no more traversing abstract syntax trees or wrestling with regexes.
https://github.com/returntocorp/semgrep.

### Hadolint
**Chapter 6.22**
Hadolint checks a Dockerfile for Docker image best practices. The linter is parsing the Dockerfile into an AST and performs rules on top of the AST.
https://github.com/hadolint/hadolint.
  
### FindSecBugs
**Chapter 6.23**  
The SpotBugs plugin for security audits of Java web applications and Android applications.
You can find more details about the project at https://github.com/find-sec-bugs/find-sec-bugs
CICD Stage: Build  

### njsscan
**Chapter 6.24**  
njsscan is a semantic aware SAST tool that can find insecure code patterns in your Node.js applications using a simple pattern matcher from libsast and pattern search tool like semgrep (lightweight static analysis for many languages).
You can find more details about the project at https://github.com/ajinabraham/njsscan.

### pylint
**Chapter 6.25**  
Pylint is a static code analysis tool for Python which looks for programming errors, helps enforce a coding standard, sniffs out code smells and offers simple refactoring suggestions.
You can find more details about the project at https://github.com/PyCQA/pylint.

### Brakeman
**Chapter 6.26**  
Brakeman is Static Analysis tool for Rails application to find vulnerabilities, Fast and Flexible tools with a very good report and fit to embed it in CI/CD pipeline.
You can find more details about the project at https://brakemanscanner.org/.
CICD Stage: Build  

## 7.Dynamic Application Security Testing 
### Nikto
**Chapter 7.1**  
Nikto is a web server assessment tool. It’s designed to find various default and insecure files, configurations, and programs on any type of web server.
Nikto is built on LibWhisker2 (by RFP) and can run on any platform which has a Perl environment. It supports SSL, proxies, host authentication, attack encoding, and more.
Source: Nikto official. https://cirt.net/Nikto2
CICD Stage: Integration

### MAP
**Chapter 7.2**  
Nmap (“Network Mapper”) is a free and open source (license) utility for network discovery and security auditing. Many systems and network administrators also find it useful for tasks such as network inventory, managing service upgrade schedules, and monitoring host or service uptime.
Source Nmap official website https://nmap.org/
CICD Stage: Integration  

### SLyze
**Chapter 7.3**  
SSLyze is a fast and powerful SSL/TLS scanning library.
It allows you to analyze the SSL/TLS configuration of a server by connecting to it, in order to detect various issues (bad certificate, weak cipher suites, Heartbleed, ROBOT, TLS 1.3 support, etc.).
SSLyze can either be used as command line tool or as a Python library.
Source: SSLyze Github https://github.com/nabla-c0d3/sslyze
CICD Stage: Integration  

### ZAP
**Chapter 7.5**  
ZAP is an open-source web application security scanner to perform security testing (Dynamic Testing) on web applications. OWASP ZAP is the flagship OWASP project used extensively by penetration testers. ZAP can also run in a daemon mode for hands-off scans for CI/CD pipeline. ZAP provides extensive API (SDK) and a REST API to help users create custom scripts.
Source: OWASP ZAP https://www.zaproxy.org/getting-started/
CICD Stage: Integration  

### Dastardly
**Chapter 7.6**  
Dastardly is an open-source lightweight web application security scanner that can be used as a Dynamic Application Security Testing tool. It can be easily integrated into your CI/CD system to find vulnerabilities in your software applications.
Source: Dastardly, from Burp Suite https://portswigger.net/burp/dastardly
CICD Stage: Integration  

## 8.Infrastructure as Code (IaC)
### Ansible
**Chapter 8.1**  
Ansible uses simple English like language to automate configurations, settings, and deployments in traditional and cloud environments. It’s easy to learn and can be understood by even non-technical folks.
Source: Ansible official website https://www.ansible.com/
CICD Stage: Prod

### Terrascan
**Chapter 8.16**   
Terrascan allows us to detect compliance and security violations across Infrastructure as Code to mitigate risk before provisioning cloud-native infrastructure.
You can find more details about the project at https://github.com/accurics/terrascan.
CICD Stage: Validate

### Snyk
**Chapter 8.24**  
CLI and build-time tool to find & fix known vulnerabilities in open-source dependencies, apart from Software Component Analysis (SCA), Snyk also support to perform SAST for Infrastructure as Code like Terraform.
Source: Snyk Github Page https://github.com/snyk/snyk
CICD Stage: Build
## 9.Compliance as Code (CaC)
### Inspec
**Chapter 9.1**  
Chef InSpec is an open-source framework for testing and auditing your applications and infrastructure. Chef InSpec works by comparing the actual state of your system with the desired state that you express in easy-to-read and easy-to-write Chef InSpec code. Chef InSpec detects violations and displays findings in the form of a report, but puts you in control of remediation.
Source: Inspec official website https://www.inspec.io/docs

## 10.Vulnerability Management
### DefectDojo
**Chapter 10.1**  
DefectDojo is a security tool that automates application security vulnerability management. DefectDojo streamlines the application security testing process by offering features such as importing third party security findings, merging and de-duping, integration with Jira, templating, report generation and security metrics.
Source: https://defectdojo.github.io/django-DefectDojo
CICD Stage: Dependent on scan being submitted to Dojo
