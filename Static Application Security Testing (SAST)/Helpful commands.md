# Helfpul SAST commands
|Command|Description|Example|
|:----|:----|:----|
|Git clone|Download source code from git repository|```git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp```|
|pip3 install|Install security tool on the system|```pip3 install trufflehog==2.2.1```|
|trufflehog|Command to run the trufflehog secrets security scanner|```trufflehog --json . \| tee secret.json```|
|bandit|Command to run the bandit security issue scanner|```bandit -r . -f json \| tee bandit-output.json```|
||||
