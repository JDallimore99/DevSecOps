# A Full Enterprise Grade DevSecOps Pipelines

## Explore SCA, SAST, DAST, IaC, CaC and VM exercises
Considering your DevOps team created a simple CI pipeline with the following contents.
```sh
image: docker:latest  # To run all jobs in this pipeline, use a latest docker image

services:
  - docker:dind       # To run all jobs in this pipeline, use a docker image which contains a docker daemon running inside (dind - docker in docker). Reference: https://forum.gitlab.com/t/why-services-docker-dind-is-needed-while-already-having-image-docker/43534

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
   - virtualenv env                       # Create a virtual environment for the python application
   - source env/bin/activate              # Activate the virtual environment
   - pip install -r requirements.txt      # Install the required third party packages as defined in requirements.txt
   - python manage.py check               # Run checks to ensure the application is working fine

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
We will explore each chapter in the lab and gather their respective Gitlab CI/CD scripts and embed them in their respective pipeline stages.
### Software component analysis (SCA)
```sh
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
### Security Application Security Testing (SAST)
```sh
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
### Dynamic Application Security Testing (DAST)
```sh
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
### Infrastructure as Code (IaC)
> You would need to set appropriate environment variables under Gitlab’s CI/CD variables to get the following tasks to work.
Also you would need to push ansible-hardening.yml and inspec profiles etc., to the git repository as discussed under Infrastructure as Coce(IaC) and Compliance as Code(CaC) modules.

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
### Compliance as Code
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
    - docker run --rm -v ~/.ssh:/root/.ssh -v $(pwd):/share hysnsec/inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i ~/.ssh/id_rsa --chef-license accept --reporter json:inspec-output.json
  artifacts:
    paths: [inspec-output.json]
    when: always
  allow_failure: true
```
### Vulnerability Management
```sh
# Vulnerability Management(VM)
sast-with-vm:
  stage: build
  before_script:
    - apk add py-pip py-requests curl
    - curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py
  script:
    - docker pull hysnsec/bandit  # Download bandit docker container
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  after_script:
    - python3 upload-results.py --host $DOJO_HOST --api_key $DOJO_API_TOKEN --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file bandit-output.json --scanner "Bandit Scan"
  artifacts:
    paths: [bandit-output.json]
    when: always
```
Let’s move to the next step where we will combine all of the above scans in a one single Gitlab CI/CD script.
## Combining various scans together
Let’s combine the SCA, SAST, DAST, IaC, CaC scans and Vulnerability Management steps into a Gitlab CI script.
We will login into the GitLab using the following details and execute this pipeline.
|Name|Value|
|:---|:---|
|URL|https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/-/blob/master/.gitlab-ci.yml|
|Username|root|
|Password|pdso-training|

Next, we need to create a CI/CD pipeline by replacing the .gitlab-ci.yml file content with the below Gitlab CI script. Click on the Edit button to start replacing the content (use Control+A and Control+V).
```sh
image: docker:latest  # To run all jobs in this pipeline, use a latest docker image

services:
  - docker:dind       # To run all jobs in this pipeline, use a docker image which contains a docker daemon running inside (dind - docker in docker). Reference: https://forum.gitlab.com/t/why-services-docker-dind-is-needed-while-already-having-image-docker/43534

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
   - virtualenv env                       # Create a virtual environment for the python application
   - source env/bin/activate              # Activate the virtual environment
   - pip install -r requirements.txt      # Install the required third party packages as defined in requirements.txt
   - python manage.py check               # Run checks to ensure the application is working fine

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
  allow_failure: true

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
  allow_failure: true

sslscan:
  stage: integration
  script:
    - docker pull hysnsec/sslyze
    - docker run --rm -v $(pwd):/tmp hysnsec/sslyze prod-acsrq8h9.lab.practical-devsecops.training:443 --json_out /tmp/sslyze-output.json
  artifacts:
    paths: [sslyze-output.json]
    when: always
  allow_failure: true

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
  allow_failure: true

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
  allow_failure: true

# Vulnerability Management(VM)
sast-with-vm:
  stage: build
  before_script:
    - apk add py-pip py-requests curl
    - curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py
  script:
    - docker pull hysnsec/bandit  # Download bandit docker container
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  after_script:
    - python3 upload-results.py --host $DOJO_HOST --api_key $DOJO_API_TOKEN --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file bandit-output.json --scanner "Bandit Scan"
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true
 
dast-zap:
  stage: integration
  before_script:
    - apk add py-pip py-requests
    - docker pull owasp/zap2docker-stable:2.10.0
  script:
    - docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training -d -x zap-output.xml
  after_script:
    - python3 upload-results.py --host $DOJO_HOST --api_key $DOJO_API_TOKEN --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file zap-output.xml --scanner "ZAP Scan"
  artifacts:
    paths: [zap-output.xml]
    when: always
    expire_in: 1 day
  allow_failure: true
```
Make sure you have added the necessary variables into your project (Settings > CI/CD) such as $DOJO_HOST, and $DOJO_API_TOKEN. Otherwise, your results are not uploaded to DefectDojo in the sast-with-vm job.
> Note: Need to complete the push actions for the git repository as seen in Vulnerability Management to allow for the job not to fail

```sh
curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py
pip3 install requests
git config --global user.email "student@pdevsecops.com"
git config --global user.name "student"
git clone http://root:pdso-training@gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv.git
cd django-nv
git add upload-results.py
git commit -m "Add upload-results.py file"
```
Save changes to the file using the Commit changes button.
## Variables Needed
|Name|Value|
|:----|:---|
|DEPLOYMENT_SERVER|prod-acsrq8h9|
|DEPLOYMENT_SERVER_SSH_PRIVKEY|ssh prod-acsrq8h9 cat /root/.ssh/id_rsa|
|DOJO_API_TOKEN|DOJO_API_TOKEN|
|DOJO_HOST|dojo-acsrq8h9.lab.practical-devsecops.training|

## Files needed
ansible-hardening.yml
```sh
---
- name: Playbook to harden ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes

  roles:
    - dev-sec.os-hardening
```
### Verify the pipeline run
As soon as a change is made to the repository, the pipeline starts executing the jobs.

We can see the results of this pipeline by visiting https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/pipelines.
You can click on the appropriate job to see the results.
