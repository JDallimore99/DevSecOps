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
## Challenge 3
## Challenge 4
## Challenge 5
