Ansible Role: Mule
=========

Installs [Mule](https://www.mulesoft.com/) on Linux servers

Requirements
------------

Java must be available on the server. You can easily install Java using the `geerlingguy.java` role. Make sure the Java version installed meets the Mule requirements.

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
Available Variables:

| Variable Name  | Description  | Default  |
|----------------|--------------|----------|
| `mule_version` | The version of Mule you want to install. Get a look [here](https://repository-master.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/)| `4.2.1`|
| `mule_group`   | The system group to create running Mule | `mule` |
| `mule_user`   | The system user to create running Mule | `mule` |
| `mule_home`   | Directory that hosts Mule | `/opt/mule` |

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
        - geerlingguy.java
        - bmeme.mule

License
-------

MIT/BSD

Author Information
------------------
This role was created in 2021 by [Bmeme](https://www.bmeme.com).

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
