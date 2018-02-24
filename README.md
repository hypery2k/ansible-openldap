# Setup OpenLDAP on Debian

[![Build Status](https://travis-ci.org/hypery2k/ansible-openldap.svg?branch=master)](https://travis-ci.org/hypery2k/ansible-openldap)

Installs and configures OpenLDAP and phpLDAPadmin, see http://www.openldap.org/

## Requirements

Install requirements using Ansible Galaxy.
````
sudo ansible-galaxy install -r requirements.yml -f
````

## Role Variables

````
---
# defaults file for ansible-openldap
openldap_admin_password: 'P@55w0rd'
openldap_admin_user: 'admin'
openldap_base: 'dc=example,dc=org'
openldap_bind_id: 'cn={{ openldap_bind_user }},{{ openldap_base }}'
openldap_bind_user: '{{ openldap_admin_user }}'
openldap_debian_packages:
  - slapd
  - ldap-utils
  - phpldapadmin
openldap_organizationalunits:  #defines OU's to populate
  - People
  - Groups
openldap_phpldapadmin_hide_warnings: 'true'
openldap_populate: false  #defines if openldap DB should be populated with openldap_organizationalunits, openldap_posixgroups and openldap_users
openldap_posixgroups:  #defines groups to create within OU's
  - name: miners
    ou: Groups
    gidNum: 5000  #start group numbers at 5000 and up
openldap_server_host: '127.0.0.1'  #defines host for phpLDAPadmin
openldap_users:
  - FirstName: John
    LastName: Smith
    ou: People  #defines OU name
    uidNum: 10000  #start user numbers at 10000 and up
    gidNum: 5000  #defines gidNum from openldap_posixgroups
    password: 'P@55w0rd'
    loginShell: /bin/bash
    homeDirectory: /home/john
pri_domain_name: 'example.org'
````

## Dependencies

Install via info in requirements  
ansible-etc-hosts


## Example Playbook

### GitHub
````
---
- hosts: all
  become: true
  vars:
    - openldap_base: 'dc=vagrant,dc=local'
    - pri_domain_name: vagrant.local
  roles:
    - role: ansible-etc-hosts
    - role: ansible-openldap
  tasks:
````
### Galaxy
````
---
- hosts: all
  become: true
  vars:
    - openldap_base: 'dc=vagrant,dc=local'
    - pri_domain_name: vagrant.local
  roles:
    - role: mrlesmithjr.etc-hosts
    - role: mrlesmithjr.openldap
  tasks:
````

## Development

### Vagrant

Spin up Vagrant environment
````
vagrant up
````

Log into phpLDAPadmin  
http://127.0.0.1:8080/phpldapadmin
````
user: cn=admin,dc=vagrant,dc=local
password: P@55w0rd
````

## License

BSD

## Author Information

Martin Reinhardt

Based on the work of Larry Smith Jr. (@mrlesmithjr)
