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
|nikto.config|Congigure nikto scan|```cat >/nikto/program/nikto.conf<<EOF SKIPPORTS=21 22 111 SKIPIDS=00035 00045 CLIOPTS=-output result.csv -Format csv EOF```|
|SKIPPORTS|Used to remove ports that would never be scanned|```SKIPPORTS=21 22 111```|
|SKIPIDS|Remove false positives|```SKIPIDS=00035 00045```|
|-output|Save output|```CLIOPTS=-output result.csv -Format csv```|
|apt-get update|Gets the update for the advanced package tool|```apt-get update```|
|apt-get install|Installs the advanced package tool|```apt-get install nmap -y```|
|nmap|Commmand to run the nmap port scanner|```nmap prod-acsrq8h9 -oX nmap_out.xml```|
|-oX|Flag used to tell the too that the output should be saved in a certain format|```nmap prod-acsrq8h9 -oX nmap_out.xml```|
|pip3 install|Used to install a tool|```pip3 install sslyze==5.0.3```|
|sslyze|Command used to run the ssyle ssl scanning tool|```sslyze --json_out sslyze-output.json prod-acsrq8h9.lab.practical-devsecops.training:443```|
|--json_out|Used to store the output in json format|```sslyze --json_out sslyze-output.json prod-acsrq8h9.lab.practical-devsecops.training:443```Z
|docker pull|Used to pull ZAP's stable docker image|```docker pull owasp/zap2docker-stable:2.10.0```|
|docker run|Used to run the ZAP scanning tool, zap-baseline.py|```docker run --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training```|
|-j|Output in JSON format|```docker run --user $(id -u):$(id -g) -w /zap -v $(pwd):/zap/wrk:rw --rm owasp/zap2docker-stable:2.10.0 zap-baseline.py -t https://prod-acsrq8h9.lab.practical-devsecops.training -J zap-output.json```|
