
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
This role injects these configuration files into mule installation folder *before* to start it:
- http-server-socket.conf
- scheduler-pools.conf
- wrapper.conf

and allows to customize default values of these files using the following variables:

### http-server-socket.conf
| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `http_server_sockets_sendBufferSize` | The size of the buffer (in bytes) used when sending data, set on the socket itself.| `1024`|
| `http_server_sockets_receiveBufferSize`   | The size of the buffer (in bytes) used when receiving data, set on the socket itself. Commented. | `1024` |
| `http_server_sockets_clientTimeout`   | The SO_TIMEOUT value on client sockets | `60000` |
| `http_server_sockets_sendTcpNoDelay`   | Transmitted data is not collected together for greater efficiency but sent immediately. | `true` |
| `http_server_sockets_linger`   | The SO_LINGER value. | `60000` |
| `http_server_sockets_keepAlive`   | SO_KEEPALIVE behavior on open sockets. | `false` |
| `http_server_sockets_reuseAddress`   | Enabling SO_REUSEADDR prior to binding the socket using bind(SocketAddress) allows the socket to be bound even though a previous connection is in a clientSocketTimeout state. | `true` |
| `http_server_sockets_receiveBacklog`   | The maximum queue length for incoming connections. | `50` |
| `http_server_sockets_serverTimeout`   | The SO_TIMEOUT value when the socket is used as a server. | `60000` |

### scheduler-pools.conf
| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `scheduler_pools_gracefulShutdownTimeout` | The maximum time (in milliseconds) to wait until all tasks in all the runtime thread pools have completed execution when stopping the scheduler service.| `15000`|
| `scheduler_pools_cpuLight_threadPoolSize`   | The number of threads to keep in the cpu_lite pool, even if they are idle. | `2*cores` |
| `scheduler_pools_cpuLight_workQueueSize`   | The size of the queue to use for holding cpu_lite tasks before they are executed. | `0` |
| `scheduler_pools_io_threadPoolCoreSize`   | The number of threads to keep in the I/O pool. | `cores` |
| `scheduler_pools_io_threadPoolMaxSize`   | The maximum number of threads to allow in the I/O pool. | `max(2, cores + ((mem - 245760) / 5120))` |
| `scheduler_pools_io_workQueueSize`   | The size of the queue to use for holding I/O tasks before they are executed. | `0` |
| `scheduler_pools_io_threadPoolKeepAlive`   | Maximum time (in milliseconds) that excess idle threads will wait for new tasks before terminating. | `30000` |
| `scheduler_pools_cpuIntensive_threadPoolSize`   | The number of threads to keep in the cpu_intensive pool, even if they are idle. | `2*cores` |
| `scheduler_pools_cpuIntensive_workQueueSize`   | The size of the queue to use for holding cpu_intensive tasks before they are executed. | `2*cores` |

### wrapper.conf
| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `wrapper_startupTimeout` | Increase the default startup timeout so that the JVM has enough time to download the required jars on a slow connection. | `600`|
| `wrapper_java_InitMemory`   | Initial Java Heap Size (in MB). | `1024` |
| `wrapper_java_MaxMemory`   | Maximum Java Heap Size (in MB). | `1024` |
| `wrapper_logging_console_format`   | Format of output for the console.  (See docs for formats) | `M` |
| `wrapper_logging_console_level`   | Log Level for console output. | `INFO` |
| `wrapper_logging_logfile_path`   | The path of log file to use for wrapper output logging. | `%MULE_BASE%/logs` |
| `wrapper_logging_logfile_filename`   | Log file to use for wrapper output logging.. | `%MULE_APP%.log` |
| `wrapper_logging_logfile_format`   | Format of output for the log file. | `M` |
| `wrapper_logging_logfile_level`   | Log Level for log file output. | `INFO` |
| `wrapper_logging_logfile_maxSize`   | Maximum size (in bytes) that the log file will be allowed to grow to before the log is rolled. | `1m` |
| `wrapper_logging_logfile_maxFiles`   | Maximum number of rolled log files which will be allowed before old files are deleted. | `10` |
| `wrapper_logging_syslog_level`   | Log Level for sys/event log output. | `NONE` |

To allow a full configuration of these files, this role makes available an advanced configuration variable where you can  add custom properties. These properties will be added to relative file during role execution and before to startup Mule for the first time:

**http-server-socket.conf**
```
# Custom configuration items
# Example:
#
# http_server_extended_configuration: |-
#   org.mule.runtime.http.server.socket.new_amazing_property=value
#
http_server_extended_configuration: ""
```

**scheduler-pools.conf**
```
# Custom configuration items
# Example:
#
# scheduler_pools_extended_configuration: |-
#   org.mule.runtime.scheduler.new_amazing_property=value
#
scheduler_pools_extended_configuration: ""
```

**wrapper.conf**
```
# Custom configuration items
# Example:
#
# wrapper_extended_configuration: |-
#   wrapper.new_amazing_property=value
#
wrapper_extended_configuration: ""
```
To best configure your Mule instance take a look at the official documentation [here](https://docs.mulesoft.com/general/)

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
