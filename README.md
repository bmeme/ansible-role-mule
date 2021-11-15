![example workflow](https://github.com/bmeme/ansible-role-mule/actions/workflows/ci.yml/badge.svg?branch=main)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)

Ansible Role: Mule
=========

Installs [Mule](https://www.mulesoft.com/) on Linux servers.

Requirements
------------

Java must be available on the server. 
You can easily install Java using the great [Geerlingguy role](https://github.com/geerlingguy/ansible-role-java) `geerlingguy.java` role. 
But you can use others obviously.

Make sure the Java version installed meets the Mule requirements.

Installation
------------

This is an Ansible role distributed using Ansible Galaxy. In order to install this role you can use the following command.

`$ ansible-galaxy install bmeme.mule`

Update
------

If you want to update the role, you need to pass --force parameter when installing. Please, check the following command:

`$ ansible-galaxy install --force bmeme.mule`

Role Variables
--------------
Basic variables are:

| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `mule_version` | The version of Mule you want to install. Get a look [here](https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/)| `4.2.1`|
| `mule_group`   | The system group to create running Mule | `mule` |
| `mule_user`   | The system user to create running Mule | `mule` |
| `mule_home`   | Directory that hosts Mule | `/opt/mule` |
| `mule_remove_package`   | Remove "tar.gz" Mule package when been installed | `true` |
| `mule_restart_handler_enabled`   | Restart/Start Mule after installation | `true` |

Mule Configuration Properties
-----------------------------

This role injects these configuration files into mule installation folder *before* to start it:
- http-server-socket.conf
- scheduler-pools.conf
- wrapper.conf

To allow a full configuration of these files, this role makes available a _configuration object_ you can set before
running your playbook. This is the configuration object:

```
mule_default_config:
  http_server_sockets:
    sendBufferSize:
      enabled: false
      value: 1024
    receiveBufferSize:
      enabled: false
      value: 1024
    clientTimeout:
      enabled: true
      value: 60000
    sendTcpNoDelay:
      enabled: true
      value: "true"
    linger:
      enabled: false
      value: 60000
    keepAlive:
      enabled: true
      value: "false"
    reuseAddress:
      enabled: true
      value: "true"
    receiveBacklog:
      enabled: true
      value: 50
    serverTimeout:
      enabled: true
      value: 60000
  scheduler_pools:
    gracefulShutdownTimeout:
      enabled: true
      value: 15000
    cpuLight:
      threadPoolSize:
        enabled: true
        value: "2*cores"
      workQueueSize:
        enabled: true
        value: 0
    io:
      threadPoolCoreSize:
        enabled: true
        value: cores
      threadPoolMaxSize:
        enabled: true
        value: "max(2, cores + ((mem - 245760) / 5120))"
      workQueueSize:
        enabled: true
        value: 0
      threadPoolKeepAlive:
        enabled: true
        value: 30000
    cpuIntensive:
      threadPoolSize:
        enabled: true
        value: "2*cores"
      workQueueSize:
        enabled: true
        value: "2*cores"
  wrapper:
    startupTimeout: "600"
    java:
      InitMemory: "1024"
      MaxMemory: "1024"
    logging:
      console:
        format: M
        level: INFO
      logfile:
        path: "%MULE_BASE%/logs"
        filename: "%MULE_APP%.log"
        format: M
        level: INFO
        maxSize: 1m
        maxFiles: 10
      syslog:
        level: NONE
```

All values reflect the Mule configuration defaults. Then if it is your first installation and/or you don't know how to
configure Mule, don't do anything in your playbook, leave defaults.

But if you know what you are doing and want to change something you need to copy-and-paste the *ENTIRE* object
in the configuration YML file of your playbook and change only values you need.

To best configure your Mule instance take a look at the official documentation [here](https://docs.mulesoft.com/general/)

I don't know if this is the best approach to achieve this result. Probably not. But that's it...
Suggestions are welcome

Dependencies
------------
N/A

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: muleserver
      vars_files:
        - vars/main.yml
      roles:
        - geerlingguy.java # for example
        - bmeme.mule

License
-------

MIT/BSD

Author Information
------------------
This role was created in 2021 by [Bmeme](https://www.bmeme.com).

Credits
-------
Building this role we are influenced by other roles we usually use/used and their approaches. 
Thanks to the great works of:
- [geerligguy](https://github.com/geerlingguy)
- [lean-delivery](https://github.com/lean-delivery)

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
