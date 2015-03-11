# FreeIPA

## Overview

 * Provides user authorisation and authentication
 * Ability to talk to site AD/LDAP servers
 * DNS management
 * Sudo rule management 

## Configurations
### Set administrator restrictions

For sites that wish to give privileged users access to control user and group membership, but restrict access to the administrator user and groups from modification and deletion. 
A typical example setup of user groups: 

`admins - system administrators; full system access`

`siteadmins - privileged user accounts; set permissions as deemed necessary`

`clusterusers - all regular users and administrators`

The `admins` user group exists by default. GID can be set with the `--GID=STR` if specific IDs are required. To add the recommended user groups:
```
ipa group-add siteadmins --desc="site admins"
ipa group-add cluster-users --desc="cluster users"
```

Add or import users and assign them to their appropriate groups
```
ipa user-add --first=fred --last=flinstone --email=fred.flinstone@bedrock.net
ipa user-add --first=wilma --last=flinstone --email=wilma.flinstone@bedrock.net
ipa user-add --first=great --last=gazoo --email=great.gazoo@bedrock.net
ipa group-add-member cluster-users --users=fflinstone,wflinstone,ggazoo
ipa group-add-member siteadmins --users=wflinstone
```

Add a role for the `siteadmins` usergroup
```
[root@ipa ~]# ipa role-add --desc="site admins" siteadmin
----------------------
Added role "siteadmin"
----------------------
  Role name: siteadmin
  Description: site admins
```

(not finished; requires additional privileges adding)
Add the required privileges to the `siteadmin` role previously created. Privileges can be viewed with the `ipa privilege-find` command.
```
[root@ipa ~]# ipa role-add-privilege --privileges="Modify Group membership" siteadmin
  Role name: siteadmin
  Description: site admins
  Privileges: Modify Group membership
----------------------------
Number of privileges added 1
----------------------------
```

Add more privileges for privileged users + administrators, e.g. group administration. Privileges can be searched with `ipa privilege-find KEYWORD`
```
ipa role-add-privilege --privileges="Group Administrators" siteadmin
ipa role-add-privilege --privileges="Password Policy Administrator" siteadmin
ipa role-add-privilege --privileges="User Administrators" siteadmin
```

Verify restriction on `admins` usergroup by logging in as a test member of the `siteadmins` group
```
[root@ipa ~]# kinit ggazoo
Password for ggazoo@BEDROCK.NET:
Password expired.  You must change it now.
Enter new password:
Enter it again:
[root@ipa ~]# ipa user-del admin
ipa: ERROR: Insufficient access: Insufficient 'delete' privilege to delete the entry 'uid=admin,cn=users,cn=accounts,dc=bedrock,dc=net’.
```

Verify `siteadmins` cannot place themselves or the `siteadmins` group into the `admins` group
```
[root@ipa ~]# kinit ggazoo
Password for ggazoo@BEDROCK.NET:
[root@ipa ~]# ipa group-add-member admins --users=ggazoo
  Group name: admins
  Description: Account administrators group
  GID: 1834400000
  Member users: admin, wflinstone
  Failed members:
    member user: ggazoo: Insufficient access: Insufficient 'write' privilege to the 'member' attribute of entry 'cn=admins,cn=groups,cn=accounts,dc=bedrock,dc=net'.
    member group:
-------------------------
Number of members added 0
-------------------------
[root@ipa ~]# ipa group-add-member admins --groups=siteadmins
  Group name: admins
  Description: Account administrators group
  GID: 1834400000
  Member users: admin, wflinstone
  Failed members:
    member user:
    member group: siteadmins: Insufficient access: Insufficient 'write' privilege to the 'member' attribute of entry 'cn=admins,cn=groups,cn=accounts,dc=bedrock,dc=net'.
-------------------------
Number of members added 0
-------------------------
```