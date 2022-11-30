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
|cat >|Create a file in a non-interactive way using command line magic: 1. The filename, here its filename
2. The here document, the << symbols are a special code block. It allows you to redirect anything between EOL into a command
3. The cat command, if it’s followed by >, allows you to create a file|```cat > filename <<EOL

Some text content
Some text content 2
Some text content 3

EOL```|

