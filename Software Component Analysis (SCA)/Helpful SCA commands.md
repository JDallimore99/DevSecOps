# Helpful SCA commands
|Command|Description|Example|
|:---|:-----|:---|
|git clone|Used for donwloading source code in terminal|```git clone https://gitlab.practical-devsecops.training/pdso/django.nv webapp```|
|pip3 install|Install safety tools|```pip3 install safety==2.1.1```|
|safety check|The command used to complete the safety software component analysis check|```safety check -r requirements.txt --json \| tee safety-output.json```|
|tee|Show the output and store it in a file simultaneously|```safety check -r requirements.txt --json \| tee safety-output.json```|
|-r|Flag used to specify the input file|```safety check -r requirements.txt --json \| tee safety-output.json```|
|--json|Flag tells that output should be in the json format|```safety check -r requirements.txt --json \| tee safety-output.json```|
|docker pull|The docker pull command serves for downloading Docker images from a registry. By default, the docker pull command pulls images from Docker Hub, but it is also possible to manually specify the private registry to pull from.|
|docker run|When you use the basic run command, Docker automatically generates a container name with a string of randomly selected numbers and letters. Since there is a slim chance you will be able to remember or recognize the containers by these generic names, consider setting the container name to something more memorable.|
|curl|A command line tool that enables data transfer over various network protocols|```curl -sL https://deb.nodesource.com/setup_12.x \| bash -```|
|apt install|Install deb packages on Ubuntu, Debian, and related Linux distributions.|```apt install nodejs -y```|
|npm install|This command installs a package, and any packages that it depends on. If the package has a package-lock or shrinkwrap file, the installation of dependencies will be driven by that, with an npm-shrinkwrap.json taking precedence if both files exist.|```npm install -g retire@3.0.6```|
|cat|Explore the contents of a file|```cat package.json```|
|retire|Command to run the retire software component analysis|```retire --outputformat json --outputpath retire_output.json```|
|--outputpath|flag specifies the output file path|```retire --outputformat json --outputpath retire_output.json```|
|--outputformat |flag specifies the output format|```retire --outputformat json --outputpath retire_output.json```|
|\| jq|The JQ command is used to transform JSON data into a more readable format and print it to the standard output on Linux. The JQ command is built around filters which are used to find and print only the required data from a JSON file.|```cat retire_output.json \| jq```|
|--severity|Specify the bug severity level from which the process fails. Allowed levels none, low, medium, high,|```retire --severity high --outputformat json --outputpath retire_output.json```|
|--ignorefile|Custom ignore file, defaults to .retireignore / .retireignore.json|```retire --severity high --ignorefile .retireignore.json --outputformat json --outputpath retire_output.json```|
