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
