# Basics of File and Directories
## Understanding Directories
Directories are an underlying file system object. The term Folder can also be interchangeably used to mean a directory, although there might be trivial semantic differences between both the terms.
As we reviewed in some of the previous exercises, if you wish to create a directory, you can use the **mkdir** command.
```sh
mkdir new-directory
```
You can explore the content of this folder using the **ls** command
```sh
ls new-directory
```
Since new-directory is empty, the above ls command would return nothing.
You can explore the present working directory, that is the current directory that you are in, using the **pwd**
command.
```sh
pwd
**Command Output**
/
```
/ indicates that you are in the root directory of the file system.

You can change the present working directory using the **cd** command.
```sh
cd new-directory
**Command Output**
```
In case the directory name is not visible on the command line, you can check it using the **pwd** command.
You can go back to the previous directory (or more appropriately the parent directory) by using the **cd** command with ... 
```sh
cd ..
```
You can remove a directory with the **rmdir** command.
```sh
rmdir new-directory
```
> Note: Successful command execution does not indicate whether the command actually succeeded. Unsuccessful command execution however, shows an error message of why the command failed. In the cases of commands such as ls, because ls is a read operation, it will return an output when ls finds files and directories in a given directory.

# Understanding Files
Everything is a file in Linux, text files, configurations, services, etc., However, Linux is so powerful there are many ways of doing the exact same operation.

If you wish to create a file, you can use any text editor of your choice. We will use **nano** and **cat** in this exercise.
```sh
nano myfile
```
Once you are in the editor, add some text and save the file using **control + O** (letter o) and hit Enter or the return.

You can exit the editor using **control + X**

You can also use the cat command (remember multiple ways of doing the exact same thing?).
**cat** command reads the content of a given file and displays as output.

If you wish to rename a file, you can use the **mv** (move) command.
```sh
mv myfile newfile
ls
```
Can see that **myfile** is renamed to **newfile** using the ls command

# Here Documents
The **cat** command is mostly used to read a file; however, using a command line magic (read as command line hack), we can even create a file in a non-interactive way (ideal for CI/CD systems).
```sh
cat > filename <<EOL

Some text content
Some text content 2
Some text content 3

EOL
```
There are three parts to this command.
1. The filename, here its filename
2. The **here document**, the << symbols are a special code block. It allows you to redirect anything between EOL into a command
3. The **cat** command, if it’s followed by >, allows you to create a file
