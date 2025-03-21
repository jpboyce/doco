---
title: Active Directory - 10 years later
date: 2019-02-05
categories:
    - Active Directory
tags:
    - Active Directory
---

Notes on the changes that Active Directory has experienced since it's release.

## Forest/Domain Design - In and Out

| Out | In |
|---|---|
| Multi-domain forests <br>Domain trusts | Single-domain forests <br>Federated identity & claims-based authentication with ADFS |

## General Notes

### OU Design

* Design for security first (delegation, administration, ACLs)

### Group Policy

* Scoping
    * Use WMI filters as little as possible
    * Global security group filters are good
    * Always make a deny group
* Testing
    * Can be done in production if filtered correctly

### Replication

* What's changed
    * Networks are good (more throughput, more reliable)
    * Increased need for convergance
    * People trust AD
* Notification based replication
    * Change inter-site to use notification-based
    * Reduces convergence
    * Reduces issues related to password cahnges, group changes, account lockouts, etc.

### Accounts

* Regular users in a general users OU
* Administrative users
    * In another OU
    * Define types
        * Client support – local admin on PCs
        * Server support – local admin on servers
        * Service admins- rights in Exchange, SQL,etc
        * AD data admin – can modify objects in AD
        * AD service admin – physical access to DCs, restore AD
* Groups
    * DOmain admins, enterprise admins, administrators
        * As empty as possible
    * Schema admins
        * Empty.  Add members when schema updates are to be performed
    * Built in (account/server/print/backup operators)
        * Too much access?
    * Turn on auditing to track cahnges to these groups

### Custom MMCs

* Makes transition to separate admin accounts easier
* Interesting implementations
    * RDP (not in the default MMC snapin list?)
    * Link to web address (can be used to access a UNC path using admin credentails, gets around explorer issues)
    * Saved queries (locked accounts, disabled,etc.)
    * Taskpad views (allows custom tasks/commands)