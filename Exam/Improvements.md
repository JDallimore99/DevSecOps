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

- The sast job with brakeman should run on every commit on every branch, and should run 
for each pull/merge requests for the railsgoat repository

```sh
rules:
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```
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

Terminal
```sh 
docker pull hysnsec/detect-secrets
docker run -v $(pwd):/src --rm -w /src hysnsec/detect-secrets scan | tee secrets-output.json
```
Github
```sh
secrets-scanning:
  stage: build
  script:
    - docker pull hysnsec/detect-secrets
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm -w /src hysnsec/detect-secrets scan | tee secrets-output.json
  artifacts:
    paths: [secrets-output.json]
    when: always
  allow_failure: true  
```
- Fail the build if there are Secrets

This was tricky and needed to use the jq function to gather the length of the results section. Then an if statement was used to verify whether the results found in the output were more than 0. If so, then the build was failed using exit code 1
```sh 
secrets-scanning:
  stage: build
  before_script:
    - apk add jq
  script:
    - docker pull hysnsec/detect-secrets
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm -w /src hysnsec/detect-secrets scan | tee secrets-output.json
    - cat secrets-output.json | jq .results | jq length
    - len=$(jq '.results | length' secrets-output.json)
    - if [ $len -gt 0 ]; then exit 1; fi
  artifacts:
    paths: [secrets-output.json]
    when: always
  allow_failure: false 
```
## Challenge 4
If there is an error with the ZAP Scan being uploaded to Dojo, either error 500 in local terminal or in GitLab, it is most likely due to the fact Dojo cannot find the production machine, therefore the lab needs to be restarted and this should fix the error
-  Ensure the ZAP Scan identifies more application urls for scanning using various available 
spidering options

This allows for the ZAP Scan to spider for longer, therefore scanning more URLs, and implements the ajax spider as well as the traditional spider. The AJAX Spider is an add-on for a crawler called Crawljax. The add-on sets up a local proxy in ZAP to talk to Crawljax. The AJAX Spider allows you to crawl web applications written in AJAX in far more depth than the native Spider. Use the AJAX Spider if you may have web applications written in AJAX.
- -z set spider options
- -m time in minutes for the zap scan to spider
- -j ajax spider
```sh
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-6jnpiomi.lab.practical-devsecops.training -m 5 -z 
```
## Challenge 5
- scan for antivirus
Add a control for scanning the antivirus by creating a custom inspec profile and then creating the control in the profile/controls directory
```sh
cat >> ubuntu/controls/example.rb <<EOL
title "sample section"

# you can also use plain tests
describe file("/tmp") do
  it { should be_directory }
end

# you add controls here

describe package('clamav') do
  it { should be_installed }
  its('version') { should eq '0.98.7' }
end

describe service('clamd') do
  it { should be_enabled }
  it { should be_installed }
  it { should be_latest }
  it { should be_running }
end
```
Then create the other control files that will include the linux baseline controls using the same methods as above. These can be found from https://github.com/dev-sec/linux-baseline/tree/master/controls

- manual fixes
Add these lines of code to mount the /dev with noexec
```sh
tmpfs /dev tmpfs defaults,noexec,nosuid,nodev 0 0
tmpfs                   /dev/shm                tmpfs   defaults,nodev,nosuid,noexec        0 0
```
```sh
mount -o remount,noexec,nosuid,nodev /dev
```
- 10 Bonus points will be awarded if the manual fixes are added to the ansible role os-hardening (of dev-sec)

To add the file to the role, download the ansible role, find the location and cd into the directory.
```sh
cd /root/.ansible/roles/dev-sec.os-hardening/tasks
```
Open the hardening role and edit the file to include the cron.yml
```
vi hardening.yml
```
```sh
- import_tasks: cron.yml  tags: cron
```
Then create the cron.yml file
```
---
- name: Find cron files and directories
  find:
    paths:
      - /etc
    patterns:
      - cron.hourly
      - cron.daily
      - cron.weekly
      - cron.monthly
      - cron.d
      - crontab
    file_type: any
  register: cron_directories

- name: Ensure permissions on cron files and directories are configured
  ansible.builtin.file:
    path: /etc/cron.d
    owner: root
    group: root
    mode: g-rwx

- name: Ensure permissions on cron files and directories are configured
  ansible.builtin.file:
    path: /etc/cron.d
    owner: root
    group: root
    mode: o-rwx

- name: Ensure permissions on cron.daily to g-rwx and directories are configured
  ansible.builtin.file:
    path: /etc/cron.daily
    owner: root
    group: root
    mode: g-rwx

- name: Ensure permissions on cron.daily to o-rwx
  ansible.builtin.file:
    path: /etc/cron.daily
    owner: root
    group: root
    mode: o-rwx
```
Now, when running the ansible role as usual, it should complete this as one of the tasks

Add devmount.yml to Ansible role, this is done by creating a devmount.yml file
```sh
```sh
- import_tasks: devmount.yml  tags: devmount
```
Create the task in the diretory
```sh
cat > devmount.yml
```
```
---
- name: Mount /dev tmpfs with noexec
  mount:
          path: /dev
          src: tmpfs
          fstype: tmpfs
          opts: defaults,noexec,nosuid,nodev
          state: mounted
```
This adds the devmount task to the role and then mounts /dev with noexec

The other manual fix is already utilised within ansible hardening and therefore does not need to be added
