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

# NAT Ports

Personally, I advertise my docker networks out to my wider network, so I don't need to NAT. If you need to NAT inbound, uncomment the 'ports' section in the docker-compose file.

# TODO

- LDAPS doesn't currently work, but should be pretty easy to enable. PRs welcome.
