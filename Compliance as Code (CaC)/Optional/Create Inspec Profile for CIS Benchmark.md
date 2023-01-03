# Create Inspec Profile for CIS Benchmark
Learn how to create a custom Inspec profile to check for your organization’s policies as code. 
Learn how to test this profile against a server.
## Install the Inspec on the system.
Download the Inspec Debian package from the InSpec website
```sh
wget https://packages.chef.io/files/stable/inspec/4.18.114/ubuntu/16.04/inspec_4.18.114-1_amd64.deb
```
Install the downloaded package
```sh
dpkg -i inspec_4.18.114-1_amd64.deb
```
```sh
inspec --help
```
**Output**
```sh
Commands:
  inspec archive PATH                # archive a profile to tar.gz (default) or zip
  inspec artifact SUBCOMMAND         # Manage Chef InSpec Artifacts
  inspec check PATH                  # verify all tests at the specified PATH
  inspec compliance SUBCOMMAND       # Chef Compliance commands
  inspec detect                      # detect the target OS
  inspec env                         # Output shell-appropriate completion configuration
  inspec exec LOCATIONS              # Run all test files at the specified LOCATIONS. Loads the given profile(s) and fetches their dependen...
  inspec habitat SUBCOMMAND          # Manage Habitat with Chef InSpec
  inspec help [COMMAND]              # Describe available commands or one specific command
  inspec init SUBCOMMAND             # Generate InSpec code
  inspec json PATH                   # read all tests in PATH and generate a JSON summary
  inspec nothing                     # does nothing
  inspec plugin SUBCOMMAND           # Manage Chef InSpec and Train plugins
  inspec shell                       # open an interactive debugging shell
  inspec supermarket SUBCOMMAND ...  # Supermarket commands
  inspec vendor PATH                 # Download all dependencies and generate a lockfile in a `vendor` directory
  inspec version                     # prints the version of this tool

Options:
  l, [--log-level=LOG_LEVEL]                         # Set the log level: info (default), debug, warn, error
      [--log-location=LOG_LOCATION]                  # Location to send diagnostic log messages to. (default: $stdout or Inspec::Log.error)
      [--diagnose], [--no-diagnose]                  # Show diagnostics (versions, configurations)
      [--color], [--no-color]                        # Use colors in output.
      [--interactive], [--no-interactive]            # Allow or disable user interaction
      [--disable-core-plugins]                       # Disable loading all plugins that are shipped in the lib/plugins directory of InSpec. Useful in development.
      [--disable-user-plugins]                       # Disable loading all plugins that the user installed.
      [--enable-telemetry], [--no-enable-telemetry]  # Allow or disable telemetry
      [--chef-license=CHEF_LICENSE]
```
## Create the profile with CIS Benchmark
Create a custom profile for Inspec using the commands and best practices mentioned in CIS Ubuntu 18.04 Benchmark; you can download the CIS Ubuntu 18.04 Benchmark PDF from here.
https://downloads.cisecurity.org/#/
Please refer the page no. 80, “Configure Sudo” for more information. Let’s convert the audit steps mentioned in the CIS Benchmark into a custom Inspec profile.

In short, we need to do two things to create a profile.
1. Figure out the command/tool that needs to be run to ascertain the state of the system.
2. Add the above command to the Inspec profile.

Create the Inspec profile.

To store our Inspec profile, we will create a new folder and cd into the newly created folder.
```sh
mkdir cis-ubuntu && cd cis-ubuntu
```
Use the inspec init command to create an Ubuntu Inspec profile.
```sh
inspec init profile ubuntu --chef-license accept
```
Run the following command to append the inspec task to the ubuntu/controls/configure_sudo.rb file. If you wish, you can edit the file using nano or any text editor.
```sh
cat >> ubuntu/controls/configure_sudo.rb <<EOL
control 'ubuntu-1.3.1' do
   title 'Ensure sudo is installed'
   desc 'sudo allows a permitted user to execute a command as the superuser or another user, as specified by the security policy.'
   describe package('sudo') do
      it { should be_installed }
   end
end

control 'ubuntu-1.3.2' do
   title 'Ensure sudo commands use pty'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe command('grep -Ei "^\s*Defaults\s+([^#]+,\s*)?use_pty(,\s+\S+\s*)*(\s+#.*)?$" /etc/sudoers').stdout do
      it { should include 'Defaults use_pty' }
   end
end

control 'ubuntu-1.3.3' do
   title 'Ensure sudo log file exists'
   desc 'Attackers can run a malicious program using sudo, which would again fork a background process that remains even when the main program has finished executing.'
   describe command('grep -Ei "^\s*Defaults\s+logfile=\S+" /etc/sudoers').stdout do
      it { should include 'Defaults logfile=' }
   end
end
EOL
```
Remove the ubuntu/controls/example.rb file, as we will not use it in this profile.
If you notice in the above Inspec profile script, you will see a couple of new terms like package and command. These terms are called resources; think of them as modules or plugins that help you achieve something in the system.
- command resource https://docs.chef.io/inspec/resources/command/
- package resource https://docs.chef.io/inspec/resources/package/

The above code checks if the sudo package is installed. We are also checking if the file /etc/sudoers contain some sane security defaults. The CIS benchmark PDF provides us the needed commands to audit the system’s state, and we are using these commands to convert them into Inspec checks.

We tried to keep everything simple in this exercise for the sake of simplicity, but you do need to implement most of these checks in your organization. If we have a few hundred ubuntu servers to patch, maintain or ensure compliance, do you think we can/should do these manually? No, we should create an Inspec profile to automate our compliance checks. This process is what most people call Compliance as Code.

Let’s validate the profile to ensure there are no syntax errors.
```sh
inspec check ubuntu
```
**Output**
```sh
Location :   ubuntu
Profile :    ubuntu
Controls :   3
Timestamp :  2020-11-24T05:19:27+00:00
Valid :      true

No errors or warnings
```
Now run the profile on the local machine before executing it against a server. If we do not provide any server details, it runs on the local machine by default.
```sh
inspec exec ubuntu
```
**Output**
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  local://

  ✔  ubuntu-1.3.1: Ensure sudo is installed
     ✔  System Package sudo is expected to be installed
  ×  ubuntu-1.3.2: Ensure sudo commands use pty
     ×  is expected to include "Defaults use_pty"
     expected "" to include "Defaults use_pty"
  ×  ubuntu-1.3.3: Ensure sudo log file exists
     ×  is expected to include "Defaults logfile="
     expected "" to include "Defaults logfile="


Profile Summary: 1 successful control, 2 control failures, 0 controls skipped
Test Summary: 1 successful, 2 failures, 0 skipped
```
## Run the Inspec against a remote server
Let’s try to run the custom profile we just created against a remote server.
Before executing the profile, we need to implement the password-less SSH login mechanism between the local machine and the remote machine.
To configure passwordless SSH login, we need to implement the following two steps.
1. Get rid of the SSH key verification checks.
2. Copy your local SSH public key into the remote machine.
We have already taken care of step 2 for you, so you only have to deal with step 1. Obviously, we can achieve step 1 objectives in multiple ways. Here, we are disabling host key checks, but it’s a bad idea to do it in production. Please research why it’s a bad idea!
> Disabling host key checks can be a security risk because it allows man-in-the-middle (MITM) attacks to go undetected. When host keys are checked, the client machine verifies the authenticity of the host it is connecting to by checking the host key against a trusted list of known host keys. If the host key of the server the client is connecting to is not in the trusted list, the connection is terminated, and the client is alerted of the potential MITM attack.
If host key checks are disabled, the client will not be able to detect if it is being subjected to an MITM attack and will unknowingly send sensitive data, such as passwords and other confidential information, to the attacker. This can result in data breaches, unauthorized access to systems, and other security issues.
Therefore, it is generally recommended to leave host key checks enabled in production environments to help protect against MITM attacks. 

```sh
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
```
As mentioned before, the above command prevents the ssh agent from prompting YES or NO question.
Run the newly created ubuntu Inspec profile against the production server (prod-ccpwla97)
```sh 
inspec exec ubuntu -t ssh://root@prod-ccpwla97 -i ~/.ssh/id_rsa --chef-license accept
```
**Output**
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  ssh://root@prod-jftfefdf:22

  ✔  ubuntu-1.3.1: Ensure sudo is installed
     ✔  System Package sudo is expected to be installed
  ×  ubuntu-1.3.2: Ensure sudo commands use pty
     ×  is expected to include "Defaults use_pty"
     expected "" to include "Defaults use_pty"
  ×  ubuntu-1.3.3: Ensure sudo log file exists
     ×  is expected to include "Defaults logfile="
     expected "" to include "Defaults logfile="


Profile Summary: 1 successful control, 2 control failures, 0 controls skipped
Test Summary: 1 successful, 2 failures, 0 skipped
```
The flags/options used in the above commands are
- -t specifies the target machine to run the Inspec profile against. Here, we are using SSH as a remote login mechanism, but we can also use winrm (windows), container (docker), etc.,
- -i provides the path where the remote/local machine’s ssh key is stored.
- --chef-license option ensures that we are accepting the license agreement automatically.

We can see that we have about 1 successful control check and 2 control failures.

## Challenges
Read the documentation about OS Resources
Understand the difference between command and service resource
Please do whatever it takes on the production machine to achieve the following Inspec output
```sh
Profile: InSpec Profile (ubuntu)
Version: 0.1.0
Target:  ssh://root@prod-jftfefdf:22

✔  ubuntu-1.3.1: Ensure sudo is installed
   ✔  System Package sudo is expected to be installed
✔  ubuntu-1.3.2: Ensure sudo commands use pty
   ✔  Defaults use_pty
      is expected to include "Defaults use_pty"
✔  ubuntu-1.3.3: Ensure sudo log file exists
   ✔  Defaults logfile="/var/log/sudo.log"
      is expected to include "Defaults logfile="

Profile Summary: 3 successful controls, 0 control failures, 0 controls skipped
Test Summary: 3 successful, 0 failures, 0 skipped
```
