# Searching in File Using Nano Editor

We can search inside the file using the ctrl + w
Create the file security-report.json using the below content
```sh
{
    "Name":"XSS",
    "Vulnerabilities": "10",
    "Valid": "2",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z",
    "Name":"SQL injection",
    "Vulnerabilities": "9",
    "Valid": "1",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z",
    "Name":"IDOR",
    "Vulnerabilities": "6",
    "Valid": "4",
    "Severity": "High",
    "generated_at": "2022-05-16T17:37:50Z"
}
```
Let us search for the text, **IDOR**
Press **ctrl + w** and type **IDOR**
and then press enter
You can the see the cursor will be moved to the word **IDOR**.

# Search in Multiple occurences
When you search for a word that has multiple occurences in the file, you can use **alt + w** for the next occurence and **alt + q** for previous occurence.