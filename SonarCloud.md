```sh
stages:
- SAST

run-sast-job:
    stage: SAST
    image: maven:3.8.5-openjdk-11-slim
    script: |
     mvn verify package sonar:sonar -Dsonar.host.url=https://sonarcloud.io/ -Dsonar.organization=gitlabdevsecopsintegration3 -Dsonar.projectKey=gitlabdevsecopsintegration3 -Dsonar.login=ed5ceecdd726df79c98b295fa36d498cb698be1d

       
```
