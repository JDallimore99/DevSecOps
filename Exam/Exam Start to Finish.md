
## Challenge 1: Create a CI/CD pipeline to embed the brakeman tool with
the following steps (25 points)
In this challenge, you will use Gitlab CI to implement a CI/CD pipeline with the following
details:
- First, create a new repo on GitLab (based on https://github.com/OWASP/railsgoat),
then create a job called sast under the build stage to set up brakeman tool
(https://brakemanscanner.org)

Clone the Railsgoat repository into the terminal
```
git clone https://github.com/OWASP/railsgoat rails
```
cd into the repository
```
cd rails
```
Set the origin of the cloned repository from origin to old-origin
```
git remote rename origin old-origin
```
Add the new origin for the cloned repository to the new web address. Essentially copying over the repository and inserting it into the new gitlab project
```
git remote add origin http://gitlab-ce-xxxxxxxx.lab.practical-devsecops.training/root/rails.git
```
This forces the new repository that has been copied into the new web url, essentially creating the new, copied, railsgoat repository in git
```
git push -u origin --all
```
And enter the GitLab credentials.
```
Username for 'http://gitlab-ce-xxxxxxxx.lab.practical-devsecops.training': root
Password for 'http://root@gitlab-ce-xxxxxxxx.lab.practical-devsecops.training': pdso-training
```

Run locally first, by installing the dependencies and the scanning tool into the terminal.
```
apt update
apt install ruby-full -y
gem install brakeman -v 5.2.1
```
Run the scanner with the output as a json file
```
brakeman -f json | tee result.json
```
Use the ignore feature in the brakeman scanning tool to ignore files. Create brakeman.ignore first and add the false-positive
```sh
cat > brakeman.ignore <<EOF
{
    "ignored_warnings": [
        {
          "fingerprint": "febb21e45b226bb6bcdc23031091394a3ed80c76357f66b1f348844a7626f4df",
          "note": "ignore XSS"
        }
    ]
}
EOF
```
Then use run the scan with the inbuilt ignore feature
```sh
brakeman -f json -i brakeman.ignore | tee result.json
```

- The sast job with brakeman should run on every commit on every branch, and
should run for each pull/merge requests for the railsgoat repository

There is no rule set for a specific branch to be utilised in the rules section, therefore it will run on every commit on every branch. This is the default for GitLab CI/CD Pipeline. However, to set the job to run for every pull/merge request on the railsgoat repository, the rule section can be utilised by adding in the following. Which states that every time there is a merge event, the job will run. However, adding this ruleset overrides the default and therefore another if rule needs to be implemented that states if there is a commit on a branch, the should should run.
```
rules:
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
  - if: '$CI_COMMIT_BRANCH'
```
- sast job must implement all the applicable DevSecOps Gospel (best practices), and
must save the scan results on the CI server for further processing in a
machine-readable format such as JSON, CSV, XML, etc.
```
-o /src/brakeman-output.json
  artifacts:
    paths: [brakeman-output.json]
```
- Ensure you mark false positives as false positives using brakeman's inbuilt features,
and ensure that the false positives are not reported as issues in the next scans

Utilising the brakeman.ignore function within the tools inbuilt feature, you can manually add the fingerprint and a note as to why this warning is being ignored. This can be added to the project repository and then added to the .gitlab-ci.yml file and appended to the sast job. Firstly, the ignore file is created in this format with the title brakeman.ignore
```
{
    "ignored_warnings": [
        {
          "fingerprint": "febb21e45b226bb6bcdc23031091394a3ed80c76357f66b1f348844a7626f4df",
          "note": "ignore XSS"
        }
    ]
}
```
Use the -i output option (or --ignore-config) in the tool scan to add the brakeman ignore file to the tools scanning output, therefore if there are any false-positives/known vulnerabiltiies within the brakeman.ignore file, these will be found in the ignored section of the result output.
```
-i src/brakeman.ignore
```
- Also, explain why an issue that is marked as false positive is indeed a false positive
and not a real finding


Therefore the entire .gitlab-ci.yml file looks like this

```sh
image: docker:latest  

services:
  - docker:dind       

stages:
  - build

sast:
  stage: build
  script:
    - docker pull hysnsec/brakeman  # Download brakeman docker container
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/brakeman -r /src -i src/brakeman.ignore -f json -o /src/brakeman-output.json
  artifacts:
    paths: [brakeman-output.json]
    when: always
  allow_failure: true
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
    - if: '$CI_COMMIT_BRANCH'
```

## Challenge 2: Create an Ansible Playbook to harden the prod server (25
points)
In this challenge, you will use Gitlab CI to implement a CI/CD pipeline with the following
details:
● Create a job called os-hardening under the deploy stage to run ansible os
hardening script from dev-sec (https://github.com/dev-sec/ansible-os-hardening),
the hardening script should harden the production machine

Let’s login into the GitLab and configure production machine https://gitlab-ce-xxxxxxxx.lab.practical-devsecops.training/root/django-nv/-/settings/ci_cd. We can use the GitLab credentials provided below to login. Click on the Expand button under the Variables section, then click the Add Variable button. Add the following key/value pair in the form.

|Name	|Value|
|:---|:----|
|Key|	DEPLOYMENT_SERVER|
|Value|	prod-xxxxxxxx|

|Name|	Value|
|:----|:----|
|Key|	DEPLOYMENT_SERVER_SSH_PRIVKEY|
|Value|	Copy the private key from the production machine using SSH. The SSH key is available at /root/.ssh/id_rsa.|

To gain the SSH Key, we enter into the prod machine in the terminal and then output the ssh
```
ssh prod-a59tqdzr
```
```
cat /root/ .ssh/id_rsa
```
```
**Output**
-----BEGIN RSA PRIVATE KEY-----MIIEpAIBAAKCAQEAwNpZcFqfOVT0Q486JHr7kQO7HGD2zsGC4iodBxc6bdU/S1znOUM633CIM0UYSqRTaOIRAi+HowZ5W0/8ZKW3okMQTQNjjY7DLiT3lRxRUly1PEi2dAimRGlgm1ycE/ucKXShiH9iSSeIrtkUiHWxvRssxSjdT0lGhB/z7PN2hu0mpj2kCDoeSXRM+EtFBkc45Mbyc7VCIyYcW2gUmFCLr8js/vUuGJYu/FQp8yZDvVg8+bBx5tHjKd+qNCs0HhytWbhVWDwl46uigIrvwk6gaQXgVv1djrPNh9eYYLoXSwTfo//cDrPZspBia9bxShKbL+zGcyoVgAy/yvdM3peI6QIDAQABAoIBAQCubpCJFB6CT7njxY+UYXxa7OH3yChUWClXATpiKHtbzo7CTpSBcbK1WOaIYQ2YrcsXyaoSrQTkyr1HfzBNpKpU5I3A6rjH2AHoId2iDAvuEBaJIUeN6ijhJeMQgxJU7LaRtIFKodU3T7/MTmLJDpMl9YdoCQ8rYJ6ccP5DKu7hF9fQA/8V5/OAkj2XKjQg7APEYV+nFjYvcpl6zw/P5DnRTzKQPG58u2TMb8cPKmi9MAJUAeFd1k2uBxBWXdIXHqDH7fHNFKlb5vaP
iYsNQtDRiVLDu1+CJ5ddo8IEFBRJVk8xUD9vjAocplINgc9rRIlIKQDs4tHxoceUwv2ZJG1RAoGBAP4tdWTj2ds7TS4yBoOGLHdXDIPHHBHV8g7V39TBCMO4aRfB2Xf+tHR5ykp7o2OqSH+sVDzK4tGy50dWZzw+m4lR3714C4BWx89OZJw/5RJ0j2OSLpIpOPAykJtrKBYPJZ3Lan90HnVkU5MD4blmRrdHFrq6idYaDXCHgBvC1undAoGBAMI8VGw9C1iDPz1PcLOmV3uVpNJFLud0gSaTtUyouGJPbauvwcnVIxgeXjRO3sPMHEvI700OvMDH/YUCE5YB0VtZ0y/55s/qGEp8LRzYFUUxHB38YnekNW9PCUZ5Fmbfcv5a2veoxdT4FO/U6Stg0aLXQxakyFj7AcDKl1WcCzh9AoGAEHuuMz67cAYmeSpxVbIrzAlvHFSbM2Tmb6PbAhcKlHavCgVeLvPri+oh/jaKX/o4/V6Vj+OwVdz+NpgZ1cRRndQbaFQSmt4F0yHIUIGsP0gjzFc8gen+cUU2L34BeXy9+b+pRl6nYwGAkfYce0Nwro4DoVRbf/DskjGXUzWNblkCgYAMWBMxccu3y1eIiPTrpeWnaAI6jsUFVqUik36RKaPWM6APqjLRpeb+EGgCQQTtQpqFwnZa2lXqlospGdGu1dy9Rn8ibGpbyk/S5ANl8uGfLRjRWwnS+q+erFI1lVp0HT1Mpu+Fj8dK2p1SBKDw7c1E4RNVbBGDfihFXVqyySD5bQKBgQDJt2QT2oxFP/UYU1OQO1Vu38U82Yw5F7XCFE5GaUWv2n+ID/4kg6IRsCXOqlg8u/Hat/tsAI4WNjyjXSLLdCvF85d6Sx2/pbPMfNBttDr6Oj0+CikklQCi1PSx8Bw50vWJVZFlHSC0thG7psyrflbOoLBKdwfvBkrfSrpfyNeQGQ==-----END RSA PRIVATE KEY-----
```
Finally, Click on the button Add Variable. Next, please visit https://gitlab-ce-xxxxxxxx.lab.practical-devsecops.training/root/django-nv/-/blob/master/.gitlab-ci.yml. Click on the Edit button and append the following code to the .gitlab-ci.yml file.
```
#job named ansible-hardening
ansible-hardening:
#In prod stage
  stage: prod
  #Using the docker image shown below (alreadu downloaded Ansible)
  image: willhallonline/ansible:2.9-ubuntu-18.04
  #Functions that are executed before the main script is run, setting up required ssh keys and permissions
  before_script:
  #Ensuring .ssh directory is present
    - mkdir -p ~/.ssh
    #Echoing the prod server private key to the id_rsa file while removing the carriage return. Needs to be removed to ensure there are no copy bays
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    #Set appropriate file permissions to the file key to allow for read and write access (chmod)
    - chmod 600 ~/.ssh/id_rsa
    #Load necessary environment variables for ssh connection
    - eval "$(ssh-agent -s)"
    Loading prod servers private key using ssh-add
    - ssh-add ~/.ssh/id_rsa
    #Grab the rsa fingerprint of the deployment server amd append to the known hosts file
    - ssh-keyscan -t rsa $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
  #Reads the deployment server variable and adds to the inventory.ini file with a group named prod
    - echo -e "[prod]\n$DEPLOYMENT_SERVER" >> inventory.ini
    #installing the role from ansible galaxy
    - ansible-galaxy install dev-sec.os-hardening
    #using the ansible playbook to run the ansible hardening file with the inventory.ini
    - ANSIBLE_STDOUT_CALLBACK=json ansible-playbook -i inventory.ini ansible-hardening.yml --check | hardening.json
    artifacts:
    paths: [hardening.json]
    when: always
  rules:
     - if: $CI_COMMIT_BRANCH == 'master'
  allow_failure: true
```

Need to include the hardening script within the git repository
```---
#Names playbook
- name: Playbook to harden ubuntu OS.
#places within the prod host
  hosts: prod
  #allows the root user to use file
  remote_user: root
  become: yes
#this is the ansible role that will be used to complete tasks in the inventory.ini file
  roles:
    - dev-sec.os-hardening
```

- Ensure the os-hardening job runs only on the master branch in the django.nv
repository, and the os-hardening job must not fail the build

```
rules:
    - if: $CI_COMMIT_BRANCH == 'master'
```
- The os-hardening job must save the results on the CI server for further processing
in a machine-readable format like JSON, CSV, XML, etc.
Firstly, can use environmental variables to make the output become json. This output makes the playbook output as a json by using the ANSIBLE_SDTOUT_CALLBACK=json variable
```
ANSIBLE_STDOUT_CALLBACK=json ansible-playbook -i inventory.ini ansible-hardening.yml | tee hardening.json
```
Another way is to edit the config file for ansible, ```ansible.cfg``` and include this within its configuration settings
```
cat > ansible.cfg <<EOF
[defaults]
stdout_callback = json
inventory = /inventory.ini
EOF
``` 
- Explain the need to save the output in machine-readable formats like JSON, CSV,
XML, etc.
- Run the ansible playbook in the dry mode before making changes to the production
machine


- As always, test everything locally on the DevSecOps-box machine before integrating
it into the CI/CD pipeline

Download Ansible locally into terminal
```sh
pip3 install ansible==6.4.0 ansible-lint==6.8.1
```
Create an inventory file for the ansible-playbook to run against, allowing for the playbook to harden the devsecops prod server
```
cat > inventory.ini <<EOL
# DevSecOps Studio Inventory
[devsecops]
devsecops-box-xxxxxxx
[prod]
prod-xxxxxxxx
EOL
```
Ensure the SSH’s yes/no prompt is not shown while running the ansible commands, so we will be using ssh-keyscan to capture the key signatures beforehand.
```sh
ssh-keyscan -t rsa prod-xxxxxxxx devsecops-box-xxxxxxxx >> ~/.ssh/known_hosts
```
Create the main playbook file with the host set to the prod host, therefore allowing the playbook to target everything within the host, which in this case is the prod server
```sh
cat > /hardening/ansible-hardening.yml <<EOL
---
- name: Playbook to harden Ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes
  roles:
    - dev-sec.os-hardening
EOL
```
Download the role from the Ansible role repository using the ansible galaxy function
```
ansible-galaxy install dev-sec.os-hardening
```
Run the ansible-playbook against the prod server using the ansible-playbook function, specifting the inventory target, the playbook being used. Using the ansible built in feature --check, we can run this in dry mode. The infrastructure should be indempotent. If the code is run once, there should be changes. If the same code is ran a second time, then it should not make any further changes. The Ansible Playbook can be run numerous times but only does it make a change the first time due to this being the only time. Using --check mode, we can run the playbook against the machine and simulate what the playbook would do against the prod serveer without the playook making any changes to the server itself.
```
ansible-playbook -i inventory.ini ansible-hardening.yml --check
```

## Challenge 3: Scan for secrets in django.nv repository (15 points)
In this challenge, you will use Gitlab CI to implement a CI/CD pipeline using a secret
scanning tool named detect-secrets with the following details:
- Create a job called secrets-scanning under the build stage to run a tool named
detect-secrets using docker (example: hysnsec/detect-secrets)
```
image: docker:latest
services:
   - docker:dind
stages: 
   - build
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

- The secrets-scanning job must fail the build if issues are found (please do not just
use exit 1 to achieve job failure)

There is no functionality for the detect-secrets scan to fail the build, therefore if the scan finds secrets it will still be marked as a success, unlike truffle-hog which would fail the build if there were secrets found. Therefore, need to use the jq function to gather the length of the results section. Then an if statement was used to verify whether the results found in the output were more than 0. If so, then the build was failed using exit code 1. As the jq function is not available in the .gitlab-ci.yml file, it needs to be added using apk add jq. Allow_failure is set to false so if the job fails, the entire build fails.
```
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
- The secrets-scanning job must save the detect-secrets results/output on the CI
server for further processing in a machine-readable format like JSON, CSV, XML, etc.
```
- docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm -w /src hysnsec/detect-secrets scan | tee secrets-output.json
    - cat secrets-output.json | jq .results | jq length
    - len=$(jq '.results | length' secrets-output.json)
    - if [ $len -gt 0 ]; then exit 1; fi
  artifacts:
    paths: [secrets-output.json]
    when: always
```
- secrets-scanning job must implement all the applicable DevSecOps Gospel (best
practices)
- As always, test everything locally on the DevSecOps-box machine before integrating
it into the CI/CD pipeline

```
docker pull hysnsec/detect-secrets
docker run -v $(pwd):/src --rm -w /src hysnsec/detect-secrets scan | tee secrets-output.json
```
## Challenge 4: Run ZAP Scan against the django.nv application (production) and upload results to Defect Dojo (20 points) In this challenge, you will run a ZAP baseline scan on the django.nv application(production) from the DevSecOps Box, then integrate baseline scan into CI/CD pipeline with the following details:
- Run ZAP Scan on the django.nv app, and upload the results of the ZAP baseline scan into defect dojo's engagement id 1, using upload-results.py python script

Download source code from the Django.nv repository on gitlab and cd into webapp
```
git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp
```
Enter into the repository
```
cd webapp
```
Download stable ZAP docker image using docker pull
```
docker pull owasp/zap2docker-stable:2.10.0
```
Run the zap baseline against the production machine and save as JSON (machine readable format)
```
docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-xxxxxxxx.lab.practical-devsecops.training -J zap-output.json
```
As we can see from the scan, there are 10 new warnings found within the production machine, with each warning showing the contributing reason and https:// related to the production machine.
To view in JSON format, use cat and the zap-output.json
```
Cat zap-output.json
```
Create a new interactive engagement on Defect Dojo within Django called Zap Scan. This should be set to a time period within the exam lifecycle. The status of the engagement should also be set to in progress
Download upload results locally into the DevSecOps box.
```
curl https://gitlab.practical-devsecops.training/-/snippets/3/raw -o upload-results.py
```
Install requests module as well.
```
Pip3 install requests
```
Create API_KEY locally in the Linux terminal.
```
export API_KEY=$(curl -s -XPOST -H 'content-type: application/json' https://dojo-xxxxxxxx.lab.practical-devsecops.training/api/v2/api-token-auth/ -d '{"username": "root", "password": "pdso-training"}' | jq -r '.token' )
```
The api key can be validified by echoing the API_KEY.
```
echo API_KEY
```
Upload results to Dojo using the upload-results.py function. Notice how the ZAP Scan output was changed from json to xml. That is because DefectDojo only accepts ZAP-Scans and translates them when uploaded in an xml format. The engagement_id, product_id and lead_id are all relevant to the engagement, repository and 
```
python3 upload-results.py --host dojo-xxxxxxxx.practical-devsecops.training --api_key $API_KEY --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file zap-output.xml --scanner "ZAP Scan"
```

- Ensure the ZAP Scan identifies more application urls for scanning using various available spidering options

There are a number of different ways to include furhter spidering in the ZAP scan using the options in the tooling. First is to extend the average time of the scan by using the -m option (time in minutes for the zap scan to spider), -J option (ajax spidering) and -z options (allows you to set spider options). Here we use the first two options to increase spidering. m is set to 5, rather than the default which is 1 minute. 
```sh
docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-xxxxxxxx.lab.practical-devsecops.training -m 5 -J
```
- Create a zap job in the integration stage to scan the production app (django.nv), and automatically upload the ZAP Scan results to Defect Dojo on every scan
```
dast-zap:
  stage: integration
  before_script:
    - apk add py-pip py-requests
    - docker pull owasp/zap2docker-stable:2.10.0
  script:
    - docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-xxxxxxxx.lab.practical-devsecops.training -d -x zap-output.xml -J -m 5
  after_script:
    - python3 upload-results.py --host $DOJO_HOST --api_key $DOJO_API_TOKEN --engagement_id 1 --product_id 1 --lead_id 1 --environment "Production" --result_file zap-output.xml --scanner "ZAP Scan"
  artifacts:
    paths: [zap-output.xml]
    when: always
    expire_in: 1 day
```

## Challenge 5: Run inspec linux-baseline profile on the production
machine (15 points)
In this challenge, you will run linux-baseline inspec profile on the production machine, and
fix failed tests/errors on the production machine as outlined below:
- Run linux-baseline inspec profile on production machine from the DevSecOps-Box

Download inspec Debian package which has the inspec tool in it
```
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
```
Install the downloaded inspec Debian package
```
dpkg -i inspec_4.37.8-1_amd64.deb
```
Run inspec against the prod machine using linux-baseline derived from the website 
```
inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-hh3rj0pi -i ~/.ssh/id_rsa --chef-license accept
```
This creates a total of 3 control failures and 8 overall failures

- Fix the inspec control failures on the production machine manually (not using any
automation), you can log in to the production machine using the ssh command

To fix this enter into the prod machine and file ound with failures
```
ssh prod-xxxxxxxx
```
As you can see these values do not match the expected. Edit to the expected values described within the scan and then save the file
```
vi /etc/login.defs
```
Run the inspec profile again and the number of failures has gone down. Fixing the entire control will reduce the number of control failures
To edit permissions in the prod machine, edit the cron.d and cron.daily permissions by using chmod
```sh
chmod o-rwx /etc/cron.d
chmod o-rwx /etc/cron.daily
chmod g-rwx /etc/cron.daily
chmod g-rwx /etc/cron.d
```
To fix the mounting issue, write these lines of code within the prod machine
```sh
tmpfs /dev tmpfs defaults,noexec,nosuid,nodev 0 0
tmpfs                   /dev/shm                tmpfs   defaults,nodev,nosuid,noexec        0 0
mount -o remount,noexec,nosuid,nodev /dev
```
- Explain how you manually (not using any automation) fixed inspec control failures
- Ensure the linux-baseline profile checks for the presence of Antivirus software on
the production machine (if the presence of the Antivirus software is not being
checked in the linux-baseline profile, please include the Antivirus check)
- 10 Bonus points will be awarded if the manual fixes are added to the ansible role
os-hardening (of dev-sec)

To add the file to the role, download the ansible role, find the location and cd into the directory.
```
cd /root/.ansible/roles/dev-sec.os-hardening/tasks
```
Open the hardening role and edit the file to include the cron.yml
```
vi hardening.yml
```
```
- import_tasks: cron.yml  
  tags: cron
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
```
- import_tasks: devmount.yml  
  tags: devmount
````
Create the task in the diretory
```
cat > devmount.yml
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
