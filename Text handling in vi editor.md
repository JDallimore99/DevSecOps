# Text Handling in vi editor
## Visual Select
Visual select can be extremely useful for identifying chunks of text and perform various operations. In this section, we will cover
1. Character mode
2. Line mode

### Character Mode
Character mode is used to highlight a sentence in a paragraph or a phrase in a sentence. Then the visually identified text can be deleted, copied, changed, or modified with any other Vim editing command.
Let us try to select the text using the visual mode.
Create a file output.json using the below content and open it:
```sh
{
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}
```
Press v to enter the visual mode and now move the cursor between the text to select them. You can also notice that the text between the initial position and current position of the cursor is selected.

### Line Mode
Line mode is used to highlight lines of text. The main difference between the Character mode and Line mode is, in Character mode, we can select phrase in the line but in Line mode we can select only lines.
Let us try to select multiple lines using the line mode.
Open the output.json file and enter the line mode by pressing the Shift + v.
Once you enter the line mode, move the cursor using the down or up arrow. You can see that multiple lines are being selected.
## Delete the text
You can delete the selected text in the vi using the d command.
Open the output.json file and enter the line mode by pressing the Shift + v.
```sh
{
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}
```
Now place the cursor before the “generated_at” and press d. You can see that we have successfully deleted the line.
```sh
{
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
}
```
>You can undo the delete action by pressing the u key.
### Delete without selecting text
Sometimes it is tedious to select the text and delete it. To make this easier, we can delete a line of text by pressing the d twice.
Open the output.json file and enter the line mode by pressing the Shift + v.
Now place the cursor before the “generated_at” and press dd. You can see that the line of text is deleted.