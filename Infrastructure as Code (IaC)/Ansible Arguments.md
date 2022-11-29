usage: ansible-doc [-h] [--version] [-v] [-M MODULE_PATH] [--playbook-dir BASEDIR]
                   [-t {become,cache,callback,cliconf,connection,httpapi,inventory,lookup,netconf,shell,vars,module,strategy,role,keyword}] [-j]
                   [-r ROLES_PATH] [-e ENTRY_POINT | -s | -F | -l | --metadata-dump] [--no-fail-on-errors]
                   [plugin [plugin ...]]

plugin documentation tool
|Argument|Description|
|:----|:----|
|plugin|Plugin|
|--metadata-dump|**For internal use only** Dump json metadata for all entries, ignores other options.|
|--no-fail-on-errors|**For internal use only** Only used for --metadata-dump. Do not fail on errors. Report the error message in the JSON instead.|
|--playbook-dir BASEDIR|Since this tool does not use playbooks, use this as a substitute playbook directory. This sets the relative path for many features including roles/ group_vars/ etc.|
|--version|show program's version number, config file location, configured module search path, module location, executable location and exit|
|-F, --list_files|Show plugin names and their source files without summaries (implies --list). A supplied argument will be used for filtering, can be a namespace or full collection name.|
|-M MODULE_PATH, --module-path MODULE_PATH|prepend colon-separated path(s) to module library (default=~/.ansible/plugins/modules:/usr/share/ansible/plugins/modules)|
|-e ENTRY_POINT, --entry-point ENTRY_POINT|Select the entry point for role(s).|
|-h, --help|show this help message and exit|
|-j, --json|Change output into json format.|
|-l, --list|List available plugins. A supplied argument will be used for filtering, can be a namespace or full collection name.
|-r ROLES_PATH, --roles-path ROLES_PATH| The path to the directory containing your roles.|
|-s, --snippet|Show playbook snippet for these plugin types: inventory, lookup, module|
|-t {become,cache,callback,cliconf,connection,httpapi,inventory,lookup,netconf,shell,vars,module,strategy,role,keyword}, --type {become,cache,callback,cliconf,connection,httpapi,inventory,lookup,netconf,shell,vars,module,strategy,role,keyword}|Choose which plugin type (defaults to "module"). Available plugin types are : ('become', 'cache', 'callback', 'cliconf','connection', 'httpapi', 'inventory', 'lookup', 'netconf', 'shell', 'vars', 'module', 'strategy', 'role', 'keyword')|
|-v, --verbose|Causes Ansible to print more debug messages. Adding multiple -v will increase the verbosity, the builtin plugins currently evaluate up to -vvvvvv. A reasonable level to start is -vvv, connection debugging might require -vvvv.|

See man pages for Ansible CLI options or website for tutorials https://docs.ansible.com
