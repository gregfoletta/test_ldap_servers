# Test LDAP Servers

This repository contains a docker-compose and LDIFs to spin up two test LDAP servers:

- retail.foletta.xyz
- wholesale.foletta.xyz

# Running

1. Edit the LDIFs in */ldifs/retail/* and */lidfs/whoelsale/* to your heart's content.
1. Spin up the servers using the command `docker-compose up -d`

# What Does It Create

- A network with the range 172.18.0.0/16
 - retail LDAP server is on 172.18.0.2
 - wholesale LDAP server ison 172.18.0.3
- Two containers, each running an OpenLDAP server
- Loads the two LDAP servers with the group and users defined in the LDIF files

# Example

```sh
bash# docker-compose up -d
Creating network "ldap_ldap" with driver "bridge"
Creating ldap_openldap_retail_1    ... done
Creating ldap_openldap_wholesale_1 ... done

bash# ldapsearch -H ldap://ldap.retail.foletta.xyz:1389 \    
      -D cn=greg,ou=users,dc=retail,dc=foletta,dc=xyz  \
       -b dc=retail,dc=foletta,dc=xyz -W
Enter LDAP Password: 
# extended LDIF
#
# LDAPv3
# base <dc=retail,dc=foletta,dc=xyz> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# retail.foletta.xyz
dn: dc=retail,dc=foletta,dc=xyz
objectClass: dcObject
objectClass: organization
dc: retail
o: example

# users, retail.foletta.xyz
dn: ou=users,dc=retail,dc=foletta,dc=xyz
objectClass: organizationalUnit
ou: users

# greg, users, retail.foletta.xyz
dn: cn=greg,ou=users,dc=retail,dc=foletta,dc=xyz
uid: greg
mail: greg@retail.foletta.xyz
cn: Greg
sn: Foletta
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
userPassword:: Zm9ydGluZXQ=
uidNumber: 1000
gidNumber: 1000
homeDirectory: /home/greg

<... output truncated ...>
```

# NAT Ports

Personally, I advertise my docker networks out to my wider network, so I don't need to NAT. If you need to NAT inbound, uncomment the 'ports' section in the docker-compose file.

# TODO

- LDAPS doesn't currently work, but should be pretty easy to enable. PRs welcome.
