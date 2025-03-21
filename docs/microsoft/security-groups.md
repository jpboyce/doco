---
title: Security Groups
date: 2019-02-05
categories:
    - Active Directory
tags:
    - Active Directory
---

Notes on the different types of Active Directory security groups

## Group Scope

| Group Scope | Group can include as members... | Group can be assigned permissions in... | Group scope can be converted to...|
|---|---|---|---|
| Universal | Accounts from any domain within the forest in which this Universal Group resides<br>Global groups from any domain within the forest in which this Universal Group resides<br>Universal groups from any domain within the forest in which this Universal Group resides | Any domain or forest | Domain local<br>Global (as long as no other universal groups exist as members) |
| Global | Accounts from the same domain as the parent global group<br>Global groups from the same domain as the parent global group | Member permissions can be assigned in any domain | Universal (as long as it is not a member of any other global groups)|
Domain local | Accounts from any domain<br>Global groups from any domain<br>Universal groups from any domain<br>Domain local groups but only from the same domain as the parent domain local group | Member permissions can be assigned only within the same domain as the parent domain local group | Universal (as long as no other domain local groups exist as members)|

| Group Scope | Members from same domain | Members from other domain, same forest | Members from external domain/forest (via trust) |
|---|---|---|---|
| Local | <ul><li>Users</li><li>Computers</li><li>Global Groups</li><li>Universal Groups</li><li>Domain Local</li><li>Other local groups</li></ul> | <ul><li>Users</li><li>Computers</li><li>Global Groups</li><li>Universal groups</li></ul> | <ul><li>Users</li><li>Computers</li><li>Global groups</li></ul> |
| Domain Local | <ul><li>Users</li><li>Computers</li><li>Global</li><li>Universal</li><li>Domain Local</li></ul> | <ul><li>Users</li><li>Computers</li><li>Global</li><li>Universal</li></ul> | <ul><li>Users</li><li>Computers</li><li>Global</li></ul> |
| Universal | <ul><li>Users</li><li>Computers</li><li>Global</li><li>Universal</li></ul> | <ul><li>Users</li><li>Computers</li><li>Global</li><li>Universal</li></ul> | <ul><li>N/A</li></ul> |
| Global | <ul><li>Users</li><li>Global groups</li></ul> | <ul><li>N/A</li></ul> | <li>N/A</li></ul> |

## When To Use A Type of Group

* Domain Local – Access/Permission to a resource (ie. Users -> Domain Local Group -> Folder/Printer)
* Global - Maintenance of objects where change doesn't require replication outside of the domain (ie. Users/computer accounts in that domain) or for setting permissions on domain directory objects that are replicated in the Global Catalog.  Suited for use as Role groups.  Only replicated in the domain they exist in.  Most limited in membership (only same domain), most flexible in availability (available in same domain/forest, trusted forests)
* Universal – To consolidate groups that span domains (Users -> Global Group -> Universal Group).  Changes in the Global Group membership won't impact the Universal group.  Useful in multi-domain forests.  Replicated to global catalog.  Can't include members from another forest,

## Best Practice Model

Use the **`IGDLA`** (Identity, Global Group, Domain Local group, Access) model.  Previously known as **`AGDLP`** (Account, Global group, Domain Local Group, Permissions).

* Users in "Roles" Groups (human readable, align with HR/position info if possible)
* Define naming convention for other groups, ie. **`<grouptype>-<resource>-<right>`**, G-Finance-Read
* Only Domain Local and Local groups can include objects from another forest
* Nesting strategy
    * Basic (**`IGDLA`**) - Identity (user account) -> Global Group (role group) -> Domain Local (defines resource & rights) -> ACL on resource
    * Recommended/multi-domain (**`IGUDLA`**) - Identity (user account) -> Global Group (role group) -> Universal Group (linking/glue group for other domains) -> Domain Local (defines resource & rights) -> ACL on resource
* Group membership is evaluated when the user logs on (security token generation) so any membership changes require a relogon (still true for claims/other methods?)