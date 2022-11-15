# Working with Nano Editor
## Intro to Nano
GNU nano is an easy to use command line text editor for Unix and Linux operating systems. It includes all the basic functionality you’d expect from a regular text editor, like syntax highlighting, multiple buffers, search and replace with regular expression support, spellchecking, UTF-8 encoding, and more
Let us open a file using nano.
Command **nano** is used to access the Nano text editor. You can specify the file name along with **nano** to open an existing file or create a new file.
Let us create the security-report.json file using the cat command.
```sh
cat > security-report.json <<EOL

{
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}

EOL
```
After creating the security-report.json file, let us open the file using nano
```sh
nano security-report.json
**Output**
{
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}
```
Now you can see the content of the file.
To exit the editor, you can do ctrl + x.
## Editing the files
Unlike vi, nano is a modeless editor, which means that you can start typing and editing the text immediately after opening the file.
You can use the arrow keys to move inside the text editor. Go to line 2 and enter the below text.
```sh
"Name": "SQL Injection",
```
The final output of the file should look like this,
```sh
{
    "Name": "SQL Injection",
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}
```
To save the file, press **ctrl + o** and press enter