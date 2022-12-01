# SAST commands
|Command|Description|Example|
|:----|:----|:----|
|Git clone|Download source code from git repository|```git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp```|
|pip3 install|Install security tool on the system|```pip3 install trufflehog==2.2.1```|
|trufflehog|Command to run the trufflehog secrets security scanner|```trufflehog --json . \| tee secret.json```|
|bandit|Command to run the bandit security issue scanner|```bandit -r . -f json \| tee bandit-output.json```|
|apt update|Updates advanced package tool|```apt update```|
|apt install|Installs an advanced package|```apt install ruby-full -y```
|gem install|Used for installing brakeman. It is a ruby command|```gem install brakeman -v 5.2.1```|
|brakeman|Command to run the brakeman scan on Rails code|```brakeman -f json \| tee result.json```|
|-i|used to ignore warnings in a scan|```brakeman -f json -i brakeman.ignore \| tee result.json```|
