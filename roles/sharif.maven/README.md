Ansible Role: Maven
===================

Role to install the [Apache Maven](https://maven.apache.org) build tool.

Requirements
------------

* Ansible >= 2.7

* Linux Distribution

    * Debian Family

        * Debian

            * Jessie (8)
            * Stretch (9)

        * Ubuntu

            * Xenial (16.04)
            * Bionic (18.04)

    * RedHat Family

        * CentOS

            * 6
            * 7

        * Fedora

            * 31

    * SUSE Family

        * openSUSE

            * 15.1

    * Note: other versions are likely to work but have not been tested.

* Java SE Development Kit (JDK)

    * The required JDK version is dependent on the Apache Maven version

        | Maven Version | Minimum JDK Version |
        | ------------: | ------------------: |
        |         3.6.x |                   7 |
        |         3.5.x |                   7 |
        |         3.3.x |                   7 |
        |         3.2.x |                   6 |
        |         3.1.x |                   5 |

Role Variables
--------------

```
link_state : It sets whether to build the soft link or destroy the soft link. (Default value is  link)
file_state : It sets whether to create the file or delete the file. (Default value is present)
package_state : It sets whether to install or uninstall the package. (Default value is present)

```

### Supported Maven Versions

The following versions of Maven are supported without any additional
configuration (for other versions follow the Advanced Configuration
instructions):

* `3.6.3`
* `3.6.2`
* `3.6.1`
* `3.6.0`
* `3.5.4`
* `3.5.3`
* `3.5.2`
* `3.5.0`
* `3.3.9`
* `3.2.5`
* `3.1.1`

Advanced Configuration
----------------------

The following role variable is dependent on the Maven version; to use a
Maven version **not pre-configured by this role** you must configure the
variable below:

```yaml
# SHA256 sum for the redistributable package (i.e. apache-maven-{{ maven_version }}-bin.tar.gz)
maven_redis_sha256sum: '6e3e9c949ab4695a204f74038717aa7b2689b1be94875899ac1b3fe42800ff82'
```

Example Playbooks
-----------------

By default this role will install the latest version of Maven supported by this
role:

```yaml
---
- hosts: all
  roles:
    - role: sharif.maven
      become: yes
```

You can install a specific version of Maven by specifying the `maven_version`
(note: if the version is not currently supported by this role then additional
configuration will be required - see
[Advanced Configuration]:

```yaml
---
- hosts: servers
  roles:
    - role: sharif.maven
      become: yes
      maven_version: '3.3.9'
```

You can install the multiple versions of Maven by using this role more than
once:

```yaml
---
- hosts: servers
  roles:
    - role: sharif.maven
      become: yes
      maven_version: '3.3.9'
      maven_is_default_installation: yes
      maven_fact_group_name: maven

    - role: sharif.maven
      become: yes
      maven_version: '3.2.5'
      maven_is_default_installation: no
      maven_fact_group_name: maven_3_2
```

Role Facts
----------

This role exports the following Ansible facts for use by other roles:

* `ansible_local.maven.general.version`

    * e.g. `3.3.9`

* `ansible_local.maven.general.home`

    * e.g. `/opt/maven/apache-maven-3.3.9`

Overriding `maven_fact_group_name` will change the names of the facts e.g.:

```yaml
maven_fact_group_name: maven_3_2
```

Would change the name of the facts to:

* `ansible_local.maven_3_2.general.version`
* `ansible_local.maven_3_2.general.home`

##Installation Commands

```
ansible-playbook -i inventory maven_installation_playbook.yml -u ec2-user
```
##Uninstallation Command

```
ansible-playbook -i inventory maven_installation_playbook.yml -u ec2-user -e link_state=absent -e file_state=absent -e package_state=absent

```
