# Linux Commands
|Command|Description|Example|
|:----|:----|:----|
|ls|Directory Listing|```ls```|
|-l|Displays one file per line with extra details|```ls -l```|
|mkdir|Command to make a new directory|```mkdir my-directory```|
|rmdir| command used to remove/delete directory|```rmdir my-directory```|
|&|Runs the process in the background allowing you to do other tasks|```pip3 install ansible==2.10.4 ansible-lint==4.3.7 &```|
|pwd|Explore the current directory that you are in|```pwd```|
|cd|You can change the current working directory|```cd new-directory```|
|cd ..|Can go back to the previous/parent directory|```cd ..```|
|nano|Text editor|```nano myfile```|
|cat|Reads the content of a file and displays as output|```cat myfile```|
|mv|Rename a file|```mv myfile newfile```|
|cat >|Create a file in a non-interactive way using command line magic: 1. The filename, here its filename, 2. The here document, the << symbols are a special code block. It allows you to redirect anything between EOL into a command, 3. The cat command, if it’s followed by >, allows you to create a file|```cat > filename <<EOL Some text content Some text content 2 Some text content 3 EOL```|
|cat /|Redirecting output of a command to a file/command|```cat /etc/passwd > mypasswd.txt```|
|echo|Look at contents of a variable or create smaller files|```echo "this is a string"```|
|cut|Use cut to extract usernames when used with cat|```cat mypasswd.txt \| cut -d ':' -f 1```|
|-d|Delimeter flag|```cut -d ':' -f 1```|
|-f|Field option|```cut -d ':' -f 1```|
|ps|Get a list of running processes|```ps -aux```|
|grep|Identify a particular process|```ps -aux \| grep bash```|
|-l|Verify the permissions of the file|```ls -l myfile```|
|stat|Verify permissions|```stat myfile```|
|chmod|Give file executable permissions|```chmod +x myfile```|
|sudo|Run a command with root priveleges|```sudo ls```|
|--gecos|The gecos field, or GECOS field is a field in each record in the /etc/passwd file on Unix and similar operating systems.|```echo -e "pdevsecops\npdevsecops" \| adduser --gecos "" john```|
|usermod|Add user to a sudo group|```usermod -aG sudo john```|
|echo $?|Return an exit code|```echo $?```|
|touch|Create new file|```touch newfile```|
|git config|Create usernames and emails to work with git repositories|```git config --global user.email "student@pdevsecops.com"```|
|git clone|Download the git repository to a local machine|```git clone http://root:pdso-training@gitlab-ce-acsrq8h9.lab.practical-devsecops.training/root/django-nv.git```|
|git status|Check the status of the repository (what files are added, modified, removed|```git status```|
|git add|Add files to the repository|```git add myfile README.md```|
|git commit|Commit files to the repository|```git commit -m "Add myfile and update README.md"```|
|-m|Is a comment REQUIRED while commiting changes to Git|```git commit -m "Add myfile and update README.md"```|
|git push|Push the local changes to the remote git repository|```git push```|
|git pull|Pull/download these changes using the git pull command|```git pull```|
|ssh|Login to a remote machine|```ssh -o "StrictHostKeyChecking=no" -i ~/.ssh/id_rsa root@prod-acsrq8h9```|
|ssh-keyscan|Utility for gathering the public SSH host keys of a number of hosts|```ssh-keyscan -t rsa prod-acsrq8h9 >> ~/.ssh/known_hosts```|
|hostname|Verify we are in the remote machine|```hostname```|
|ssh root@[prod machine]|Run commands remotely, anything in quotes is run on the remote machine|```ssh root@prod-acsrq8h9 "hostname"```|
|ssh restart|Restart the ssh server is required|```/etc/init.d/sshd restart```|
|ssh-keygen|Generates a new public-private|```ssh-keygen -t rsa```|
|ssh-copy-id|Helps in copying public keys to the authorized_keys file|```ssh-copy-id -i ~/.ssh/id_rsa.pub user@targetserver```|
|jq|Pipe to jq|```echo '{"cloudprovider":{"name":"AWS","url":"www.aws.com"}} ' \| jq '.'```|
|curl|command line tool that enables data transfer over various network protocols. It communicates with a web or application server by specifying a relevant URL and the data that need to be sent or received|```curl https://randomuser.me/api/```|
|jq '.'|Used to prettify a JSON file|```curl https://randomuser.me/api/ \| jq '.'```|
|vi|Used to access the visual editor|```vi security-report.json```|
|:q|Exits the vi editor|```:q```|
|/|Used for searching in vi editor|```/High```|
|d|Used to delete lines/text in vi editor|```d```|
|nano|Open nano editor|```nano security-report.json```|
---
# Docker Commands
|Command|Description|Example|
|:----|:---|:----|
|docker pull|to download the docker image from the docker registry|```docker pull ubuntu```|
|--quiet/-q|hide the output|```docker pull -q alpine```|
|-d|Run a container in the background|```docker run -d --name myubuntu ubuntu```|
|-interactive/-i|Keep the container running|```docker run -d --name myubuntu -i ubuntu```|
|docker rm|remove the previous container|```docker rm myubuntu```|
|docker run|start a container from the docker image.|```docker run -d --name myubuntu ubuntu```|
|docker ps|list|```docker ps```|
|docker exec|execute command inside container or to get the shell in the container|```docker exec myubuntu whoami```|
|-a|See all containers|```docker ps -a```|
|docker top|display running processes inside container|```docker top myubuntu```|
|docker stop|Stop the container|```docker stop webserver && docker rm webserver```|
|-p|Bind container port to all IP addresses on the host|```docker run -d --name=webserver --env APP=nginx -p 80:80 nginx:1.21.3```|
|--name|Names a container|```docker run -d --name webserver --env APP=nginx nginx:1.21.3```|
|-e, --env|Sets the environment variables|```docker run -d --name webserver --env APP=nginx nginx:1.21.3```|
|docker build|Create a docker image|```docker build -t django.nv:1.0 .```|
|docker images|See the docker image|```docker images```|
|dockre tags|Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE|```docker tag django.nv:1.0 django.nv:1.1```
|docker rmi|Remove docker image|```docker rmi django.nv:1.0```|
|docker volume|Manage docker volumes|```docker volume --help```|
|docker volume create|Create a volume|```docker volume create demo```|
|-v /opt:/opt|Bind mounts have been around since the early days of Docker. Bind mounts have limited functionality compared to volumes. When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine.|```docker run --name ubuntu2 -d -v /opt:/opt -it ubuntu:18.04```|
|tmpfs|This tmpfs mount will not save the data persistently as it uses host’s memory(RAM) as a temporary storage.||
|registry|Store docker images in a docker repository|```docker run -d -p 5000:5000 --restart=always --name registry registry:2```|
|docker network|networking in docker||
|docker network create|Create a new docker network|```docker network create mynetwork```|
|docker inspect|Explore more details about the network e.g. subnet aand gateway assigned|```docker inspect mynetwork```|
|--network|attach the network to a container|```docker run -d --name ubuntu --network mynetwork -it ubuntu:18.04```|
|connect|Connect a pre-existing container to a netowrk|
|apt update|Test the network connectivity in the container|```docker exec ubuntu apt update```|
|--driver|Create a known network such as macvlan|```docker network create --driver macvlan mymacvlan```|
|none|Isolates/disable the network in the containers|```docker run -d --name ubuntu --network=none -it ubuntu:18.04docker run -d --name ubuntu --network=none -it ubuntu:18.04```|
|service|Start the service|```service nginx start```|
|apt install|install utilities and commands|```apt install curl -y```|
|docker logs| Can use this to view the logs of a docker container|```docker logs custom-nginx-container```|
|sleep infinity|Used in advanced docker file to prevent Exited status. It tells the container to keep the process alive.|```CMD [";sleep infinity"] EOL```|

# CICD Commands
|Command|Description|Example|
|:---|:-----|:-----|
|stage|Specifies which stage the job is set to|```stage: build```|
|script|Specifies what actions the job will take|```script:```|
|allow_failure|You can use the allow_failure tag to “not fail the build” even though the tool found issues.|```allow_failure: true```|
|artifacts|You can store the tool results in a file using the artifacts tag|```artifacts:```|
|expire_in:|We can set the amount of time to store the file|```expire_in: 1 week```|
|paths:|Give the path of the scan result files that need to be stored|```paths: [output.json]```|
|when:|We can enforce a human intervention (click the play button in Gitlab) to run a job (deployment).|```when: manual```|
|rules: -if|If something is changed, the pipeline will kickstart. Other than that, you can also use any expressions like (==, !=, =~, ~=) and conjunction/disjunction like (&&, \|\|) then combine them with the help of predefined variables from GitLab to make your CI/CD workflow.|```rules: - if: '$CI_COMMIT_BRANCH == "master"'   # this job will get triggered when the branch name is __master__```|
|rules: -changed|Define the rule clause to specify under what rules the job should be executed, e.g. a job named build should only run when a file named Dockerfile is changed. So, if you edit the .gitlab-ci.yml file, then copy the above content and save the changes, it won’t execute the pipeline because there was no change to the Dockerfile.|```rules: - changes:   - Dockerfile```|
|rules: -exists|Specifies that a job will run if something exists. This can be either a specific file or a glob pattern (match multiple files in a directory)|```rules:  - exists: - Dockerfile```|
|ssh root@prod...|Logging into product machine using ssh in terminal|```ssh root@prod-acsrq8h9```|
|cat /root.ssh/id_rsa|Command to produce the private key|```cat /root/.ssh/id_rsa```|
|echo|Outputs text|```- echo "This is a deploy step."```|

> Predefined variable reference: https://docs.gitlab.com/ee/ci/variables/predefined_variables.html

# SCA commands
|Command|Description|Example|
|:---|:-----|:---|
|git clone|Used for donwloading source code in terminal|```git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp```|
|pip3 install|Install safety tools|```pip3 install safety==2.1.1```|
|safety check|The command used to complete the safety software component analysis check|```safety check -r requirements.txt --json \| tee safety-output.json```|
|tee|Show the output and store it in a file simultaneously|```safety check -r requirements.txt --json \| tee safety-output.json```|
|-r|Flag used to specify the input file|```safety check -r requirements.txt --json \| tee safety-output.json```|
|--json|Flag tells that output should be in the json format|```safety check -r requirements.txt --json \| tee safety-output.json```|
|docker pull|The docker pull command serves for downloading Docker images from a registry. By default, the docker pull command pulls images from Docker Hub, but it is also possible to manually specify the private registry to pull from.|
|docker run|When you use the basic run command, Docker automatically generates a container name with a string of randomly selected numbers and letters. Since there is a slim chance you will be able to remember or recognize the containers by these generic names, consider setting the container name to something more memorable.|
|curl|A command line tool that enables data transfer over various network protocols|```curl -sL https://deb.nodesource.com/setup_12.x \| bash -```|
|apt install|Install deb packages on Ubuntu, Debian, and related Linux distributions.|```apt install nodejs -y```|
|npm install|This command installs a package, and any packages that it depends on. If the package has a package-lock or shrinkwrap file, the installation of dependencies will be driven by that, with an npm-shrinkwrap.json taking precedence if both files exist.|```npm install -g retire@3.0.6```|
|cat|Explore the contents of a file|```cat package.json```|
|retire|Command to run the retire software component analysis|```retire --outputformat json --outputpath retire_output.json```|
|--outputpath|flag specifies the output file path|```retire --outputformat json --outputpath retire_output.json```|
|--outputformat |flag specifies the output format|```retire --outputformat json --outputpath retire_output.json```|
|\| jq|The JQ command is used to transform JSON data into a more readable format and print it to the standard output on Linux. The JQ command is built around filters which are used to find and print only the required data from a JSON file.|```cat retire_output.json \| jq```|
|--severity|Specify the bug severity level from which the process fails. Allowed levels none, low, medium, high,|```retire --severity high --outputformat json --outputpath retire_output.json```|
|--ignorefile|Custom ignore file, defaults to .retireignore / .retireignore.json|```retire --severity high --ignorefile .retireignore.json --outputformat json --outputpath retire_output.json```|

# SAST commands
|Command|Description|Example|
|:----|:----|:----|
|Git clone|Download source code from git repository|```git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp```|
|pip3 install|Install security tool on the system|```pip3 install trufflehog==2.2.1```|
|trufflehog|Command to run the trufflehog secrets security scanner|```trufflehog --json . \| tee secret.json```|
|bandit|Command to run the bandit security issue scanner|```bandit -r . -f json \| tee bandit-output.json```|
|apt update|Updates advanced package tool|```apt update```|
|apt install|Installs an advanced package|```apt install ruby-full -y```
|gem install|Used for installing brakeman. It is a ruby command|```gem install brakeman -v 5.2.1```|
|brakeman|Command to run the brakeman scan on Rails code|```brakeman -f json \| tee result.json```|
|-i|used to ignore warnings in a scan|```brakeman -f json -i brakeman.ignore \| tee result.json```|

# DAST Commands
|Command|Description|Example|
|:----|:----|:----|
|apt install|Install advanced package tool|```apt install -y libnet-ssleay-perl```|
|git clone|Download source code from git repository|```git clone https://github.com/sullo/nikto```|
|./nikto.pl|Command to run the Nikto scanner|```./nikto.pl -output nikto_output.xml -h prod-acsrq8h9```|
|-h|Flag used to set the target application which we want to scan|```./nikto.pl -output nikto_output.xml -h prod-acsrq8h9```
|-output|Flag used to set the output file in which we want to store the result|```./nikto.pl -output nikto_output.xml -h prod-acsrq8h9```|
|cat|Show the output file|```cat nikto_output.xml```|
|-list-plugins|shows a list of the available plugins|```./nikto.pl -list-plugins```|
|-Plugins \<plugin-name\>|Run a nikto plugin scan against a production machine|```./nikto.pl -h prod-acsrq8h9 -Plugins "@@default;dictionary(dictionary:/nikto/program/databases/db_dictionary)"```|
|nikto.config|Congigure nikto scan|```cat >/nikto/program/nikto.conf<<EOF SKIPPORTS=21 22 111 SKIPIDS=00035 00045 CLIOPTS=-output result.csv -Format csv EOF```|
|SKIPPORTS|Used to remove ports that would never be scanned|```SKIPPORTS=21 22 111```|
|SKIPIDS|Remove false positives|```SKIPIDS=00035 00045```|
|-output|Save output|```CLIOPTS=-output result.csv -Format csv```|
|apt-get update|Gets the update for the advanced package tool|```apt-get update```|
|apt-get install|Installs the advanced package tool|```apt-get install nmap -y```|
|nmap|Commmand to run the nmap port scanner|```nmap prod-acsrq8h9 -oX nmap_out.xml```|
|-oX|Flag used to tell the too that the output should be saved in a certain format|```nmap prod-acsrq8h9 -oX nmap_out.xml```|
|pip3 install|Used to install a tool|```pip3 install sslyze==5.0.3```|
|sslyze|Command used to run the ssyle ssl scanning tool|```sslyze --json_out sslyze-output.json prod-acsrq8h9.lab.practical-devsecops.training:443```|
|--json_out|Used to store the output in json format|```sslyze --json_out sslyze-output.json prod-acsrq8h9.lab.practical-devsecops.training:443```Z
|docker pull|Used to pull ZAP's stable docker image|```docker pull owasp/zap2docker-stable:2.10.0```|
|docker run|Used to run the ZAP scanning tool, zap-baseline.py|```docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training```|
|-j|Output in JSON format|```docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training -J zap-output.json```|

# IaC Commands
|Command|Description|Example|
|:---|:---|:----|
|pip3 install|install programs such as Ansible|```pip3 install ansible==6.4.0```|
|inventory file|Using cat to create an inventory file to define a list of hosts that can be sorted|```cat > inventory.ini <<EOL [devsecops] devsecops-box-acsrq8h9 [sandbox] sandbox-acsrq8h9 [prod] prod-acsrq8h9 EOL ```|
|ansible|Use the ansible command|```ansible -i inventory.ini prod --list-hosts```|
|--version|See if there is any default configuration for Ansible|```ansible --version```|
|mkdir|Create a configuration file by making a directory|```mkdir /etc/ansible/```|
|ansible.cfg|Create a file that is used as a configuration file, allwing for definition of inventory settings|```cat > /etc/ansible/ansible.cfg <<EOF [defaults] stdout_callback = yaml deprecation_warnings = False host_key_checking = False retry_files_enabled = False inventory = /inventory.ini EOF```|
|ssh-keyscan|ensure the SSH’s yes/no prompt is not shown while running the ansible commands, so let’s use the ssh-keyscan to capture the key signatures beforehand|```ssh-keyscan -t rsa devsecops-box-acsrq8h9 sandbox-acsrq8h9 prod-acsrq8h9 >> ~/.ssh/known_hosts```|
|ping|test the remote nodes or hosts and make sure they respond back using Ansible’s default SSH channel|```ansible -i inventory.ini all -m ping```|
|shell|Run hostname commmand on all machines|```ansible -i inventory.ini all -m shell -a "hostname"```|
|apt|used to install a package inside a remote host|```ansible -i inventory.ini all -m apt -a "name=ntp"``` or ```ansible -i inventory.ini prod -m apt -a "name=ntp state=present"```|
|copy|An ad-hoc command used to copy a file into all remote machines|```ansible -i inventory.ini all -m copy -a "src=/root/notes dest=/root"```|
|grep|Use to find modules within ansible commands|```ansible-doc -l \| grep shell```|
|"uptime"|Used to find the uptime on production machines using shell module|```ansible -i inventory.ini prod -m shell -a "uptime"```|
|playbook|Create a playbook file that can be used to run against the production environment|```cat > playbook.yml <<EOL --- - name: Example playbook to install firewalld   hosts: prod   remote_user: root  become: yes  gather_facts: no   vars:     state: present    tasks:  - name: ensure firewalld is at the latest version     apt:       name: firewalld  EOL```|
|ansible-playbook|Command to run the playbook against a prod machine|```ansible-playbook -i inventory.ini playbook.yml```|
|ansible-galaxy|Galaxy provides pre-packaged units of work known to Ansible as roles and collections.|```ansible-galaxy install secfigo.terraform```|
|install|Used to install roles within ansible|```ansible-galaxy install secfigo.terraform```|
|search|Can search for desired roles within ansible using search option|```ansible-galaxy search terraform```|
|echo|Used to copy contents of private key variable stored in Gitlab CI into the id_rsa file under the ~/.ssh inside the container|```- echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" \| tr -d '\r' > ~/.ssh/id_rsa```|
|eval|eval runs the command ssh-agent in the background and sends the key whenever SSH asks for a key in an automated fashion.|```- eval "$(ssh-agent -s)"```|


To find the available modules, you can check out this link (https://docs.ansible.com/ansible/2.8/modules/list_of_all_modules.html) or use the local help command-line tool.
