# Helpful Commands
- **allow_failure: true** 

You can use the allow_failure tag to “not fail the build” even though the tool found issues.
- **artifacts** 

You can store the tool results in a file using the artifacts tag
- **expire_in: 1 week**

We can set the amount of time to store the file
- **paths:**

Give the path of the scan result files that need to be stored
- **when: manual**

We can enforce a human intervention (click the play button in Gitlab) to run a job (deployment).
