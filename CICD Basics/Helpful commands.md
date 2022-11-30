# Helpful Commands
|Command|Description|
|:---|:-----|
|allow_failure: true|You can use the allow_failure tag to “not fail the build” even though the tool found issues.|
|artifacts|You can store the tool results in a file using the artifacts tag|
|expire_in: 1 week|We can set the amount of time to store the file|
|paths:|Give the path of the scan result files that need to be stored|
|when: manual|We can enforce a human intervention (click the play button in Gitlab) to run a job (deployment).|
|rules: -if|If something is changed, the pipeline will kickstart. Other than that, you can also use any expressions like (==, !=, =~, ~=) and conjunction/disjunction like (&&, \|\|) then combine them with the help of predefined variables from GitLab to make your CI/CD workflow.|
|rules: -changed|Define the rule clause to specify under what rules the job should be executed, e.g. a job named build should only run when a file named Dockerfile is changed. So, if you edit the .gitlab-ci.yml file, then copy the above content and save the changes, it won’t execute the pipeline because there was no change to the Dockerfile.|
|rules: -exists|Specifies that a job will run if something exists. This can be either a specific file or a glob pattern (match multiple files in a directory)|
|ssh root@prod...|Logging into product machine using ssh in terminal|
|cat \/root.ssh/id_rsa|Command to produce the private key|

> Predefined variable reference: https://docs.gitlab.com/ee/ci/variables/predefined_variables.html
