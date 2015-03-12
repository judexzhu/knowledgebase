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

`AdminUsers - privileged user accounts; set permissions as deemed necessary`

`ClusterUsers - all regular users and administrators`

The `admins` user group exists by default. GID can be set with the `--GID=STR` if specific IDs are required. If not previously done so, add the recommended user groups:
```
ipa group-add AdminUsers --desc="site admins"
ipa group-add ClusterUsers --desc="cluster users"
```

Add or import users and assign them to their appropriate groups
```
ipa user-add --first=fred --last=flinstone --email=fred.flinstone@bedrock.net
ipa user-add --first=wilma --last=flinstone --email=wilma.flinstone@bedrock.net
ipa user-add --first=great --last=gazoo --email=great.gazoo@bedrock.net
ipa group-add-member ClusterUsers --users=fflinstone,wflinstone,ggazoo
ipa group-add-member AdminUsers --users=wflinstone
```

Add a role for the `AdminUsers` usergroup. Each role contains configurable amount of privileges available to a designated user or group. 
```
[root@ipa ~]# ipa role-add --desc="site admins" AdminUsers
----------------------
Added role "AdminUsers"
----------------------
  Role name: AdminUsers
  Description: site admins
```

(not finished; requires additional privileges adding)

Add the required privileges to the `AdminUsers` role previously created. Privileges can be viewed with the `ipa privilege-find` command.
```
[root@ipa ~]# ipa role-add-privilege --privileges="Modify Group membership" AdminUsers
  Role name: AdminUsers
  Description: site admins
  Privileges: Modify Group membership
----------------------------
Number of privileges added 1
----------------------------
```

Add more privileges for privileged users + administrators, e.g. group administration. Privileges can be searched with `ipa privilege-find KEYWORD`
```
ipa role-add-privilege --privileges="Group Administrators" AdminUsers
ipa role-add-privilege --privileges="Password Policy Administrator" AdminUsers
ipa role-add-privilege --privileges="User Administrators" AdminUsers
```

Verify restriction on `admins` usergroup by logging in as a test member of the `AdminUsers` group
```
[root@ipa ~]# kinit ggazoo
Password for ggazoo@BEDROCK.NET:
Password expired.  You must change it now.
Enter new password:
Enter it again:
[root@ipa ~]# ipa user-del admin
ipa: ERROR: Insufficient access: Insufficient 'delete' privilege to delete the entry 'uid=admin,cn=users,cn=accounts,dc=bedrock,dc=net’.
```

Verify `AdminUsers` cannot place themselves or the `AdminUsers` group into the `admins` group
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
[root@ipa ~]# ipa group-add-member admins --groups=AdminUsers
  Group name: admins
  Description: Account administrators group
  GID: 1834400000
  Member users: admin, wflinstone
  Failed members:
    member user:
    member group: AdminUsers: Insufficient access: Insufficient 'write' privilege to the 'member' attribute of entry 'cn=admins,cn=groups,cn=accounts,dc=bedrock,dc=net'.
-------------------------
Number of members added 0
-------------------------
```