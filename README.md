# ldap-with-docker


Gestion por Terminal
 Instalar cliente OpenLdap con el sigueinte comando
sudo yum install openldap-clients

Listar todas las entradas en el directorio LDAP:
ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin "(objectclass=*)"

Agregar Unidad organizativa (OU) con este contenido

fichero.ldif

"dn: ou=users,dc=example,dc=org
objectClass: organizationalUnit
ou: users"

El comando es este:

ldapadd -x -h localhost -D "cn=admin,dc=example,dc=org" -w admin -f fichero.ldif

Añadir una nueva entrada LDAP utilizando LDIF:
Creamos un nuevo fichero new_entry.ldif con el sigueinte contenido

"dn: uid=user1,ou=users,dc=example,dc=org
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: User 1
sn: Lastname 1
uid: user1
uidNumber: 10001
gidNumber: 10000
homeDirectory: /home/user1
loginShell: /bin/bash
userPassword: {SSHA}KbRo+R8okbkpRQxK70BP1F0tXlXxMzSh"


Ejectuamos el comando pra añadir entrada 
ldapadd -x -h localhost -D "cn=admin,dc=example,dc=org" -w admin -f fichero.ldif
 
Modificar una entrada LDAP utilizando LDIF:
Creamos fichero modify_entry.ldif con el sigueinte contenido

"dn: uid=user1,ou=users,dc=example,dc=org
changetype: modify
replace: cn
cn: New User 1"

Ejecutamos el comando para modificar entrada
ldapmodify -x -h localhost -D "cn=admin,dc=example,dc=org" -w admin -f modify_entry.ldif 
Eliminar una entrada LDAP:
Para eliminar entrada Ejecutar el comando:
ldapdelete -x -H ldap://localhost -D "cn=admin,dc=example,dc=org" -w admin "uid=user1,ou=users,dc=example,dc=org"
Agregar una nueva unidad organizativa (OU) al directorio LDAP:
Crear fichero new_ou.ldif con el siguiente contenido

"dn: ou=employees,dc=example,dc=org
objectClass: organizationalUnit
ou: employees"

Ejecutar el comando sigueinte:
ldapadd -x -H ldap://localhost -D "cn=admin,dc=example,dc=org" -w admin -f new_ou.ldif

Eliminar una unidad organizativa (OU) del directorio LDAP:
ldapdelete -x -H ldap://localhost -D "cn=admin,dc=example,dc=org" -w admin "ou=employees,dc=example,dc=org" 

Configurar límites de consulta en el servidor LDAP:
Esto limita el número de resultados devueltos por una consulta LDAP.
Realizar una consulta LDAP con límites configurados:
ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin -z 3 "(objectclass=*)"
Exportar el contenido del directorio LDAP a un archivo LDIF:
ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin -LLL > ldap_content.ldif

