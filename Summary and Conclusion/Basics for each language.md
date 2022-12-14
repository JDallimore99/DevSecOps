# Basics for each language
Include the basics of downloading and running for each language (Python, Ruby, Java and Node.JS)

## Python
Installing and using Python scanners
### Terminal
Install
```sh
pip3 install safety==2.1.1
```
Scan
```sh
safety check -r requirements.txt --json | tee safety-output.json
```
### GitLab CI/CD Pipeline
```sh
test:
  stage: test
  script:
    # We are going to pull the hysnsec/safety image to run the safety scanner
    - docker pull hysnsec/safety
    # third party components are stored in requirements.txt for python, so we will scan this particular file with safety.
    - docker run --rm -v $(pwd):/src hysnsec/safety check -r requirements.txt --json > oast-results.json
  artifacts:
    paths: [oast-results.json]
    when: always # What does this do?
  allow_failure: true
  ```
Focus on using docker pull to get the download and get the tool. This is found in hysnsec.  
Focus on running the scanner using dokcer run. We need to share the project’s source code inside the container for it to access these files. We can do that using the -v option of the docker command, including the path ```-v $pwd:/src```. The option mounts the current directory in the host (runner) to /src inside the container. This could also be -v /home/ubuntu/code:/src or c:\Users\student\code:/src if you were using windows.

## NodeJS
### Terminal
Install
```sh
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt install nodejs -y
npm install -g retire@3.0.6
npm install
```
Run the scanner
```sh
retire --outputformat json --outputpath retire_output.json
cat retire_output.json
```
Can use .retireignore.json to mark false positives and then use this to remove false positives in further scans.
```sh
retire --severity high --ignorefile .retireignore.json --outputformat json --outputpath retire_output.json
```
### GitLab CI/CD Pipeline
```sh
oast-frontend:
  stage: test
  image: node:alpine3.10
  script:
    - npm install
    - npm install -g retire # Install retirejs npm package.
    - retire --outputformat json --outputpath retirejs-report.json --severity high
  artifacts:
    paths: [retirejs-report.json]
    when: always # What is this for?
    expire_in: one week
```

## Java
### Terminal
Install 
```sh
apt update
apt install openjdk-8-jre -y
wget -O /opt/v6.1.6.zip https://github.com/jeremylong/DependencyCheck/releases/download/v6.1.6/dependency-check-6.1.6-release.zip
unzip /opt/v6.1.6.zip -d /opt/
export PATH=/opt/dependency-check/bin:$PATH
```
Run the scan
```sh
dependency-check.sh --scan /webapp --format "CSV" --project "Webgoat" --failOnCVSS 8 --out /opt
```
### GitLab CI/CD Pipeline
Install/push (done in terminal first)
```sh
git clone https://github.com/WebGoat/WebGoat.git webgoat
cd webgoat
git remote rename origin old-origin
git remote add origin http://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/webgoat.git
git push -u origin --all
Username for 'http://gitlab-ce-acsrq8h9.lab.practical-devsecops.training': root
Password for 'http://root@gitlab-ce-acsrq8h9.lab.practical-devsecops.training':
```
Create a run-depcheck file
```sh 
#!/bin/sh

DATA_DIRECTORY="$PWD/data"
REPORT_DIRECTORY="$PWD/reports"

if [ ! -d "$DATA_DIRECTORY" ]; then
  echo "Initially creating persistent directories"
  mkdir -p "$DATA_DIRECTORY"
  chmod -R 777 "$DATA_DIRECTORY"

  mkdir -p "$REPORT_DIRECTORY"
  chmod -R 777 "$REPORT_DIRECTORY"
fi

cd webgoat-container

# Make sure we are using the latest version
docker pull owasp/dependency-check

# mvn install -Dmaven.test.skip=true
docker run --rm \
  --volume $(pwd):/src \
  --volume "$DATA_DIRECTORY":/usr/share/dependency-check/data \
  --volume "$REPORT_DIRECTORY":/report \
  owasp/dependency-check \
  --scan /src \
  --format "CSV" \
  --project "Webgoat" \
  --failOnCVSS 4 \
  --out /report
```
Add to CI/CD yaml file
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
  script:
    - echo "This is a build step"

test:
  stage: test
  script:
    - echo "This is a test step"

odc-backend:
  stage: test
  image: gitlab/dind:latest
  script:
    - chmod +x ./run-depcheck.sh
    - ./run-depcheck.sh
  artifacts:
    paths:
      - reports/dependency-check-report.csv
    when: always
    expire_in: one week

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
