# Basics of File Permission
## Understanding File Permissions
The permissions of a file or a directory could be reviewed using the **ls -l** or the **stat** command. You can provide executable permissions to a file using the **chmod** command.

You can verify the permissions of a file using the **ls -l** command, where -l stands for the long list.
```sh
ls -l myfile
```
You can also verify the permissions if a file using the **stat** command
```sh
stat myfile
```
You can give this file executable permissions using the **chmod** command.
```sh
chmod +x myfile
```
You can verify the executable permissions using the ls -l or the stat command
```sh
ls -l myfile
or 
stat myfile

**Command Output**
-rwxr-xr-x 1 root root 3 Aug 13 20:36 myfile
or
...
```
Notice the letter **x** in the command output. It signifies the executable permissions that we just added.

You can run this file by using the following command.
```sh
./myfile

**Command Output**
bin   dev  home  lib64  mnt     opt   root  sbin  sys  usr
boot  etc  lib   media  myfile  proc  run   srv   tmp  var
```
Since we have **ls** inside **myfile**, it shows us the directory list.
> Note The dot (./) at the beginning of the command tells the Linux that this executable file is in the current directory.

## Running Commands as Different Users
The sudo command allows you to run programs and utilities as another user (by default, as the superuser). An administrator can give temporary or permanent permission to escalate permissions using the **sudoers** file. This feature helps administrators to delegate some of the permissions without sharing the **root** password.
```sh
sudi ls
```
Since we are already the root user, you wouldn’t notice any significant differences. Let’s create a user called john.
```sh
echo -e "pdevsecops\npdevsecops" | adduser --gecos "" john

**Commmand Output**
Adding user `john' ...
Adding new group `john' (1000) ...
Adding new user `john' (1000) with group `john' ...
Creating home directory `/home/john' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: Retype new UNIX password: passwd: password updated successfully
```
We can use the **usermod** command to add the user to the **sudo** group.
```sh
usermod -aG sudo john
```
Let’s become john by using the sudo command.
```sh
sudo su - john

**Command Output**
To run a command as administrator (user "root"),   "sudo <command>".
See "man sudo_root" for details.
john@devsecops-box-gj1lt9xw:~$
```
Reading the content of a very sensitive file on Linux, i.e., **/etc/shadow.**
```sh
cat /etc/shadow
```
**Permission denied**. Even though we are logged in as john we still have to explicitly specify **sudo** before the actual command to try performing an operation as a high privileged user.

So let’s try reading the content of /etc/shadow again, but this time with **sudo** prefix.
```sh
echo pdevsecops | sudo -S cat /etc/shadow

**Command Output**
root:*:18526:0:99999:7:::
daemon:*:18526:0:99999:7:::
bin:*:18526:0:99999:7:::
sys:*:18526:0:99999:7:::
sync:*:18526:0:99999:7:::
games:*:18526:0:99999:7:::
man:*:18526:0:99999:7:::
lp:*:18526:0:99999:7:::
mail:*:18526:0:99999:7:::
news:*:18526:0:99999:7:::
uucp:*:18526:0:99999:7:::
proxy:*:18526:0:99999:7:::
www-data:*:18526:0:99999:7:::
backup:*:18526:0:99999:7:::
list:*:18526:0:99999:7:::
irc:*:18526:0:99999:7:::
gnats:*:18526:0:99999:7:::
nobody:*:18526:0:99999:7:::
_apt:*:18526:0:99999:7:::
sshd:*:18739:0:99999:7:::
john:$6$hJz8BHbB$WkIBAYHTABpbnwBfFWk6nwJHhEzoGjq4Big3ciZw1NtIMEp3lUMdm8NOUKTrG8fgzQI/WzNRtd3FB88ODoIJw/:18759:0:99999:7:::
```
