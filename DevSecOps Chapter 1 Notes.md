# DevSecOps Chapter 1 Notes
## DevSecOps
DevOps is a set of practices intended to reduce the time between committing a change to a system and the change being placed into normal production, while ensuring high quality.
**By Definition, security is part of DevOps**

**Resilience**
DevOps helps organisations in designing and implementing resilient solutions
**Speed**
Speed is **competitive advantage** and DevOps helps to go to market faster
**Automation**
Automation helps to reduce complexity of modern syste,s and can **scale** as per needs
**Flexibility** 
With ever changing technology, businesses have to be flexible and fast to deliver value to their customers, otherwise **they risk** losing their **business**
**Reliability** 
Customers need more reliable & available systems. DevOps reduces failure rates and provides faster **feedback**

## DevSecOps Implemenation
- **Shift Security Left**
_Use CI/CD pipeline to embed security_
- **Self Service**
_Gives Developers and operations visibility into security activities_
- **Security Champions**
_Encourage security champions to pick security tasks_
- **Everything as Code**
_Compliance as Code and hardening configuration management systems_
- **Secure by Default** 
_Use secure by default frameworks and services_
- **Use Maturity Models**
_Use DevSecOps Maturity Models to improve further_

## DevSecOps Challenges
- Most of the Security tools are not easy to integrate
- Every tool has its own nuances so its not uniform
- Many don't provide an API to use it
- Spidering is a big ussue with DAST Automation
- Handling False Positives locally vs a service

```sh
sast:
  stage: build
  script:
    - docker pull hysnsec/bandit  # Download bandit docker container
    - docker run --user $(id -u):$(id -g) -v $(pwd):/src --rm hysnsec/bandit -r /src -f json -o /src/bandit-output.json
  artifacts:
    paths: [bandit-output.json]
    when: always
  allow_failure: true   #<--- allow the build to fail but don't mark it as such
```
Notice the last line allow_failure: true, its important for DevSecOps.