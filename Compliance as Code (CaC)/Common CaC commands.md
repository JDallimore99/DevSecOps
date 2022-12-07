# CaC Commands

|Command|Description|Example|
|:----|:---|:----|
|wget|Used to download from the web|```wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb```|
|dpkg|Used to install downloaded packages|```dpkg -i inspec_4.37.8-1_amd64.deb```|
|inspec|Command used to run inspec|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|-t|Tells the target machine|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|-i|flag used to specify the ssh-key since we are using login via ssh|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|--chef-license accept|Tells that we are accepting this commands prevent the insec from prompting yes or no question|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|only|Used to specify that a job should be started when a condition is met|```only: - master```|
|mkdir|Create a new folder/directory|```mkdir inspec-profile && cd inspec-profile```
|inspec init|Create a Ubuntu profile|```inspec init profile ubuntu --chef-license accept```|
|check|Validate the profile created|```inspec check ubuntu```|
|exec|Run the profile on local-machine before executing on the server|```inspec exec ubuntu```|
|describe file|Used to add checks from PCI/DSS requirements|```describe file('/etc/pam.d/system-auth') do    its('content') { should match(/^\s*password\s+requisite\s+pam_pwquality\.so\s+(\S+\s+)*try_first_pass/) }  its('content') { should match(/^\s*password\s+requisite\s+pam_pwquality\.so\s+(\S+\s+)*retry=[3210]/)} end```|
