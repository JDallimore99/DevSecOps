# How to Improve on Gaps found in Exam
## Challenge 1
-  First, create a new repo on GitLab (based on https://github.com/OWASP/railsgoat)
Create Rails repository on GitLab
```sh
git remote rename origin old-origin
git remote add origin http://gitlab-ce-yp2xo5em.lab.practical-devsecops.training/root/rails.git
git push -u origin --all
```
And enter the GitLab credentials.
```sh
Username for 'http://gitlab-ce-acsrq8h9.lab.practical-devsecops.training': root
Password for 'http://root@gitlab-ce-acsrq8h9.lab.practical-devsecops.training': pdso-training
```
- Ensure you mark false positives as false positives using brakeman's inbuilt features, and 
ensure that the false positives are not reported as issues in the next scans 

Add Brakeman.ignore file to the GitLab file repository, in the config section, then add argument to the Gitlab-ci.yml file.
```sh
image: docker:latest

services:
  - docker:dind
stages:
  - build
  - test
  - release
  - preprod
  - integration
  - prod

sast:
  stage: build
  script:
    - docker pull hysnsec/brakeman
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/brakeman -r /src -f json  -o /src/brakeman-output.json -i src/config/brakeman.ignore
  artifacts:
    paths: [brakeman-output.json]
    when: always
  allow_failure: true  
```
If there is an error stating configuration cannot be found, this is due to the fact that GitLab cannot find the path of the brakeman.ignore file correctly, and therefore need to double check the path of the file/alter path of the file.

- Also, explain why an issue that is marked as false positive is indeed a false positive and not a 
real finding

This is difficult to do if not working with developers etc. However the best way of doing this is using the confidence levels of the report. If there is a high confidence level, then it is most likely not a false positive. 
Another way of working out if something is a false positive is by looking at the reporting tools Github and seeing if there is any mention of errors in the output documentation.
If nothing else works, google it and see if these are common false positives or not
## Challenge 2
- Ensure the os-hardening job runs only on the master branch in the django.nv repository,
- The os-hardening job must save the results on the CI server for further processing in a 
machine-readable format like JSON, CSV, XML, etc.
```sh
ANSIBLE_STDOUT_CALLBACK=json ansible-playbook -i inventory.ini ansible-hardening.yml | tee hardening.json
```
- Explain the need to save the output in machine-readable formats like JSON, CSV, XML, etc
There are several good reasons. Here is a short list:
1. Because it is difficult, costly, inconvenient, and in a lot of cases impossible to read data by humans
2. Humans make errors, machines (most often) don't
3. Machines are good at sharing information with others than humans
4. Computers need data to run applications properly
5. Computers can identify hidden patterns in the data, that humans cannot see
6. Computers can do complex manipulations on the data
7. Computers are much less prone to lose information than humans
8. Computer labour is cheaper than human labour
9. Computer labour is faster than human labour

- Run the ansible playbook in the dry mode before making changes to the production 
machine
```sh
ansible-playbook playbook.yaml --check
```
Another way to run a playbook in check mode is to add the check_mode parameter to the playbook content:
```sh
---
- hosts: all
  tasks:
  - name: A command to run in check mode
    command: /your/command
    check_mode: on
```
## Challenge 3
- Create a job called secrets-scanning under the build stage to run a tool named detect-secrets 
using docker (example: hysnsec/detect-secrets)
```sh
script:
    - docker pull hysnsec/detect-secrets
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/detect-secrets scan | tee secrets-output.json
  artifacts:
    paths: [secrets-output.json]
    when: always
  allow_failure: true  
```

## Challenge 4
If there is an error with the ZAP Scan being uploaded to Dojo, either error 500 in local terminal or in GitLab, it is most likely due to the fact Dojo cannot find the production machine, therefore the lab needs to be restarted and this should fix the error
-  Ensure the ZAP Scan identifies more application urls for scanning using various available 
spidering options

This allows for the ZAP Scan to spider for longer, therefore scanning more URLs, and implements the ajax spider as well as the traditional spider. The AJAX Spider is an add-on for a crawler called Crawljax. The add-on sets up a local proxy in ZAP to talk to Crawljax. The AJAX Spider allows you to crawl web applications written in AJAX in far more depth than the native Spider. Use the AJAX Spider if you may have web applications written in AJAX.
- -j Ajax spider
- -m time in minutes for the zap scan to spider
```sh
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-6jnpiomi.lab.practical-devsecops.training -j -m 5
```
## Challenge 5
