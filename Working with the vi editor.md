# Working with the vi editor
## Intro to vi editor
Visual Editor (vi), one of the powerful terminal-based text editors. Using vi editor, we can read, create and write a file. You can also use other popular text editors like vim, Nano, Emacs, Ne for terminal based file operations.

Command vi is used to access the visual Editor (vi). You can specify the file name along with vi to open an existing file or create a new file.
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
Open the **vi editor**
```sh
vi security-report.json
**Output**
{
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}
```

## Modes of Operation
### Command Mode
Command mode is the default mode when you open the vi editor, we can enter commands in this mode to move through a file, and to delete, copy, or paste a piece of text.
Okay I’m done reading the file, How do i close the editor?
Now type the command, :q This command exits the vi.
### Insert Mode
Insert mode allows you to edit or enter the content in a file. To enter into insert mode, just press i.
Now let us create a new file **foo.txt** using vi.
```sh
vi foo.txt
```
You can see that vi loads the command mode by default.
Now press i to enter the Insert mode and enter the below text.
Once you finish entering the text, we need to go to command mode to save the file. Does it sound complex? We can go to command mode just by pressing esc button.
Let us save the file and exit the vi by entering :wq
If you dont want to save the changes, enter :q!, to close the vi without saving the changes.
### Line Mode
Line Mode is invoked by typing a colon [:], while vi is in Command Mode. The cursor will jump to the last line of the screen and vi will wait for a command.
There is a way to enter the text into the file without entering the Insert mode, how do we do it?
You can do this by copying the text and pasting it directly without pressing i. You can paste the text using **ctrl + v** or **command + v**.
