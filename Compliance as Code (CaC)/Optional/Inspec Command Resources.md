# Inspec Command Resources
Need to install the Inspec tool, create the profile and then run the Compliance scan against the server.
## Install Inspec
Download inspec package 
```sh
wget https://packages.chef.io/files/stable/inspec/4.18.114/ubuntu/16.04/inspec_4.18.114-1_amd64.deb
```
Install the downloaded package.
```sh
dpkg -i inspec_4.18.114-1_amd64.deb
```
**Output**
```sh
Selecting previously unselected package inspec.
(Reading database ... 11606 files and directories currently installed.)
Preparing to unpack inspec_4.18.114-1_amd64.deb ...
You're about to install InSpec!
Unpacking inspec (4.18.114-1) ...
Setting up inspec (4.18.114-1) ...
Thank you for installing InSpec!
```
## Create the profile
Create a new folder and cd into that folder.
```sg
mkdir cis-ubuntu
cd cis-ubuntu
```
## Test the barebone profile
```
inspec init profile ubuntu --chef-license accept
```
**Output**
```sh
+---------------------------------------------+
✔ 1 product license accepted.
+---------------------------------------------+

 ─────────────────────────── InSpec Code Generator ─────────────────────────── 

Creating new profile at /cis-ubuntu/ubuntu
 • Creating directory controls
 • Creating file controls/example.rb
 • Creating file README.md
 • Creating file inspec.yml
 ```
 ## Understand the audit command
Will create a custom profile based on CIS Ubuntu 18.04 Benchmark, you can download the benchmark PDF from here and refer the page 83 “Ensure sudo commands use pty”.
Add this control/security best practice into Inspec profile. But, before we create the profile we need to ensure that the command works in DevSecOps Box and understand its output.

### Execute the audit command.

Following is the command, we will be using to check if pty is being used.
```sh
grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*
```
Didn’t see any output so we can assume that we didn’t find use_pty setting in our sudoers file. Lets add this security best practice to the file and see how the above command behaves.

Edit /etc/sudoers file using nano or any text editor and add the use_pty setting.
```sh
cat >> /etc/sudoers <<EOL
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
Defaults        use_pty    # you can put it here
# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
EOL
```
Run the audit command once again and see if it finds pty usage. In short, absense of this string is a control failure and we need to enforce its presence.
```sh
grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*
```
**Output**
```sh
/etc/sudoers:Defaults        use_pty    # you can put it here
```
## Add the command to the profile
To do this, inspec provides us with a command resource/method.

Let’s have a look at Inspec documentation for the command resource. https://docs.chef.io/inspec/resources/command/

As mentioned above, we can use this resource to execute commands like the grep we ran in the last step. Once a command returns its output, we can use stdout(aka output) to compare it against the desired/expected value in our profile.
Use the following command to replace the inspec task in the file, ubuntu/controls/example.rb. If you wish, you can edit the file manually, using nano or any text editor.
```sh
cat > ubuntu/controls/example.rb <<EOL
control 'ubuntu-1.3.2' do
   title 'Ensure sudo commands use pty'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe command('grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*') do
      its('stdout') { should match /Defaults(\s*)use_pty/ }
   end
end
EOL
```
Ignore the should match text for now
Why use the regex? This is to ensure, we capture the exact match irrespective of space or tab as a seperator used between Defaults and use_pty strings.
**Output**
```sh
/etc/sudoers:Defaults        use_pty    # you can put it here
The sudoer file can also just a space and it will be valid.

/etc/sudoers:Defaults use_pty    # you can put it here
```
Validate the profile to ensure there are no syntax errors.
```sh
inspec check ubuntu
```
**Output**
```sh
Location :   ubuntu
Profile :    ubuntu
Controls :   1
Timestamp :  2020-11-25T07:35:04+00:00
Valid :      true

No errors or warnings
```
Run the profile on the local-machine before executing it on the remote server
```sh
inspec exec ubuntu
```
**Output**
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  local://

  ✔  ubuntu-1.3.2: Ensure sudo commands use pty
     ✔  Command: `grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*` stdout is expected to match /Defau
lts(\s*)use_pty/


Profile Summary: 1 successful control, 0 control failures, 0 controls skipped
Test Summary: 1 successful, 0 failures, 0 skipped
```
1 successful control check because we just add the remediation into DevSecOps Box.

## Run the Inspec Profile to test for compliance against a server
Let’s try to run the custom profile created by us against the server.

Before executing the profile, we need to execute the below command.

echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config

The above command prevents the ssh agent from prompting YES or NO question.

Let’s run inspec with the following options.
```sh
inspec exec ubuntu -t ssh://root@prod-ccpwla97 -i ~/.ssh/id_rsa --chef-license accept
```
**Output**
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  ssh://root@prod-ccpwla97:22

  ×  ubuntu-1.3.2: Ensure sudo commands use pty
     ×  Command: `grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*` stdout is expected to match /Defau
lts(\s*)use_pty/
     expected "" to match /Defaults(\s*)use_pty/
     Diff:
     @@ -1,2 +1,2 @@
     -/Defaults(\s*)use_pty/
     +""

Profile Summary: 0 successful controls, 1 control failure, 0 controls skipped
Test Summary: 0 successful, 1 failure, 0 skipped
```
The flags/options used in the above commands are:

- -t : tells the target machine to run the profile against.
- -i : provides the path where the remote machine’s ssh key is stored.
- --chef-license : accept ensures that we are accepting license agreement there by preventing the inspec from prompting YES or NO question.
You can see, we got 1 control failure i.e., the production machine doesn’t follow the CIS best practice. The DevOps/Security team can run Ansible hardening scripts to ensure the production machine is hardened and continously scan the production machine for compliance.
