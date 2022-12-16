# Challenge 1
## Task 1
Implement SCA, SAST, DAST for the django.nv project
As per the best practices, need to test DevSecOps tools locally before embedding them into CI/CD pipeline.

## Software Component Analysis (SCA)
For front-end:
```sh
npm install -g retire # Install retirejs npm package
retire --outputformat json --outputpath retirejs-report.json --severity high
```
For backend:
```sh
docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
```
## Static Application Security Testing (SAST)
For secrets-scanning:
```sh
docker run --rm -v $(pwd):/src hysnsec/trufflehog file:///src --json | tee trufflehog-output.json
```
## For code analysis:
```sh
docker run --user $(id -u):$(id -g) --rm -v $(pwd):/src hysnsec/bandit -r /src -f json -o /src/bandit-output.json
```
## Dynamic Application Security Testing (DAST)
SSL Scan
```sh
docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-acsrq8h9.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json
```
Nmap
```sh
docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-acsrq8h9 -oX /tmp/nmap-output.xml
```
ZAP Baseline
```sh
docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training -J zap-output.json
```
Task 2
Please embed these tests in CI/CD pipeline
Considering your DevOps team created a simple CI pipeline with the following contents.
```
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

build:
  stage: build
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py check

test:
  stage: test
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py test taskManager

integration:
  stage: integration
  script:
    - echo "This is an integration step"
    - exit 1
  allow_failure: true # Even if the job fails, continue to the next stages

prod:
  stage: prod
  script:
    - echo "This is a deploy step."
  when: manual # Continuous Delivery
```
We will try to do these challenges step by step as mentioned in the courseware. SCA, SAST and then DAST.

Software Component Analysis (SCA)
```
# Software Component Analysis
sca-frontend:
  stage: build
  image: node:alpine3.10
  script:
    - npm install
    - npm install -g retire # Install retirejs npm package.
    - retire --outputformat json --outputpath retirejs-report.json --severity high
  artifacts:
    paths: [retirejs-report.json]
    when: always # What is this for?
    expire_in: one week
  allow_failure: true #<--- allow the build to fail but don't mark it as such  
  
sca-backend:
  stage: build
  script:
    - docker pull hysnsec/safety
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  artifacts:
    paths: [oast-results.json]
    when: always # What does this do?
  allow_failure: true #<--- allow the build to fail but don't mark it as such
```
Static Application Security Testing (SAST)
```
# Git Secrets Scanning
secrets-scanning:
  stage: build
  script:
    - apk add git
    - git checkout master
    - docker run --rm -v $(pwd):/src hysnsec/trufflehog file:///src --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always # What is this for?
    expire_in: one week
  allow_failure: true   #<--- allow the build to fail but don't mark it as such

# Static Application Security Testing
sast:
  stage: build
  script:
    - docker pull hysnsec/bandit  # Download bandit docker container
    # Run docker container, please refer docker security course, if this doesn't make sense to you.
    - docker run --user $(id -u):$(id -g) --rm -v $(pwd):/src hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true   #<--- allow the build to fail but don't mark it as such
```
Dynamic Application Security Testing (DAST)
```
# Dynamic Application Security Testing
nikto:
  stage: integration
  script:
    - docker pull hysnsec/nikto
    - docker run --rm -v $(pwd):/tmp hysnsec/nikto -h prod-acsrq8h9 -o /tmp/nikto-output.xml
  artifacts:
    paths: [nikto-output.xml]
    when: always

sslscan:
  stage: integration
  script:
    - docker pull hysnsec/sslyze
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-acsrq8h9.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always

nmap:
  stage: integration
  script:
    - docker pull hysnsec/nmap
    - docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-acsrq8h9 -oX /tmp/nmap-output.xml
  artifacts:
    paths: [nmap-output.xml]
    when: always

zap-baseline:
  stage: integration
  script:
    - docker pull owasp/zap2docker-stable:2.10.0
    - docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training -J zap-output.json
  after_script:
    - docker rmi owasp/zap2docker-stable:2.10.0  # clean up the image to save the disk space
  artifacts:
    paths: [zap-output.json]
    when: always # What does this do?
  allow_failure: false
```
Let’s combine the SCA, SAST and DAST steps into a Gitlab CI script.

We will login into the GitLab using the following details and execute this pipeline.

Name	Value
URL	https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/-/blob/master/.gitlab-ci.yml
Username	root
Password	pdso-training
Next, we need to create a CI/CD pipeline by replacing the .gitlab-ci.yml file content with the below Gitlab CI script. Click on the Edit button to start replacing the content (use Control+A and Control+V).
```
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

build:
  stage: build
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py check

test:
  stage: test
  image: python:3.6
  before_script:
   - pip3 install --upgrade virtualenv
  script:
   - virtualenv env
   - source env/bin/activate
   - pip install -r requirements.txt
   - python manage.py test taskManager

# Software Component Analysis
sca-frontend:
  stage: build
  image: node:alpine3.10
  script:
    - npm install
    - npm install -g retire # Install retirejs npm package.
    - retire --outputformat json --outputpath retirejs-report.json --severity high
  artifacts:
    paths: [retirejs-report.json]
    when: always # What is this for?
    expire_in: one week
  allow_failure: true #<--- allow the build to fail but don't mark it as such  

sca-backend:
  stage: build
  script:
    - docker pull hysnsec/safety
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  artifacts:
    paths: [oast-results.json]
    when: always # What does this do?
  allow_failure: true #<--- allow the build to fail but don't mark it as such

# Git Secrets Scanning
secrets-scanning:
  stage: build
  script:
    - docker run -v $(pwd):/src --rm hysnsec/trufflehog --repo_path /src file:///src --json | tee trufflehog-output.json
  artifacts:
    paths: [trufflehog-output.json]
    when: always  # What is this for?
    expire_in: one week
  allow_failure: true

# Static Application Security Testing
sast:
  stage: build
  script:
    - docker pull hysnsec/bandit  # Download bandit docker container
    # Run docker container, please refer docker security course, if this doesn't make sense to you.
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true   #<--- allow the build to fail but don't mark it as such

# Dynamic Application Security Testing
nikto:
  stage: integration
  script:
    - docker pull hysnsec/nikto
    - docker run --rm -v $(pwd):/tmp hysnsec/nikto -h prod-acsrq8h9 -o /tmp/nikto-output.xml
  artifacts:
    paths: [nikto-output.xml]
    when: always

sslscan:
  stage: integration
  script:
    - docker pull hysnsec/sslyze
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-acsrq8h9.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always

nmap:
  stage: integration
  script:
    - docker pull hysnsec/nmap
    - docker run --rm -v $(pwd):/tmp hysnsec/nmap prod-acsrq8h9 -oX /tmp/nmap-output.xml
  artifacts:
    paths: [nmap-output.xml]
    when: always

zap-baseline:
  stage: integration
  script:
    - docker pull owasp/zap2docker-stable:2.10.0
    - docker run --user $(id -u):$(id -g) --rm -v $(pwd):/zap/wrk:rw owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training -J zap-output.json
  after_script:
    - docker rmi owasp/zap2docker-stable:2.10.0  # clean up the image to save the disk space
  artifacts:
    paths: [zap-output.json]
    when: always # What does this do?
  allow_failure: false
```
Save changes to the file using the Commit changes button.

Verify the pipeline run

We can see the results of this pipeline by visiting https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/pipelines.

Task 3
Ensure all the best practices covered in the course videos, labs and slack discussion are being implemented

Tried to implement all the best practices we have learned in the course
1. Tested the tools locally before embedding in the pipeline
2. Ensured the scans finish within 10 minutes
3. Ensured they each run in their own jobs
4. We saved the output in a file
5. We didn’t fail the builds

# Challenge 2
## Task 1
### Harden the production machine

As per the best practices, need to test DevSecOps tools locally before embedding them into CI/CD pipeline.

IaC
We need the inventory.ini first, so lets create one
```sh
cat > inventory.ini <<EOL
# DevSecOps Studio Inventory
[devsecops]
devsecops-box-acsrq8h9

EOL
```
Let’s download the role
```sh
ansible-galaxy install dev-sec.os-hardening
```
Next, let’s create a playbook
```sh
cat > ansible-hardening.yml <<EOL
---
- name: Playbook to harden Ubuntu OS.
  hosts: devsecops
  remote_user: root
  become: yes

  roles:
    - dev-sec.os-hardening

EOL
```
Let’s run the pipeline now.
```sh
ansible-playbook -i inventory.ini ansible-hardening.yml
```
### Task 2
Ensure it stays compliant with linux-baseline Inspec Profile
We can verify if the machine stays hardened using Inspec.
```sh
docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i /root/.ssh/id_rsa --chef-license accept --reporter json:/share/inspec-output.json
```
> Note /share directory should be used when using hysnsec/inspec image. Because it’s a custom image adding another directory would not work when you are saving the inspec output.
If there are any discrepancies in the results, we need to explore and fix them.

Task 3
Embed these tests as part of CI/CD pipeline

Infrastructure as Code (IaC)
```sh
# Infrastructure as Code
# PLEASE ENSURE YOU HAVE SETUP THE ENVIRONMENT VARIABLES APPROPRIATELY
ansible-hardening:
  stage: prod
  image: willhallonline/ansible:2.9-ubuntu-18.04
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - echo -e "[prod]\n$DEPLOYMENT_SERVER" >> inventory.ini
    - ansible-galaxy install dev-sec.os-hardening
    - ansible-playbook -i inventory.ini ansible-hardening.yml
```
Compliance as Code (CaC)
```sh
# Compliance as Code
# PLEASE ENSURE YOU HAVE SETUP THE ENVIRONMENT VARIABLES APPROPRIATELY
inspec:
  stage: prod
  only:
    - "master"
  environment: production
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i /root/.ssh/id_rsa --chef-license accept --reporter json:/share/inspec-output.json
  artifacts:
    paths: [inspec-output.json]
    when: always
```
Login into the GitLab using the following details and execute this pipeline.

Name	Value
URL	https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/-/blob/master/.gitlab-ci.yml
Username	root
Password	pdso-training
Next, we need to create a CI/CD pipeline by replacing the .gitlab-ci.yml file content with the below Gitlab CI script. Click on the Edit button to start replacing the content (use Control+A and Control+V).
```
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

# Infrastructure as Code
# PLEASE ENSURE YOU HAVE SETUP THE ENVIRONMENT VARIABLES AND NEEDED FILES APPROPRIATELY
ansible-hardening:
  stage: prod
  image: willhallonline/ansible:2.9-ubuntu-18.04
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - echo -e "[prod]\n$DEPLOYMENT_SERVER" >> inventory.ini
    - ansible-galaxy install dev-sec.os-hardening
    - ansible-playbook -i inventory.ini ansible-hardening.yml

# Compliance as Code
# PLEASE ENSURE YOU HAVE SETUP THE ENVIRONMENT VARIABLES AND NEEDED FILES APPROPRIATELY
inspec:
  stage: prod
  only:
    - "master"
  environment: production
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i ~/.ssh/id_rsa --chef-license accept --reporter json:inspec-output.json
  artifacts:
    paths: [inspec-output.json]
    when: always
```
> Make sure you have added the necessary variables into your project (Settings > CI/CD) such as $DEPLOYMENT_SERVER_SSH_PRIVKEY and $DEPLOYMENT_SERVER.

Save changes to the file using the Commit changes button.

Verify the pipeline run
As soon as a change is made to the repository, the pipeline starts executing the jobs.

We can see the results of this pipeline by visiting https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/pipelines
