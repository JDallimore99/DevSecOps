Commands:
|Argument|Description|
|:----|:----|
|inspec archive PATH|# archive a profile to tar.gz (default) or zip|
|inspec artifact SUBCOMMAND|# Manage Chef InSpec Artifacts|
|inspec automate SUBCOMMAND or compliance SUBCOMMAND|# Chef Automate commands|
|inspec check PATH|# verify all tests at the specified PATH|
|inspec clear_cache|# clears the InSpec cache. Useful for debugging.|
|inspec detect|# detect the target OS|
|inspec env|# Output shell-appropriate completion configuration|
|inspec exec LOCATIONS|# Run all tests at LOCATIONS.|
|inspec habitat SUBCOMMAND|# Manage Habitat with Chef InSpec|
|inspec help [COMMAND]|# Describe available commands or one specific command|
|inspec init SUBCOMMAND|# Generate InSpec code|
|inspec json PATH|# read all tests in PATH and generate a JSON summary|
|inspec plugin SUBCOMMAND|# Manage Chef InSpec and Train plugins|
|inspec shell|# open an interactive debugging shell|
|inspec supermarket SUBCOMMAND ...|# Supermarket commands|
|inspec vendor PATH|# Download all dependencies and generate a lockfile in a `vendor` directory|
|inspec version|# prints the version of this tool|

Options:
|Argument|Description|
|:----|:----|
|l, [--log-level=LOG_LEVEL]                       # Set the log level: info (default), debug, warn, error
|[--log-location=LOG_LOCATION]                  # Location to send diagnostic log messages to. (default: $stdout or Inspec::Log.error)
|[--diagnose], [--no-diagnose]                  # Show diagnostics (versions, configurations)
|[--color], [--no-color]                        # Use colors in output.
|[--interactive], [--no-interactive]            # Allow or disable user interaction
|[--disable-user-plugins]                       # Disable loading all plugins that the user installed.
|[--enable-telemetry], [--no-enable-telemetry]  # Allow or disable telemetry
|[--chef-license=CHEF_LICENSE]                  # Accept the license for this product and any contained products: accept, accept-no-persist, accept-silent


About Chef InSpec:
  Patents: chef.io/patents
