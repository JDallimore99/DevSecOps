# Git Basics
## Working with Git Locally
Git is a well-known version control system AKA source code management system. It is used to store the application code and configurations. With DevOps, we are now storing infrastructure as code, security as code, and other necessary documentation in Git.
## Initial Git setup
To work with git repositories, we need to first setup a username and email.
We can use git config commands to set it up.
```sh
git config --global user.email "student@pdevsecops.com"
git config --global user.name "student"
```
## Download/clone/copy the repository 
We can use the git clone command to download the git repository to a local machine.
```sh
git clone http://root:pdso-training@gitlab-ce-gj1lt9xw.lab.practical-devsecops.training/root/django-nv.git
```
>Notice the username/password combination before the at symbol

By cloning the above repository, we created a local copy of the remote repository.
Lets **cd** into this repository to explore its content.
```sh
cd django-nv
ls -l
```
## Add a file to the repository
Create a file in the repository using the **cat** command
```sh
cat > myfile <<EOL
This is my file
EOL
```
Modify the **README.md**
```sh
echo "Practical DevSecOps" >> README.md
```
Check the status of the repository (that is what files are added, modified, removed)
```sh
git status
**Command Output**
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        myfile

no changes added to commit (use "git add" and/or "git commit -a")
```
As we can see **myfile** is under **Untracked** files, and **README.md** is under **modified**.

We need to use **git add** command to add these two files to store them in the Git repository. Even though, these two files are in the directory, they are not yet added to the repository.
```sh
git add myfile README.md
git status
**Command Output**
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md
        new file:   myfile
```
Adding a file to a git repo is a two-step process.
1. Add the file(s) to the stage
2. Commit the changes to the repository

Step 1 is already complete. Commit the changes using the **git commit** command
```sh
git commit -m "Add myfile and update README.md"
**Command Output**
[master fb7a678] Add myfile and update README.md
 2 files changed, 2 insertions(+)
 create mode 100644 myfile
```
**-m** is a comment that is required while committing changes to Git.
## Pushing Commmits to the Remote Repository
Since Git is a decentralized source code management system, all changes live in your local git repository till you push them to the server. Think it like this, git was meant to run even when you do not have internet connectivity, on flights, vessels, or in a jungle somewhere.
We have internet connectivity, lets push the local changes to the remote git repository using git push command.
```sh 
git push 
```
**Pull the changes from the repository**
Can pull/download these changes using the git pull command.
```sh
git pull
```