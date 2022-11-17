# Secure SDLC and CI/CD Pipelines
## Traditional Secure SDLC
1. Training
2. Requirements
3. Design
4. Implementation
5. Verification
6. Release 
7. Response

## Modern Secure SDLC
1. Plan - Requirements
2. Code - Code Repository
3. Build - CI Server
4. Test - Integration
5. Release - Artefact Repository 
6. Deploy - CD Orchestration
7. Operate  - Monitor

## DevSecOps Maturity Model
**Dynamic Depth**
How deep are dynamic scans executed within a Security DevOps CI chain?
**Static Depth**
How deep is static code analysis performed within a Security DevOps CI chain?
**Intensity**
How intense are the majority of the executed attacks within a Security DevOps CI chain?
**Consolidation**
How complete is the process of handling findings within a Security DevOps CI chain?

**4 Different levels**
- Level 1 - Default tool settings
- Level 2 - Minor tweaks to rule sets

Continous Delivery - Human approval is needed before code is deployed
Continous Deployment - Everythig is automated, including deployment

## GitLab Features
**Version Control**
With DevSecOPs speed, everything has to be stored as code to achieve audit, security and review benefits
**Code Review**
GitLab also helps in reviewing the code with in-built PR/Merge tool
**Continous Integration/Deployment**
Gitlab has a mature CI/CD too which helps in testing, releasing code, rolling back and update/deletion of the infrastructure
**Docker/Kubernetes Integration**
Gitlab also provides nature DOckr and Kubernetes support with in-built docker registry 
## Development Workflows
Pipelines accomodate several development workflows
1. Branch flow (e.g. different branch for dev, qa, staging, production)
git branch feature-la
2. Feature/Trunk-based Flow (e.g. feature branches and single master branch, possibly with tags for release)
3. Fork-based flow (e.g. merge requests come from forks)

## GitLab CI/CD
Implementation of CI/CD which can use to create deployment pipeline for project.
**General Workflow**
Create PM/MR > Build > Unit Test > Integration Test
Merge to Master > Unit Test > Integration Test > Deploy to Staging
Merge to production, Tag for release > Download Artefact > Deploy to Production

1. A pipeline is a system consisting of one or more stages to continously integrate/delivery/deploy software
2. A stage (build,test,deploy) is a combination of jobs to achieve goal of a stage
3. Jobs in a stage are run in parallel and on success, the pipeline moves on to the next stage. If one of the jobs fails, the next stage is not (usually)  executed

## Shared gitlab runners vs Dedicated gitlab runners?
CI/CD systems are built on client and server architecture. There is a server(master) component which oversees projects, manages jobs and delegates job(s) to respective clients(runners).
Runners (clients) takes care of running the job(s) and reporting back the status of the job. Runners (also known as slaves) can be shared among different projects or very specific to a project.
### Shared Runners
Shared runners run jobs which are generic in nature or not tied to a particular environment. By default, runners are shared unless you make them specific to a particular job.
### Dedicated Runners
Dedicated runners/slaves are an important concept from a DevSecOps Engineer's perspective as it will allow us to delegate security scans to a particular runner.
For example, most security tools like Checkmarx, Fortify are hardware bound(tied to Mac/CPU etc.,) and can't be moved easily to a different system. So if you want to run a scan on a specific hardware tied slave, you have to use a dedicated runner.
Dedicated runners can also be categorised based on programming languages(python, ruby, java), build tools (docker) and available hardware resources(RAM, CPU, HDD).


