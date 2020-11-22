# Ansible Role: Java

Installs Java for RedHat/CentOS and Debian/Ubuntu linux servers.

## Role Variables

Available variables are listed below, along with default values:

    # The defaults provided by this role are specific to each distribution.
    java_packages:
      - java-1.8.0-openjdk


## Prerequisite

Ansible

## Example Playbook (using default package)

```yaml
---
- hosts: servers
    roles:
      - role: sharif.java
        become: yes
```

## Example Playbook

For RHEL / CentOS:
```yaml
---
- hosts: all
  roles:
    - role: sharif.java
      when: "ansible_os_family == 'RedHat'"
      become: yes
```

For Ubuntu :
```yaml
---
- hosts: server
    tasks:
      - name: installing repo for Java 8 in Ubuntu
        apt_repository: repo='ppa:openjdk-r/ppa'
    
- hosts: server
    roles:
      - role: sharif.java
        when: "ansible_os_family == 'Debian'"
        java_packages:
          - openjdk-8-jdk
```

## Installation Command
```
ansible-playbook -i inventory java_installation_playbook.yml -u ec2-user
```
## Uninstallation Command
```
ansible-playbook -i inventory java_installation_playbook.yml -u ec2-user -e shall_remove=yes
```
