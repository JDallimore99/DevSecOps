|Command|Description|Example|
|:---|:---|:----|
|pip3 install|install programs such as Ansible|```pip3 install ansible==6.4.0```|
|inventory file|Using cat to create an inventory file to define a list of hosts that can be sorted|```cat > inventory.ini <<EOL [devsecops] devsecops-box-acsrq8h9 [sandbox] sandbox-acsrq8h9 [prod] prod-acsrq8h9 EOL ```|
|ansible|Use the ansible command|```ansible -i inventory.ini prod --list-hosts```|
|--version|See if there is any default configuration for Ansible|```ansible --version```|
|mkdir|Create a configuration file by making a directory|```mkdir /etc/ansible/```|
|ansible.cfg|Create a file that is used as a configuration file, allwing for definition of inventory settings|```cat > /etc/ansible/ansible.cfg <<EOF [defaults] stdout_callback = yaml deprecation_warnings = False host_key_checking = False retry_files_enabled = False inventory = /inventory.ini EOF```|
|ssh-keyscan|ensure the SSH’s yes/no prompt is not shown while running the ansible commands, so let’s use the ssh-keyscan to capture the key signatures beforehand|```ssh-keyscan -t rsa devsecops-box-acsrq8h9 sandbox-acsrq8h9 prod-acsrq8h9 >> ~/.ssh/known_hosts```|
|ping|test the remote nodes or hosts and make sure they respond back using Ansible’s default SSH channel|```ansible -i inventory.ini all -m ping```|
|shell|Run hostname commmand on all machines|```ansible -i inventory.ini all -m shell -a "hostname"```|
|apt|used to install a package inside a remote host|```ansible -i inventory.ini all -m apt -a "name=ntp"``` or ```ansible -i inventory.ini prod -m apt -a "name=ntp state=present"```|
|copy|An ad-hoc command used to copy a file into all remote machines|```ansible -i inventory.ini all -m copy -a "src=/root/notes dest=/root"```|

To find the available modules, you can check out this link (https://docs.ansible.com/ansible/2.8/modules/list_of_all_modules.html) or use the local help command-line tool.
