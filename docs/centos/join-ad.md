---
title: Join Active Directory
date: 2020-10-10
categories:
    - CentOS
tags:
    - CentOS
---

# Join Active Directory

## Pre-reqs

* Ensure network configuration is such that the domain can be looked up (ie. DNS configuration)

## Steps

Some other sites specify the Python v2 package of policycoreutils.  This doesn’t work on CentOS 7.x or later, so v3 was used

1. Install required software.
    ``` shell
    yum install sssd realmd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools krb5-workstation openldap-clients python3-policycoreutils
    ```
2. Join the domain with an account of appropriate rights
    ``` shell
    realm join --user=administrator contoso.com
    ```
3. Run realm command to verify join
    ``` shell
    realm list

    # Sample output:
    [root@svr31 ~]# realm list
    contoso.com
      type: kerberos
      realm-name: CONTOSO.COM
      domain-name: contoso.com
      configured: kerberos-member
      server-software: active-directory
      client-software: sssd
      required-package: oddjob
      required-package: oddjob-mkhomedir
      required-package: sssd
      required-package: adcli
      required-package: samba-common-tools
      login-formats: %U@contoso.com
      login-policy: allow-realm-logins
    ```
4. Verify administrator account
    ``` shell
    id administrator@contoso.com

    # Sample output:
    [root@svr31 ~]#  id administrator@contoso.com
    uid=740800500(administrator@contoso.com)
    gid=740800513(domain users@contoso.com)
    groups=740800513(domain users@contoso.com),
      740800520(group policy creator owners@contoso.com),
      740800512(domain admins@contoso.com),
      740800518(schema admins@contoso.com),
      740800572(denied rodc password replication group@contoso.com),
      740800519(enterprise admins@contoso.com)
    ```
5. Verify Computer object in AD (A computer object should appear in the Computers container as shown below)

    ![Image](../images/de540216-3a12-458f-becd-e72f1ec10c3d.png)

6. Edit sudo account to include sudo AD group
    ``` shell
    [root@svr31 ~]# cat /etc/sudoers.d/sudoers
    %sudoers@contoso.com      ALL=(ALL)       ALL
    ```