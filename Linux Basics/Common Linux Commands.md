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
|jq|Pipe to jq|```echo '{"cloudprovider":{"name":"AWS","url":"www.aws.com"}} ' | jq '.'```|
