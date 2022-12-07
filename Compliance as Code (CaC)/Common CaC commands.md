# CaC Commands

|Command|Description|Example|
|:----|:---|:----|
|wget|Used to download from the web|```wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb```|
|dpkg|Used to install downloaded packages|```dpkg -i inspec_4.37.8-1_amd64.deb```|
|inspec|Command used to run inspec|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|-t|Tells the target machine|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|-i flag used to specify the ssh-key since we are using login via ssh|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|
|--chef-license accept|Tells that we are accepting this commands prevent the insec from prompting yes or no question|```inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@prod-acsrq8h9 -i ~/.ssh/id_rsa --chef-license accept```|

