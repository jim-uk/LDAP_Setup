# LDAP_Setup

apt install slapd ldap-utils ldapscripts

Edit ldap.conf
-------------
Throughout this guide we will issue many commands with the LDAP utilities. To save some typing, we can configure the OpenLDAP libraries with certain defaults in /etc/ldap/ldap.conf (adjust these entries for your server name and directory suffix):

BASE dc=x,dc=y,dc=z

URI ldap://ldap.x.y.z

Check admin permissions
----------------------
ldapwhoami -x -D cn=admin,dc=x,dc=y,dc=z -W

Add the basic groups
--------------------
ldapadd -x -D cn=admin,dc=x,dc=y,dc=z -W -f addBasicGroup.ldif

Enable memberOf
----------------
sudo ldapadd -Q -Y EXTERNAL -H ldapi:/// -f addMemberOf.ldif

sudo ldapmodify -Q -Y EXTERNAL -H ldapi:/// -f refint1.ldif

sudo ldapadd -Q -Y EXTERNAL -H ldapi:/// -f refint2.ldif

Add Our Groups
--------------
sudo ldapadd -x -D cn=admin,dc=x,dc=y,dc=z -W -f addGroups.ldif

Configure LDAP Scripts
---------------------
sudo pico /etc/ldapscripts/ldapscripts.conf

* Edit lines: SERVER, BINDDN, SUFFIX, GSUFFIX, USUFFIX, MSUFFIX, GCLASS (to groupOfNames) and GDUMMYMEMBER

Add our Users
----------------
sudo ldapadduser <username> <group>

Add User to group
-----------------
sudo ldapaddusertogroup <username> <group>


Verification
------------
ldapsearch -x -LLL -b dc=x,dc=y,dc=z \\* +


Not Don't use:
ldapaddgroup as this throws an error even though we've added groupOfNames to ldapscripts.conf
