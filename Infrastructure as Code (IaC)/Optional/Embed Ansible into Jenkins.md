```sh
pipeline {
    agent any

    options {
        gitLabConnection('gitlab')
    }

    stages {
        stage("build") {
            agent {
                docker {
                    image 'python:3.6'
                    args '-u root'
                }
            }
            steps {
                sh """
                pip3 install --user virtualenv
                python3 -m virtualenv env
                . env/bin/activate
                pip3 install -r requirements.txt
                python3 manage.py check
                """
            }
        }
        stage("test") {
            agent {
                docker {
                    image 'python:3.6'
                    args '-u root'
                }
            }
            steps {
                sh """
                pip3 install --user virtualenv
                python3 -m virtualenv env
                . env/bin/activate
                pip3 install -r requirements.txt
                python3 manage.py test taskManager
                """
            }
        }
        stage("integration") {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    echo "This is an integration step"
                    sh "exit 1"
                }
            }
        }
        stage("prod") {
            steps {
                input "Deploy to production?"
                echo "This is a deploy step."
            }
        }
    }
    post {
        failure {
            updateGitlabCommitStatus(name: "\${env.STAGE_NAME}", state: 'failed')
        }
        unstable {
            updateGitlabCommitStatus(name: "\${env.STAGE_NAME}", state: 'failed')
        }
        success {
            updateGitlabCommitStatus(name: "\${env.STAGE_NAME}", state: 'success')
        }
        aborted {
            updateGitlabCommitStatus(name: "\${env.STAGE_NAME}", state: 'canceled')
        }
        always { 
            deleteDir()                     // clean up workspace
            dir("${WORKSPACE}@tmp") {       // clean up tmp directory
                deleteDir()
            }
            dir("${WORKSPACE}@script") {    // clean up script directory
                deleteDir()
            }
        }
    }
}
```
## Embed Ansible
Embed Ansible in Jenkins
Remember!

Except for DevSecOps-Box, every other machine closes after two hours, even if you are in the middle of the exercise
After two hours, in case of a 404, you need to refresh the exercise page and click on Start the Exercise button to continue working
We need to add credentials into Jenkins, namely, SSH Private Key and SSH hostname of the machine we are securing, i.e., production machine. Since we don’t want the credentials to be hardcoded in the Jenkinsfile. Let’s add the creds to the Jenkins credential store by visiting https://jenkins-acsrq8h9.lab.practical-devsecops.training/credentials/store/system/domain/_/.

Click on the Add Credentials link on the left sidebar, and select SSH Username with private key as Kind, then add the following credentials into it.

|Name|	Value|
|:----|:----|
|ID|	ssh-prod|
|Description|	Credentials to login into Production machine|
|Username	|root|
|Private Key	|check Enter directly, click the Add button and copy the private key from Production machine, available at /root/.ssh/id_rsa|
|Passphrase	|leave it blank because we want this process to be automatic without any human intervention. If you desire more robust credential security mechanism, please use dedicated secret management systems like Hashicorp Vault.|

Once done, click the OK button.

Add another credential to save SSH hostname, so it’s not hardcoded in the Jenkinsfile, then select Secret text as Kind, then add the following credentials into it.

|Name|	Value|
|:----|:-----|
|Secret	|prod-acsrq8h9|
|ID	|prod-server|

Next, go back to the GitLab tab and append the following code in the Jenkinsfile after the prod stage.

```
        stage("ansible-hardening"){
            agent {
                docker {
                    image 'willhallonline/ansible:2.13-ubuntu-20.04'
                    args '-u root'
                }
            }
            steps {
                sshagent(['ssh-prod']) {
                    withCredentials([string(credentialsId: 'prod-server', variable: 'DEPLOYMENT_SERVER')]) {
                        sh """
                        echo "[prod]\n$DEPLOYMENT_SERVER" >> inventory.ini
                        ansible-galaxy install dev-sec.os-hardening
                        ansible-playbook -i inventory.ini ansible-hardening.yml
                        """
                    }
                }
            }
        }
```
Save changes to the file using the Commit changes button.

We can see the results of this pipeline by visiting https://jenkins-acsrq8h9.lab.practical-devsecops.training/job/django.nv.

Click on the appropriate build history to see the output.

Oh, it failed? Why? We also got the following helpful message in the CI output.

+ ansible-playbook -i inventory.ini ansible-hardening.yml
ERROR! the playbook: ansible-hardening.yml could not be found
script returned exit code 1
That makes sense. We didn’t upload the ansible-hardening.yml to the git repository.

Let’s copy the hardening script.
```
---
- name: Playbook to harden the Ubuntu OS.
  hosts: prod
  remote_user: root
  become: yes

  roles:
    - dev-sec.os-hardening
```
Visit the add new file URL https://gitlab-ce-acsrq8h9lab.practical-devsecops.training/root/django-nv/-/new/master/.

If you are comfortable with the Git command line, please use git add, git commit, and git push commands.

Ensure you name the file as
ansible-hardening.yml
__{: .smallcopy}.

Save changes to the repo using the Commit changes button.

We can see the results by visiting https://gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv/pipelines.

Click on the appropriate job name to see the output.

There you have it. We ran ansible hardening locally first and then embedded it into a CI/CD pipeline.
