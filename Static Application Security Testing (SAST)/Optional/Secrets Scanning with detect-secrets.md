# Secrets Scanning with detect-secrets
## Download the source code
We will do all the exercises locally first in DevSecOps-Box, so let’s start the exercise.
First, we need to download the source code of the project from our git repository.
```
git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp
```
**Output**
```
Cloning into 'webapp'...
warning: redirecting to https://gitlab.practical-devsecops.training/pdso/django.nv.git/
remote: Enumerating objects: 228, done.
remote: Total 228 (delta 0), reused 0 (delta 0), pack-reused 228
Receiving objects: 100% (228/228), 1.03 MiB | 1.04 MiB/s, done.
Resolving deltas: 100% (86/86), done.
```
Let’s cd into the application so we can scan the app.
```
cd webapp
```
We are now in the webapp directory
## Install detect-secrets
detect-secrets is a tool that designed with the enterprise client in mind: providing a backwards compatible to preventing new secrets from entering the code base and etc.
You can find more details about the project at https://github.com/Yelp/detect-secrets.
Let’s install the detect-secrets tool on the system to scan for the secrets in our code.
```
pip3 install detect-secrets==1.3.0
```
**Output**
```
Collecting detect-secrets==1.3.0
  Downloading detect_secrets-1.3.0-py3-none-any.whl (115 kB)
     |████████████████████████████████| 115 kB 31.4 MB/s 
Collecting pyyaml
  Downloading PyYAML-6.0-cp38-cp38-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.whl (701 kB)
     |████████████████████████████████| 701 kB 19.1 MB/s 
Requirement already satisfied: requests in /usr/lib/python3/dist-packages (from detect-secrets==1.3.0) (2.22.0)
Installing collected packages: pyyaml, detect-secrets
Successfully installed detect-secrets-1.3.0 pyyaml-6.0
```
Let’s explore what options detect-secrets provides us.
```
detect-secrets --help

usage: detect-secrets [-h] [-v] [--version] [-C <path>] [-c NUM_CORES] {scan,audit} ...

positional arguments:
  {scan,audit}
    scan                Creates a baseline by scanning a repository for secrets.
    audit               Manually assesses a baseline to determine validity of secrets found.

optional arguments:
  -h, --help            show this help message and exit
  -v, --verbose         Verbose mode.
  --version             Display version information.
  -C <path>             Run as if detect-secrets was started in <path>, rather than in the current working directory.
  -c NUM_CORES, --cores NUM_CORES
                        Specify the number of cores to use for parallel processing. Defaults to using the max cores on
                        the current host.
```
## Run the Scanner
As we have learned in the DevSecOps Gospel, we should save the output in the machine-readable format. JSON, CSV, XML formats can be parsed by the machines easily.

Let’s run the detect-secrets command with the scan argument to find any secrets in our code.
```
detect-secrets scan
```
**Output**
```
{
  "version": "1.3.0",
  "plugins_used": [
    {
      "name": "ArtifactoryDetector"
    },
    {
      "name": "AWSKeyDetector"
    },

...[SNIP]...

    "taskManager/settings.py": [
      {
        "type": "Secret Keyword",
        "filename": "taskManager/settings.py",
        "hashed_secret": "877e30c29747a7ef645f02be73bd691e8a386e82",
        "is_verified": false,
        "line_number": 24
      }
    ]
  },
  "generated_at": "2022-08-09T01:26:54Z"
}
```
It seems detect-secrets‘s default output format is JSON format. Let’s store the output/results in a file.
```
detect-secrets scan > secrets-output.json
```
We can also mark the issues/findings as false-positives in the output file using the audit command. You can accept an issue as a real issue with the letter y for yes and n for no.

Go ahead and mark the issues appropriately.
```
echo "q\n" | detect-secrets audit secrets-output.json
```
The echo “q\n” above is just STDIN to automatically quit or exit from the detect-secrets itself.

**Output**
```
Secret:      1 of 6
Filename:    fixtures/users.json
Secret Type: Secret Keyword
----------
4:    "pk": 1,
5:    "fields": {
6:      "first_name" : "",
7:      "last_name" : "",
8:      "username" : "admin",
9:      "password": "md5$oAKvI66ce0Xq$a5c1836db3d6dedff5deca630a358d8b",
10:      "is_superuser" : true,
11:      "email" : "admin@tm.com",
12:      "is_staff" : true,
13:      "is_active" : true
14:    }
----------
Is this a secret that should be committed to this repository? (y)es, (n)o, (s)kip, (q)uit:
```
## GitLab Integration
```
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
   - virtualenv env                       
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

secrets-scanning:
  stage: test
  image: python:3.6
  script: 
    - pip3 install detect-secrets==1.3.0
    - detect-secrets scan > secrets-output.json
  artifacts:
    paths: [secrets-output.json]
    when: always 
  allow_failure: true
#or 
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
