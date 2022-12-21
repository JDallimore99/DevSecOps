# How to Embed Checkov into CI/CD pipeline
## Simple CI/CD Pipeline
Create a new projects by visiting https://gitlab-ce-hh3rj0pi.lab.practical-devsecops.training/projects/new#blank_project, give the project name terraform and click Create project button. Once done we need to push our source code into GitLab, let’s download the code using git clone in DevSecOps Box.
```sh
git clone https://gitlab.practical-devsecops.training/pdso/terraform.git terraform
cd terraform
```
Rename git url to the new one.
```sh
git remote rename origin old-origin
git remote add origin http://gitlab-ce-hh3rj0pi.lab.practical-devsecops.training/root/terraform.git
```
Then, push the code into the repository.
```sh
git push -u origin --all
```
And enter the GitLab credentials.
```sh
Username for 'http://gitlab-ce-hh3rj0pi.lab.practical-devsecops.training': root
Password for 'http://root@gitlab-ce-hh3rj0pi.lab.practical-devsecops.training': pdso-training
```

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
  script:
    - echo "This is a build step"

test:
  stage: test
  script:
    - echo "This is a test step"

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
There are four jobs in this pipeline, a build job, a test job, a integration job, and a prod job.
Login into the GitLab using the following details and execute this pipeline.
> Note: When in the exam, make sure the gitlab is on the right branch. By commiting code to the master branch, changes can be made and a pipeline run. If changes are made in main branch, no pipeline will run and therefore won't work.

## Embed Checkov in CI/CD pipeline
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

checkov:
  stage: validate
  script:
    - docker pull bridgecrew/checkov
    - docker run --rm -w /src -v $(pwd):/src bridgecrew/checkov -d aws -o json | tee checkov-output.json
  artifacts:
    paths: [checkov-output.json]
    when: always

test:
  stage: test
  script:
    - echo "This is a test step"

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

## Allow Job Failure
Append allow_failure: true to the end of the Checkov job
```sh
checkov:
  stage: validate
  script:
    - docker pull bridgecrew/checkov
    - docker run --rm -w /src -v $(pwd):/src bridgecrew/checkov -d aws -o json | tee checkov-output.json
  artifacts:
    paths: [checkov-output.json]
    when: always
  allow_failure: true   #<--- allow the build to fail but don't mark it as such
```

