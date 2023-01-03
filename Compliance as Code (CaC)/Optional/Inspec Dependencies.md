# Inspec Dependencies
Learn how inspec dependencies works and how they help you in checking the compliance requirements.
## Install Inspec
Download the Inspec debian package from the InSpec website.
```sh
wget https://packages.chef.io/files/stable/inspec/4.37.8/ubuntu/18.04/inspec_4.37.8-1_amd64.deb
```
Install the downloaded package.
```sh
dpkg -i inspec_4.37.8-1_amd64.deb
```
#### Create the profile
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
 ## Inspec Dependencies
In addition to its own controls, an InSpec profile can bring in the controls from another InSpec profile.
Additionally, when inheriting the controls of another profile, a profile can skip or even modify those included controls.
#### Defining the Dependency
To do so, we need to specify the included profiles into the including profile’s inspec.yml file in the depends section.
For each profile to be included, a location for the profile from where to be fetched and a name for the profile should be included.
We can include different sources of the profiles such as a path, URL, GitHub, supermarkets or compliance. In this exercise, we fetch 2 profiles, the SSH baseline from GitHub via the URL and Linux Baseline from the Supermarket.
```sh
rm ubuntu/inspec.yml
```
```sh
cat >> ubuntu/inspec.yml <<EOL
name: profile-dependency
title: Profile with Dependencies
maintainer: InSpec Authors
copyright: InSpec Authors
copyright_email: support@chef.io
license: Apache-2.0
summary: InSpec Profile that is only consuming dependencies
version: 0.2.0
depends:
  - name: SSH baseline
    url: https://github.com/dev-sec/ssh-baseline
  - name: Linux Baseline
    supermarket: dev-sec/linux-baseline
EOL
```
The inspec.yml file will be read in order to source any profile dependencies when you execute a local profile. Thereafter, it will cache the dependencies locally and generate an inspec.lock file.
```sh
cd ubuntu
```
```sh
inspec vendor
```
```sh
cd ..
```
#### Run the Inspec Tool to test for compliance against a server
Let’s try to run the custom profile created by us against the server. Before executing the profile we need to execute the below command to avoid being prompted with Yes or No when connecting to a server via ssh.
```sh
echo "StrictHostKeyChecking accept-new" >> ~/.ssh/config
```
This commands prevent the ssh agent from prompting YES or NO question`
Let’s run inspec with the following options.
```sh
inspec exec ubuntu -t ssh://root@prod-A89Mmt64 -i ~/.ssh/id_rsa --chef-license accept
```
Woah! there’s lots going on here. Lets explore these options one by one.

The flags/options used in the above commands are:

- -t : tells the target machine to run the profile against.
- -i : provides the path where the remote machine’s ssh key is stored.
- --chef-license : accept ensures that we are accepting license agreement there by preventing the inspec from prompting YES or NO question.
## Additional Resources
Additional Resources
- https://docs.chef.io/inspec/profiles
- https://supermarket.chef.io/tools?q=baseline&platforms%5B%5D=




