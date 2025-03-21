---
title: Creating Platforms for Test Kitchen
date: 2018-06-10
status: deprecated
categories:
    - Chef
tags:
    - Chef
    - Deprecated
---

# Creating Platforms for Test Kitchen

A Platform for Test Kitchen is a target environment for Test Kitchen to execute on.  Often a form of Virtual Machine.

## Packer Templates

A Packer Template is a JSON file that defines the characteristics of a Packer output.  It has some main sections:

* builders - Defines the so-called builders that are used to create the machine images.  These are things like VirtualBox, VMware, etc.
* provisioners - This section defines software that will be installed and configured on the machine
* post-processors - These define steps after the machine is built
* variables - Can be used to customise settings
* communicators - This specifies the interface that Packer uses to get assets on the machine and execute them.  Options are winrm or ssh

Key parts of the vbox-2016.json file that is used:

| Section | Item | Notes |
|---|---|---|
| builders | vboxmanage | This sets things like RAM and CPU.  I've added some extra customisations that turn on virtualisation options and set the clipboard to bidirectional |
| builders | hard_drive_interface | This normally defaults to ide.  I've changed this to sata.  Originally tried scsi (which would've made the controller an LSI Logic but installs errored out) |
| builders | disk_size | Added this to verify that customising C: size can be done (internal default from packer is 40GB) |
| builders | hard_drive_nonrotational | Setting this to true tells the OS that the HD is a SSD and stops it doing certain tasks like defragging |
| post-processors | output | This creates a box file ready for immediate use by Vagrant with a timestamped name. |
| post-processors | vagrant_template | The vagrant template file used when the box file is created.  The only items of note appears to be an override setting that resets memory back to 1GB |

As a result of the json file used, packer creates a box file that can be picked up by Vagrant.

## Vagrant

Vagrant is used to create "boxes".  These boxes are based on the output from Packer and are used by Test Kitchen to spin up virtual machines for testing (Test Kitchen can use a number of providers to provision test machines and has built in support for Vagrant).  To add a box file, use the command:

``` shell
vagrant box add <boxname> <box file path>
```

Where **`<boxname>`** is the name the box will have once created and **`<box file path>`** is the location of the file that Packer created.  Once created, you can verify with a list command.  Example output is shown below.

``` shell
vagrant box add windows2016 C:\gitrepo\packer-templates\windows2016-20180609-virtualbox.box
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'windows2016' (v0) for provider:
    box: Unpacking necessary files from: file:///C:/gitrepo/packer-templates/windows2016-20180609-virtualbox.box
    box: Progress: 100% (Rate: 933M/s, Estimated time remaining: --:--:--)
==> box: Successfully added box 'windows2016' (v0) for 'virtualbox'!
vagrant box list
windows2016 (virtualbox, 0)
```

## Test Kitchen

Test Kitchen is used to test cookbooks as part of local development.  The main items of note in relation to this are:

* A kitchen.yml file in the cookbook folder that defines settings like platforms, suites, etc
* The platforms defined in the yml file are operating systems that the cookbook will be tested on.  Multiple platforms can be specified (such as different distributions and versions of Linux)
* A suite is a grouping of settings to define a test scenario.  These settings can include a runlist (cookbooks/recipes to be executed), tests (for testing phases) and attributes
* An instance is a combination of a specific platform and specific suite.  The instance is referenced by combining the names of the platform and suite.  For example, if the platform being used is windows2016 and the suite is default, then the * instance would be default-windows2016
* The driver is what is used to create, manage and destroy the instance.  In this case, it is Vagrant

The typical lifecycle of using Test Kitchen involves:

1. Kitchen Create to create your instance
2. Kitchen Converge to apply the cookbook settings (sometimes a 2nd converge to ensure your cookbook is idempotent)
3. Kitchen Verify to run your tests to confirm the cookbook is working as expected
4. Kitchen Destroy to destroy the instance when finished

It is possible to use kitchen test which will perform perform this sequence end to end.  Typically you would use this to verify the end to end process of a cookbook (for example, on multiple platforms and suites), while the steps listed above are used during day to day development.

!!! note "Kitchen and Platforms"

    Most Kitchen commands can accept a Platform option.  If you do not specify a Platform, then the command will execute on all possible instances as defined in your kitchen.yml file (that is, # of platforms x # of suites).

You can use the Kitchen List command to quickly see what Instances are available based on the configuration of your kitchen.yml file.  For example:

``` powershell
PS C:\gitrepo\cb_vra_iaas> kitchen list
Instance              Driver   Provisioner  Verifier  Transport  Last Action    Last Error
default-windows-2016  Vagrant  ChefZero     Inspec    Winrm      <Not Created>  <None>
```

On the basis of this, Kitchen commands can be executed referencing this instance name specifically.  The commands also use "fuzzy matching" or some form of regular expression.  So specifying just "default" would create instances that use the default suite but use all available platforms (for a scenario where you want to run the default suite on all platforms) or alternatively, specify the platform name by itself (which would create instances of that OS type and for all defined suites if you wanted to run all scenarios on that one OS type).

!!! warning "Vagrant Box Name and Platform Name"

    Ensure the name used to create the Vagrant Box is the same name used for the Platform in kitchen.yml  If the names don't match, the kitchen commands will fail due to the kitchen-vagrant driver being unable to find the Vagrant box, as shown below:
    ``` powershell
    PS C:\gitrepo\cb_vra_iaas> kitchen create
    -----> Starting Kitchen (v1.21.2)
    -----> Creating <default-windows-2016>...
           Bringing machine 'default' up with 'virtualbox' provider...
           ==> default: Box 'windows-2016' could not be found. Attempting to find and install...
               default: Box Provider: virtualbox
               default: Box Version: >= 0
           ==> default: Box file was not detected as metadata. Adding it directly...
           ==> default: Adding box 'windows-2016' (v0) for provider: virtualbox
               default: Downloading: windows-2016
        default:
           An error occurred while downloading the remote file. The error
           message, if any, is reproduced below. Please fix this error and try
           again.

           Couldn't open file /C:/gitrepo/cb_vra_iaas/.kitchen/kitchen-vagrant/default-windows-2016/windows-2016
    ```
    In this example, the platform name was "**`windows-2016`**" while the Vagrant box was "**`windows2016`**" (no hyphen).

## Extra Considerations

Some items of extra consideration:

* While VirtualBox has a number of virtual disk formats, it appears when creating a Packer template, the created VirtualBox system defaults to VMDK.  VMDK disks are generally fixed in size and can't be expanded after the fact
* This means if you need a larger C: drive, it has to be performed as part of the initial template creation
* While it may be possible to increase the disk size later in the chain, it may be too troublesome to do so