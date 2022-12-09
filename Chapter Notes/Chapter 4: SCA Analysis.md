
## Appendix
### How SCA tools work
All SCA tools look for components/library/dependency files in the repository. If they find a dependency file, they scan that file for vulnerabilities. Every programming language has its own package/dependency manager.
- For *python*, the package manager is *pip* and dependency file is *requirements.txt*.
- For *node.js*, the package manager is *npm* and dependency file is *package.json*.
- For *ruby*, the package manager is *rubygems* and the dependency file is *gemfile*.
- For *java*, the package manager is *maven* and the dependency file is *pom.xml*

> **Note:** Some SCA tools like Snyk need the respective package manager to be installed before they can scan the dependecies for a package.
e.g if a project has both package.json (Javascript/node.js) and requirements.txt (python) then snyk expects npm and pip to be installed on that machine otherwise it won't be scan dependencies.
If npm is installed, it will process package.json and will ignore requirements.txt
If pip is installed on the machine, it will process requirements.txt. It won't process package.json or error out.
[SCA-works.pdf](https://github.com/jdallimoreDel/DevSecOps/files/10196320/SCA-works.pdf)
