Ansible role for OpenLDAP backed OpenVPN server
===============================================

This role installs an OpenVPN-server backed by an OpenLDAP server for 
authentication. LDAP users are manageable through the phpLDAPAdmin web 
interface which is also installed.

Warning
-------

The role is assumed to be installed on a dedicated machine, which means 
that any readly existing settings on the machine might be overwritten, e.g. 
the Apache listening ports.

Variables
---------

The variables that can be passed to this role and a brief description about
them are as follows:

     # Toggles for overriding configs 
     # generated in a previous run, this
     # includes certificates and keys.
     # By default all toggles are enabled and
     # configs will be overriden.
     override_openldap_config:       yes
     override_easyrsa_config:        yes
     override_openvpn_config:        yes
     override_dnsmasq_config:        yes
     override_phpldapadmin_config:   yes

     # Settings for the generated CA
     # certificate and their default
     # values.
     ca_country:                     "NL"
     ca_province:                    "UT"
     ca_city:                        "MyCity"
     ca_organization:                "MyOrganization"
     ca_email:                       "my@email.tld"
     ca_ou:                          "MyOrganizationalUnit"

     # Settings for the OpenLDAP-server and
     # their default values
     ldap_dn:                        "dc=example,dc=com"
     ldap_rootpw:                    "MyLDAPRootPassword"
     ldap_organization:              "MyOrganization"
     ldap_organization_desc:         "Description of MyOrganization"

     # The FQDN of the OpenVPN-server
     openvpn_fqdn:                   "my.server.tld"

     # Settings for the phpLDAPAdmin interface
     # which enables the administrator to manage
     # VPN-users.
     phpldapadmin_fqdn:              "my.server.tld"
     phpldapadmin_server_admin_mail: "admin@email.tld"

     # Name servers used by DNSMasq and their
     # default values
     dnsmasq_dns_1:                  "8.8.8.8"
     dnsmasq_dns_2:                  "8.8.4.4"

Examples
--------

1) Initial installation of all components.

      - hosts:  openvpn
        become: true
        roles:
         - role: edeckers.openvpn-ldap
           override_openldap_config:     yes
           override_easyrsa_config:      yes
           override_openvpn_config:      yes
           override_dnsmasq_config:      yes
           override_phpldapadmin_config: yes
           ldap_dn:                      "dc=example,dc=com"
           ldap_rootpw:                  "ThisIsMyPassword"

Note: a user ```cn=admin,dc=example,dc=com``` is created with the given password. You
can use these credentials to login to the phpLDAPAdmin webinterface at ```http://server```, 
which lets you manage VPN-users. There is a ```client.ovpn``` file available for download
at ```http://server/client.ovpn``` which a client uses to login to the VPN.

2) Disable overriding of certificates and keys on an existing installation.

      - hosts:  openvpn
        become: true
        roles:
         - role: edeckers.openvpn-ldap
           override_openldap_config:     yes
           override_easyrsa_config:      no
           override_openvpn_config:      yes
           override_dnsmasq_config:      yes
           override_phpldapadmin_config: yes
           ldap_dn:                      "dc=example,dc=com"
           ldap_rootpw:                  "ThisIsMyPassword"
