
## Appendix
### How SCA tools work
All SCA tools look for components/library/dependency files in the repository. If they find a dependency file, they scan that file for vulnerabilities. Every programming language has its own package/dependency manager.
For *python*, the package manager is *pip* and dependency file is *requirements.txt*.
For *node.js*, the package manager is *npm* and dependency file is *package.json*.
For *ruby*, the package manager is *rubygems* and the dependency file is *gemfile*.
For *java*, the package manager is *maven* and the dependency file is *pom.xml*
