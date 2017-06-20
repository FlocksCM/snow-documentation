---
title: How to create custom LDAP certificates
tags: [ldap]
keywords: ldap, certificates
last_updated: July 3, 2016
summary: "This section explains to tho create custom LDAP certificates."
sidebar: mydoc_sidebar
permalink: mydoc_how_to_create_custom_ldap_certificates.html
folder: mydoc
---

1. Access to your sNow! LDAP server through SSH as root (i.e. ldap01)
```
ssh root@ldap01
```
2. Create a new backup with the new configuration

3. Change to the /etc/ssl/certs/ and create the CA 
```
cd  /etc/ssl/certs/
```
4. Copy the files and generated certificates to /etc/openldap/certs/
```
cp /etc/ssl/certs/ /etc/openldap/certs/
```
5. Change to the /etc/ssl/certs/ directory and create the server key
```
cd  /etc/ssl/certs/
openssl genrsa -aes128 -out server.key 2048
openssl rsa -in server.key -out server.key
openssl req -new -days 3650 -key server.key -out server.csr
openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650
chmod 400 server.*
```
6. Copy the files and generated certificates to /etc/openldap/certs/
```
cp /etc/ssl/certs/server.key /etc/ssl/certs/server.crt /etc/openldap/certs/
```
7. Update the ACLs
```
chown ldap /etc/openldap/certs/*
```
8. Update LDAP server
```
ldapmodify -Y EXTERNAL -H ldapi:/// -f /root/ssl.ldif
```
9. Create a new backup with the new configuration

10. Transfer the certificates to /sNow/snow-configspace

11. Transfer the complete LDAP configuration to /sNow/snow-configspace

