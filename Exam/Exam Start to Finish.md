
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
