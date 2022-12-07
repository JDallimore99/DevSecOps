#IaC Commands
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
|grep|Use to find modules within ansible commands|```ansible-doc -l \| grep shell```|
|"uptime"|Used to find the uptime on production machines using shell module|```ansible -i inventory.ini prod -m shell -a "uptime"```|
|playbook|Create a playbook file that can be used to run against the production environment|```cat > playbook.yml <<EOL --- - name: Example playbook to install firewalld   hosts: prod   remote_user: root  become: yes  gather_facts: no   vars:     state: present    tasks:  - name: ensure firewalld is at the latest version     apt:       name: firewalld  EOL```|
|ansible-playbook|Command to run the playbook against a prod machine|```ansible-playbook -i inventory.ini playbook.yml```|
|ansible-galaxy|Galaxy provides pre-packaged units of work known to Ansible as roles and collections.|```ansible-galaxy install secfigo.terraform```|
|install|Used to install roles within ansible|```ansible-galaxy install secfigo.terraform```|
|search|Can search for desired roles within ansible using search option|```ansible-galaxy search terraform```|
|echo|Used to copy contents of private key variable stored in Gitlab CI into the id_rsa file under the ~/.ssh inside the container|```- echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" \| tr -d '\r' > ~/.ssh/id_rsa```|
|eval|eval runs the command ssh-agent in the background and sends the key whenever SSH asks for a key in an automated fashion.|```- eval "$(ssh-agent -s)"```|


To find the available modules, you can check out this link (https://docs.ansible.com/ansible/2.8/modules/list_of_all_modules.html) or use the local help command-line tool.
