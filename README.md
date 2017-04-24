# Requirements
* Optional : define a variable domainname to update /etc/hosts with IP hostname.domainename hostname of ldap server
Exemple of a group_vars/inventory.yml 
```
---
ldap:
   ssl: exemple_ca.crt
   server: "ldaps://ldap1/,ldaps://ldap2/"
   base: "dc=exemple,dc=com"
   bind: "cn=readonly"
   servers:
     - hostname: ldap1
       ip: 10.0.0.1
     - hostname: ldap2
       ip: 10.0.0.2
domainname: 'aws.edifixio.com'
```

# Exemple how to use :
```
- { role: ldap_client , when: ldap is defined and ldap.base is defined and ldap.server is defined }
```

# 
* Install ldap needed packages
sssd-ldap, sssd-tools, sudo (On debian family)
authconfig, sssd-ldap, sssd-tools, sudo, openldap-clients (On RedHat family)
* Push ssl certificat if any

* Update /etc/hosts for ldap server
* Update /etc/nsswitch.conf (adding sss)
* Enable sssd service at boot of the machine
* Execute authconfig (in RedHat family) to configure pam and ...
* Execute pam-auth-update (in Debian family) to configure pam
* Enable Create homedirectory at first connexion via pam_mkhomedir.so (on Debian Family)
* Configure /etc/openldap/ldap.conf for default ldap parameters in ldapsearch
* SSHD : add AuthorizedKeysCommand & AuthorizedKeysCommandUser into /etc/ssh/sshd_config to get ssh key from ldap

