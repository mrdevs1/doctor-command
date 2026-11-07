# WP-CLI Doctor Command

WP-CLI Doctor Command is a diagnostic package for WordPress that helps developers identify potential website problems through a series of configurable health checks.

It allows developers to inspect important parts of a WordPress installation directly from the command line. Instead of manually checking different settings and components, you can run multiple checks with a single command and review the results in a structured format.

Repository: https://github.com/wp-cli/doctor-command

## Overview

The Doctor Command package adds the `wp doctor` command to WP-CLI.

It runs predefined checks against a WordPress installation and reports whether each check passes, produces a warning, or identifies an error.

Each check returns three main fields:

- **Name:** The identifier of the diagnostic check.
- **Status:** The result of the check.
- **Message:** A description of the result.

The available statuses are:

| Status | Description |
|---|---|
| `success` | The check completed successfully. |
| `warning` | The check found something that may need attention. |
| `error` | The check identified a problem or an unexpected condition. |

The package also supports custom checks through a `doctor.yml` configuration file.

## Features

- Run multiple WordPress diagnostic checks from the command line.
- Check WordPress core update status.
- Verify WordPress core checksums.
- Inspect scheduled cron jobs.
- Identify duplicate cron events.
- Check the number of active plugins.
- Detect inactive plugins.
- Review plugin and theme update availability.
- Inspect important WordPress configuration values.
- Check the size of autoloaded options.
- Create custom diagnostic checks using YAML configuration.
- Filter diagnostic results by fields and values.
- Export results in JSON, CSV, YAML, and other supported formats.
- Focus on warnings and errors using spotlight mode.

## Requirements

Before installing this package, make sure your environment has:

- WordPress installed.
- WP-CLI version 3.0 or later.
- PHP compatible with your installed WP-CLI version.
- Terminal or command-line access to your WordPress installation.

You can check your WP-CLI version with:

```bash
wp --version
```

You can also inspect your environment with:

```bash
wp --info
```

## Installation

First, make sure WP-CLI is installed and updated.

```bash
wp cli update
```

Install the latest stable version of the Doctor Command package:

```bash
wp package install wp-cli/doctor-command:@stable
```

To install the development version instead, use:

```bash
wp package install wp-cli/doctor-command:dev-main
```

After installation, verify that the command is available:

```bash
wp doctor
```

## Usage

Run WP-CLI commands from the directory containing your WordPress installation, unless you specify the installation path with the appropriate WP-CLI option.

### 1. Run the Doctor Command

Start by running:

```bash
wp doctor
```

This provides an entry point for diagnosing your WordPress installation.

### 2. Run a Specific Diagnostic Check

Use the `check` subcommand to run an individual check.

For example, to check whether WordPress core is up to date:

```bash
wp doctor check core-update
```

The result includes the check name, status, and diagnostic message.

### 3. Run All Available Checks

To execute all registered checks, use:

```bash
wp doctor check --all
```

This is useful when you want an overview of the WordPress installation without running each diagnostic separately.

### 4. Show Only Warnings and Errors

Use spotlight mode to hide successful checks and focus on results that may require attention.

```bash
wp doctor check --all --spotlight
```

This can make diagnostic output easier to review on websites with many checks.

### 5. List Available Checks

To see which diagnostic checks are registered, run:

```bash
wp doctor list
```

This displays the available check names and their descriptions.

### 6. Export Results as JSON

You can export diagnostic results in JSON format:

```bash
wp doctor check --all --format=json
```

JSON output is useful when passing diagnostic information to scripts or other applications.

### 7. Export Results as CSV

To generate CSV output, run:

```bash
wp doctor check --all --format=csv
```

CSV output can be useful for reporting, spreadsheets, and further data analysis.

### 8. Filter Diagnostic Fields

You can limit the fields displayed in the results.

For example:

```bash
wp doctor check --all --fields=name,status
```

This displays only the check name and status.

## Available Diagnostic Checks

The package includes a collection of predefined checks.

Some examples include:

| Check | Purpose |
|---|---|
| `autoload-options-size` | Checks the size of autoloaded WordPress options. |
| `core-update` | Checks whether a WordPress core update is available. |
| `core-verify-checksums` | Verifies WordPress core files against official checksums. |
| `cron-count` | Checks the number of scheduled WordPress cron events. |
| `cron-duplicates` | Looks for duplicate scheduled cron events. |
| `plugin-active-count` | Checks the number of active plugins. |
| `plugin-deactivated` | Checks for deactivated plugins. |
| `plugin-update` | Checks plugin update availability. |
| `theme-update` | Checks theme update availability. |
| `language-update` | Checks language update availability. |
| `option-blog-public` | Checks the WordPress site visibility setting. |
| `cache-flush` | Checks the cache-related condition defined by the package. |

The complete list of registered checks can be viewed with:

```bash
wp doctor list
```

## Custom Diagnostic Configuration

One of the useful features of WP-CLI Doctor Command is the ability to define additional checks through a YAML configuration file.

This allows developers to adapt diagnostic procedures to the requirements of a particular website or development environment.

### Create a Configuration File

Create a file named:

```text
doctor.yml
```

You can define a custom check in the file.

For example:

```yaml
plugin-w3-total-cache:
  check: Plugin_Status
  options:
    name: w3-total-cache
    status: uninstalled
```

This example defines a check for the installation status of the W3 Total Cache plugin.

### Run Custom Checks

Pass the configuration file using the `--config` option:

```bash
wp doctor check --all --config=doctor.yml
```

You can also select specific fields:

```bash
wp doctor check --all --fields=name,status --config=doctor.yml
```

Custom configuration makes it possible to build repeatable diagnostic procedures for different WordPress projects.

## Supported Output Formats

The `wp doctor check` command supports several output formats.

| Format | Description |
|---|---|
| `table` | Displays results in a readable terminal table. |
| `json` | Produces JSON output for scripts and integrations. |
| `csv` | Produces comma-separated values. |
| `yaml` | Produces YAML output. |
| `count` | Returns a count of matching results. |

Example:

```bash
wp doctor check --all --format=yaml
```

## Practical Use Cases

### WordPress Website Maintenance

Developers and maintenance teams can use the package to run diagnostic checks before or during routine maintenance.

### Troubleshooting Existing Websites

When taking over an existing WordPress project, developers can run the available checks to get an initial overview of its configuration and potential problems.

### Plugin and Theme Monitoring

The package can help identify plugin and theme update availability and review plugin-related conditions.

### Cron Job Inspection

WordPress cron events are important for scheduled tasks. The diagnostic checks can help identify unusual cron counts and duplicate events.

### Automated Reporting

JSON and CSV output make it possible to use diagnostic results in scripts and external reporting workflows.

### Repeatable Maintenance Procedures

Custom YAML configuration allows teams to define checks that match their own maintenance requirements.

## Example Diagnostic Workflow

A basic diagnostic workflow can be performed with the following commands:

```bash
# List available diagnostic checks
wp doctor list

# Run all registered checks
wp doctor check --all

# Focus on warnings and errors
wp doctor check --all --spotlight

# Check WordPress core update status
wp doctor check core-update

# Check cron events
wp doctor check cron-count

# Export results as JSON
wp doctor check --all --format=json
```

Running these commands provides a practical starting point for reviewing a WordPress installation.

## Important Notes

- Run commands against the correct WordPress installation.
- Review warnings and errors before making changes.
- Keep WordPress core, plugins, themes, and WP-CLI updated.
- Diagnostic results do not automatically fix identified problems.
- Back up your website before making changes to important settings or files.
- Some checks may depend on the WordPress environment and the packages available in your WP-CLI installation.

## Contributing

Contributions are welcome.

If you want to improve the project, consider:

- Reporting bugs.
- Improving documentation.
- Suggesting additional diagnostic checks.
- Fixing existing issues.
- Submitting improvements through pull requests.

Before submitting a contribution, review the existing issues and the project's contribution guidelines.

## License

This project is distributed under the MIT License.

See the [LICENSE](LICENSE) file for more information.

## Project Links

- **GitHub Repository:** https://github.com/wp-cli/doctor-command
- **WP-CLI Website:** https://wp-cli.org/
- **WP-CLI Handbook:** https://make.wordpress.org/cli/handbook/

## Conclusion

WP-CLI Doctor Command provides a convenient way to inspect WordPress installations through predefined and configurable diagnostic checks.

By combining multiple checks into a command-line workflow, it helps developers review website conditions, investigate potential issues, and create repeatable maintenance procedures.

It is a useful tool for WordPress developers, website administrators, and maintenance teams who regularly work with WP-CLI.
