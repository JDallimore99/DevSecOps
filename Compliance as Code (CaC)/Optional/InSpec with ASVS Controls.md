# InSpec with ASVS Controls
earn how to create an InSpec profile with ASVS controls and how they help you in verifying application specific compliance requirements.

ASVS expands as Application Security Verification Standard, which is a flagship project from OWASP that provides a basis for testing web application technical security controls and also provides developers with a list of requirements for secure development. For more details about ASVS, please refer to the OWASP ASVS Project Page.
https://owasp.org/www-project-application-security-verification-standard/

## Setup Inspec
install InSpec on the system to learn Compliance as Code (CaC).

Download the InSpec debian package from the InSpec website.
```sh
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
```
Install the downloaded package.
```sh
dpkg -i inspec_4.37.8-1_amd64.deb
```
Create a new folder and cd into that folder.
```sh
mkdir cis-ubuntu
cd cis-ubuntu
```
#### Create a new InSpec profile
Create a new InSpec profile named ubuntu with the inspec init command.
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
Move to the next step and learn how to modify the new inspec profile to satisfy a few ASVS requirements.
## Add ASVS controls to the custom InSpec profile
Need to add the newly created controls into the profile and run them against a system to check for compliance.
To do this, InSpec provides us with a command resource/method.
Let’s have a look at inspec documentation for the command resource. https://docs.chef.io/inspec/resources/command/
Can use this resource to execute commands like the grep we ran in the last step. Once a command returns its output, we can use stdout(aka output) to compare it against the desired/expected value in our profile.
Use the following command to replace the inspec task in the file, ubuntu/controls/example.rb. If you wish, you can edit the file manually, using nano or any text editor.

```sh
cat > ubuntu/controls/asvs.rb <<EOL
control 'ASVS-14.4.1' do
    impact 0.7
    title 'Safe character set'
    desc 'HTTP response contains content type header with safe character set'
    describe http('https://prod-ccpwla97.lab.practical-devsecops.training') do
        its ('headers.Content-type') { should cmp 'text/html; charset=utf-8'}
    end
end

control 'ASVS-14.4.2' do
    impact 0.7
    title 'Contain Content Disposition header attachment'
    desc "Add Content-Disposition header to the server's configuration.","Add 'attachment' directive to the header."
    describe http('https://prod-ccpwla97.lab.practical-devsecops.training') do
        its ('headers.content-disposition') { should cmp 'attachment' }
    end
end

control 'ASVS-14.4.3' do
    impact 0.7
    title 'Content Security Policy Options != none / contain unsafe-inline;unsafe-eval;\* '
    desc "Ensure that CSP is not configured with the directives: 'unsafe-inline', 'unsafe-eval' and wildcards."
    describe http('https://prod-ccpwla97.lab.practical-devsecops.training') do
        its ('headers.content-security-policy') { should_not cmp 'none' }
        its ('headers.content-security-policy') { should_not include 'unsafe-inline;unsafe-eval;\*'}
    end
end

control 'ASVS-14.4.4' do
    impact 0.7
    title 'Content type Options = no sniff'
    desc 'All responses should contain X-Content-Type-Options=nosniff'
    describe http('https://prod-ccpwla97.lab.practical-devsecops.training') do
        its ('headers.x-content-type-options') { should cmp 'nosniff'}
    end
end

control 'ASVS-14.4.5' do
    impact 0.7
    title 'HSTS is using directives max-age=15724800'
    desc 'Verify that HTTP Strict Transport Security headers are included on all responses and for all subdomains, such as Strict-Transport-Security: max-age=15724800; includeSubDomains.'
    describe http('https://prod-ccpwla97.lab.practical-devsecops.training') do
        its ('headers.Strict-Transport-Security') { should match /\d/ }
    end
end

control 'ASVS-14.4.6' do
    impact 0.7
    title "'Referrer-Policy' header is included"
    desc "HTTP requests may include Referrer header, which may expose sensitive information. Referrer-Policy restiricts how much information is sent in the Referer header."
    describe http('https://prod-ccpwla97.lab.practical-devsecops.training') do
        its ('headers.referrer-policy') { should cmp 'no-referrer; same-origin' }
    end
end
EOL
```
Look for Syntax Error
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
Run the profile on the local-machine before executing it on the remote server.
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
Have about 1 successful control check because we just add the remediation into DevSecOps Box.

## Run the Inspec Profile to test for compliance against a server
Let’s try to run the custom profile created by us against the server.
Before executing the profile, we need to execute the below command.
```sh
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
```
The above command prevents the ssh agent from prompting YES or NO question.
Let’s run inspec with the following options.
```sh
inspec exec ubuntu -t ssh://root@prod-h535a4go -i ~/.ssh/id_rsa --chef-license accept
```
The flags/options used in the above commands are:
- -t : tells the target machine to run the profile against.
- -i : provides the path where the remote machine’s ssh key is stored.
- --chef-license : accept ensures that we are accepting license agreement there by preventing the inspec from prompting YES or NO question.
You can see, we got 1 control failure i.e., the production machine doesn’t follow the CIS best practice. The DevOps/Security team can run Ansible hardening scripts to ensure the production machine is hardened and continously scan the production machine for compliance.
