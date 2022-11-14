# Basics of Command Outputs
## Output Redirection
Linux allows you to save the output of a command to a file or send it to another command for further processing. It does it by using redirection.
```sh
cat /etc/passwd > mypasswd.txt
```
We are reading a file located at **/etc/passwd** using the **cat** command, and saving the output (the entire content of /etc/passwd) into a new file (as instructed by the > symbol) called **mypasswd.txt.**
Can verify the content of the newly created **mypasswd.txt** file by using the following command.
```sh
cat mypasswd.txt

**Command Output**
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin
```
Can also append to an existing file using >> characters. If you want to append the content of the /etc/passwd file to the mypasswd.txt file, you can use the following command.
```sh
cat /etc/passwd >> mypasswd.txt
```
Verify
```sh
cat mypasswd.txt
```
Similarly, there is a handy **echo** command to look at the contents of variables, or create smaller files using the echo command.
```sh
echo "this is a string"
```
We can see the contents of the pre-defined variable called HOSTNAME using the echo command.
```sh
echo $HOSTNAME
```
You can redirect a string or the content of a variable to a file as well.
```sh
echo "this is a string" > file.txt
cat file.txt
**Command Output**
this is a string
```
# Piping the Output
In piping, you take a command’s output and send it to another command for further processing.
Lets take an example of **mypasswd.txt** file that we created in the previous step, and parse the contents of the file using **cat** and **cut** command to extract the usernames.
If you wish to extract only usernames from the output, you can use the cut command with delimiter as colon (:).
```sh
cat mypasswd.txt
cat mypasswd.txt | cut -d ':' -f 1
```
Behind the scenes, the cat command’s output was sent to the cut command as an input, and it used **-d (delimiter)** flag with a colon as a separator/delimiter and **-f (field)** option to get the 1st field.

Another common usage of pipe is to to search for a given string in a command output.

For example, as indicated below, we can get a list of running processes using the **ps** command, and send it to the **grep** command to identify a particular process (bash process in this case).
```sh
ps -aux | grep bash

**Command Output**
root         1  0.0  0.0  21340  4660 pts/0    Ss   14:29   0:00 bin/bash
root       624  0.0  0.0  13216  1156 pts/0    S+   14:30   0:00 grep --color=auto bash
```