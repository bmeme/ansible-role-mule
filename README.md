
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)

Ansible Role: Mule
=========

Installs [Mule Community Edition](https://www.mulesoft.com/) on Linux servers.

## Requirements
Java must be available on the server. 
You can easily install Java using the great [Geerlingguy role](https://github.com/geerlingguy/ansible-role-java) `geerlingguy.java` role. 
However, you can also use other roles, of course.

Ensure that the installed Java version meets Mule's requirements.

## Installation
This is an Ansible role distributed using Ansible Galaxy. In order to install this role you can use the following command.

`$ ansible-galaxy install bmeme.mule`

## Update
If you want to update the role, you need to pass --force parameter when installing. Please, check the following command:

`$ ansible-galaxy install --force bmeme.mule`

## Role Variables
Basic variables are:

| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `mule_version` | The version of Mule you want to install. Get a look [here](https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/)| `4.5.0`|
| `mule_group`   | The system group to create running Mule | `mule` |
| `mule_user`   | The system user to create running Mule | `mule` |
| `mule_home`   | Directory that hosts Mule | `/opt/mule` |
| `mule_remove_package`   | Remove "tar.gz" Mule package when been installed | `true` |
| `mule_restart_handler_enabled`   | Restart/Start Mule after installation | `true` |

## Mule Configuration Properties
In previous versions of this role (1.x.x), it allows a complete Mule configuration injecting template files.
Unfortunately, this approach caused some cross-compatibility problems supporting different software releases and for this reason we removed this feature.

Now this role allows to configure only basic java properties and logging properties into the `wrapper.conf` file

| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `mule_wrapper_java_command` | Java Application absolute path | `java` |
| `mule_wrapper_java_initmemory` | Initial Java Heap Size (in MB) | `1024` |
| `mule_wrapper_java_maxmemory` | Maximum Java Heap Size (in MB) | `2048` |
| `mule_wrapper_startup_timeout` | Default startup timeout | `600` |
| `mule_wrapper_console_format` | Format of output for the console.  (See docs for formats) | `M` |
| `mule_wrapper_console_loglevel` | Log Level for console output.  (See docs for log levels) | `INFO` |
| `mule_wrapper_logfile` | Log file to use for wrapper output logging. | `%MULE_BASE%/logs/%MULE_APP%.log` |
| `mule_wrapper_logfile_format` | Format of output for the log file.  (See docs for formats) | `M` |
| `mule_wrapper_logfile_loglevel` | Log Level for log file output.  (See docs for log levels) | `INFO` |
| `mule_wrapper_logfile_maxsize` | Maximum size that the log file will be allowed to grow to before the log is rolled | `1m` |
| `mule_wrapper_logfile_maxfiles` | Maximum number of rolled log files which will be allowed before old files are deleted. | `10` |
| `mule_wrapper_syslog_loglevel` | Log Level for sys/event log output.  (See docs for log levels) | `NONE` |

To best configure your Mule take a look at the official documentation [here](https://docs.mulesoft.com/general/) and customize your instance at your needs directly into your playbook.

## Dependencies
N/A

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: muleserver
      vars_files:
        - vars/main.yml
      roles:
        - geerlingguy.java # for example
        - bmeme.mule

## License
MIT

## Author Information
This role was created by [Bmeme](https://www.bmeme.com). It is actually maintained by [Daniele Piaggesi](https://github.com/g0blin79) and [Roberto Mariani](https://github.com/jean-louis).

## Credits
Building this role we are influenced by other roles we usually use/used and their approaches. 
Thanks to the great works of:
- [geerligguy](https://github.com/geerlingguy)
- [lean-delivery](https://github.com/lean-delivery)
