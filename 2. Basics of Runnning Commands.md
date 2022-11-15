# Basics of Running Commands
1. **Directory Listing**
ls is the Linux command that helps in listing files and directories
```sh
ls
```

```sh
**Command Output**
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```
Try **ls** with additional options such as **-l**. This option displays one file per line with extra details
```sh
ls -l
```
```sh
**Command Output**
total 76
drwxr-xr-x   1 root   root    4096 Jun 20 09:28 bin
drwxr-xr-x   2 root   root    4096 Apr 24  2018 boot
drwxr-xr-x   5 root   root     380 Jul  5 14:57 dev
drwxr-xr-x   1 root   root    4096 Jul  5 14:57 etc
drwxr-xr-x   2 root   root    4096 Apr 24  2018 home
drwxr-xr-x   1 root   root    4096 Jul  5 14:57 lib
drwxr-xr-x   2 root   root    4096 Jun 15 10:24 lib64
drwxr-xr-x   2 root   root    4096 Jun 15 10:21 media
drwxr-xr-x   2 root   root    4096 Jun 15 10:21 mnt
drwxr-xr-x   1 root   root    4096 Jul  5 14:57 opt
dr-xr-xr-x 458 root   root       0 Jul  5 14:57 proc
drwx------   1 root   root    4096 Jun 20 06:22 root
drwxr-xr-x   1 root   root    4096 Jul  5 14:57 run
drwxr-xr-x   1 root   root    4096 Jun 20 09:28 sbin
drwxr-xr-x   2 root   root    4096 Jun 15 10:21 srv
dr-xr-xr-x  13 nobody nogroup    0 Jul  5 14:57 sys
drwxrwxrwt   1 root   root    4096 Jun 20 09:28 tmp
drwxr-xr-x   1 root   root    4096 Jun 20 09:28 usr
drwxr-xr-x   1 root   root    4096 Jun 15 10:24 var
```

**ls** is a read operation. Suppose there are no files and directories which you are searching for then it returns nothing except an error message
```sh
ls -l abracadabra
```
```sh
**Command Output**
ls: cannot access 'abracadabra': No such file or directory
```

2. **Create a new directory and look for an output**
**mkdir** is a command to make a new directory
```sh
mkdir my-directory
```
Successful command execution doesn’t indicate any message whereas a message appears in the case of unsuccessful command execution.

3. **Removing a directory**
**rmdir** is the command used to remove (or delete) a directory
```sh
rmdir my-directory
```
Now, running it again you see an error message.
This is because at first you removed a directory and now it doesn’t exist. So if you run it again it cannot find that directory hence displaying the error message.

## Blocking and non-blocking command runs
Sometimes commands can take longer to execute due to the size of the file or package. You may avoid waiting for long until the package gets installed. Let’s take an example

1. **Try a long running operation, Eg: Installing Ansible**
Install Ansible using the below command, do not worry you will learn about it later in the course.
```sh
pip3 install ansible==2.10.4 ansible-lint==4.3.7
```
2. **Try a long running operation in the background, Eg: Installing Ansible**
Instead of waiting you may use & (ampersand) at the end of the command. It simply runs the process in the background allowing you to do other tasks.
Now run the below command to see the output:
```sh
pip3 install ansible==2.10.4 ansible-lint==4.3.7 &
```
