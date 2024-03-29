# Inspec File Resource
Learn how to install the Inspec and learn to create a profile with file resource.
Need to install the Inspec tool, create the profile and then run the Compliance scan against the server.

## Install Inspec
```sh
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
```
Install Downloaded package
```sh
dpkg -i inspec_4.37.8-1_amd64.deb
```
#### Create the Profile
Create a new folder and cd into that folder.
```sh
mkdir cis-ubuntu
cd cis-ubuntu
```
#### Test the barebone profile
```sh
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
#### Run the audit command
Let’s take the same example covered in command resource exercise i.e, “Ensure sudo commands use pty” and try to automate it using file resource instead of command resource.
Used the following command in command resource exercise for the auditing the machine.
```sh
grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*
```
Obviously, got nothing as haven’t hardened this machine.
Create a proper sudoers file to see before and after states of the file.
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
Re-run the above command.
```sh
grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers /etc/sudoers.d/*
```
**Output**
```sh
/etc/sudoers:Defaults        use_pty    # you can put it here
```
Got the expected value
## Understand the File Resource
Use the file InSpec audit resource to test all system file types, including files, directories, symbolic links, named pipes, sockets, character devices, block devices, and doors.

Let’s have a look at Inspec documentation of the file resource and see what it has to offer us.

Besides using command resource, we can also use file resource to test the desired/expected state of the system.

As they say, “more than one way to do it”.

Run the following command to replace the inspec task at ubuntu/controls/example.rb. If you wish, you can edit the file using nano or any text editor.
```sh
cat > ubuntu/controls/example.rb <<EOL
control 'ubuntu-1.3.2' do
   title 'Ensure sudo commands use pty'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe file('/etc/sudoers') do
      its('content') { should match /Defaults(\s*)use_pty/ }
   end
end
EOL
```
Lets validate the profile to ensure there are no syntax errors.
```sh
inspec check ubuntu
```
**Output**
Location :   ubuntu
Profile :    ubuntu
Controls :   1
Timestamp :  2020-11-25T07:35:04+00:00
Valid :      true
```
No errors or warnings
Now run the profile on the local machine i.e., devsecops-box.
```sh
inspec exec ubuntu
```
**Output**
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  local://

  ✔  ubuntu-1.3.2: Ensure sudo commands use pty
     ✔  File /etc/sudoers content is expected to match /Defaults(\s*)use_pty/


Profile Summary: 1 successful control, 0 control failures, 0 controls skipped
Test Summary: 1 successful, 0 failures, 0 skipped
```
You can see, we have about 1 successful control check because we created that file with the right settings.
#### Run the Inspec tool to test for compliance against a server
Let’s try to run the custom profile created by us against the server
```sh
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
```
```sh
inspec exec ubuntu -t ssh://root@prod-ccpwla97 -i ~/.ssh/id_rsa --chef-license accept
```
**Output**
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  ssh://root@prod-ccpwla97:22

  ×  ubuntu-1.3.2: Ensure sudo commands use pty
     ×  File /etc/sudoers content is expected to match /Defaults(\s*)use_pty/
     expected "#\n# This file MUST be edited with the 'visudo' command as root.\n#\n# Please consider adding local ...\n# See sudoers(5) for more information on \"#include\" directives:\n\n#includedir /etc/sudoers.d\n" to match /Defaults(\s*)use_pty/
     Diff:
     @@ -1,2 +1,31 @@
     -/Defaults(\s*)use_pty/
     +#
     +# This file MUST be edited with the 'visudo' command as root.
     +#
     +# Please consider adding local content in /etc/sudoers.d/ instead of
     +# directly modifying this file.
     +#
     +# See the man page for details on how to write a sudoers file.
     +#
     +Defaults  env_reset
     +Defaults  mail_badpass
     +Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
     +
     +# Host alias specification
     +
     +# User alias specification
     +
     +# Cmnd alias specification
     +
     +# User privilege specification
     +root      ALL=(ALL:ALL) ALL
     +
     +# Members of the admin group may gain root privileges
     +%admin ALL=(ALL) ALL
     +
     +# Allow members of group sudo to execute any command
     +%sudo     ALL=(ALL:ALL) ALL
     +
     +# See sudoers(5) for more information on "#include" directives:
     +
     +#includedir /etc/sudoers.d

Profile Summary: 0 successful controls, 1 control failure, 0 controls skipped
Test Summary: 0 successful, 1 failure, 0 skipped
```
The flags/options used in the above commands are:
- -t : tells the target machine to run the profile against.
- -i : provides the path where the remote machine’s ssh key is stored.
- --chef-license : accept ensures that we are accepting license agreement there by preventing the inspec from prompting YES or NO question.
As you can see on line number 43, we have about 0 successful control check and 1 control failures. It failed as production machine is not hardened yet.


