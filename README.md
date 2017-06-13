openldap_server
===============

This roles installs the OpenLDAP server on the target machine. It has the
option to enable/disable SSL by setting it in defaults or overriding it.

Requirements
------------

This role requires Ansible 2.2 or higher, and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows:

``` yml
    openldap_server_domain_name: example.com    # The domain prefix for ldap
    openldap_server_rootpw: passme              # This is the password for admin for openldap
    openldap_server_enable_ssl: true            # To enable/disable ssl for the ldap
    openldap_server_country: US                 # The self signed ssl certificate parameters
    openldap_server_state: Oregon
    openldap_server_location: Portland
    openldap_server_organization: IT

    openldap_server_hashing: SSHA # The used password hashing method

    # Create users
    openldap_enable_user_creation: true
    openldap_server_users:
        william:
          password: 26April1564
          fullname: William Shakespeare
          givenname: William
          surname: Shakespeare
          email: william@example.com
          shell: /usr/bin/zsh
          phone: "555-5555-555"
        john:
          password: 31Oct1795
          fullname: John Keats
          givenname: John
          surname: Keats
          email: john@example.com
          shell: /usr/bin/bash
    openldap_server_groups:
      staff:
        - william
        - john
    openldap_server_acl:
      - what: "attrs=userPassword"
        directives:
          - by: self
            permission: write
          - by: anonymous
            permission: auth
          - by: dn.base="cn=Manager,dc=example,dc=com"
            permission: write
          - by: "*"
            permission: read
      - what: "attrs=shadowLastChange"
        directives:
          - by: self
            permission: write
          - by: "*"
            permission: read
      - what: "*"
        directives:
          - by: self
            permission: write
          - by: n.base="cn=Manager,dc=example,dc=com"
            permission: write
          - by: "*"
            permission: read

```

Examples
--------

1) Configure an OpenLDAP server without SSL:
``` yml
    - hosts: all
      sudo: true
      roles:
      - role: bennojoy.openldap_server
        openldap_server_domain_name: example.com
        openldap_server_rootpw: passme
        openldap_server_enable_ssl: false
```
2) Configure an OpenLDAP server with SSL:
``` yml
    - hosts: all
      sudo: true
      roles:
      - role: bennojoy.openldap_server
        openldap_server_domain_name: example.com
        openldap_server_rootpw: passme
        openldap_server_enable_ssl: true
        openldap_server_country: US
        openldap_server_state: Oregon
        openldap_server_location: Portland
        openldap_server_organization: IT
```
Dependencies
------------

None

License
-------

BSD

Author Information
------------------

Contains [Work from Benno Joy](https://github.com/bennojoy/openldap_server)
and [Work from b4ldr](https://github.com/b4ldr/openldap_server)
