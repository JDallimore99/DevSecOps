|Command|Description|Example|
|:----|:----|:----|
|apt install|Install advanced package tool|```apt install -y libnet-ssleay-perl```|
|git clone||```git clone https://github.com/sullo/nikto```|
|./nikto.pl|Command to run the Nikto scanner|```./nikto.pl -output nikto_output.xml -h prod-acsrq8h9```|
|-h|Flag used to set the target application which we want to scan|```./nikto.pl -output nikto_output.xml -h prod-acsrq8h9```
|-output|Flag used to set the output file in which we want to store the result|```./nikto.pl -output nikto_output.xml -h prod-acsrq8h9```|
|cat|Show the output file|```cat nikto_output.xml```|
|-list-plugins|shows a list of the available plugins|```./nikto.pl -list-plugins```|
|-Plugins \<plugin-name\>|Run a nikto plugin scan against a production machine|```./nikto.pl -h prod-acsrq8h9 -Plugins "@@default;dictionary(dictionary:/nikto/program/databases/db_dictionary)"```|

