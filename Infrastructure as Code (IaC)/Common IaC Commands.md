|Command|Description|Example|
|:---|:---|:----|
|pip3 install|install programs such as Ansible|```pip3 install ansible==6.4.0```|
|inventory file|Using cat to create an inventory file to define a list of hosts that can be sorted|```cat > inventory.ini <<EOL [devsecops] devsecops-box-acsrq8h9 [sandbox] sandbox-acsrq8h9 [prod] prod-acsrq8h9 EOL ```|
|ansible|Use the ansible command|```ansible -i inventory.ini prod --list-hosts```|
|--version|See if there is any default configuration for Ansible|```ansible --version```|
|mkdir|Create a configuration file by making a directory|```mkdir /etc/ansible/```|
|ansible.cfg|Create a file that is used as a configuration|```cat > /etc/ansible/ansible.cfg <<EOF [defaults] stdout_callback = yaml deprecation_warnings = False host_key_checking = False retry_files_enabled = False inventory = /inventory.ini EOF```|
