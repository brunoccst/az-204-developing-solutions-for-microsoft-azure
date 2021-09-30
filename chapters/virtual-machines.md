# Virtual Machines
A Virtual Machine (VM) is the virtualization/emulation of a computer system. In other words, a Virtual Machine is a server that _acts and looks like_ a physical machine: you have control over it, you can install/uninstall software in it and configure it as you want, however, you have no control over the physical machine. You can usually access such VMs with an Remote Desktop Protocol tool, such as Remote Desktop Connection, and manage the VM as you would in your own machine.

Azure provides Virtual Machines as _Infrastructure as a Service_ (IaaS). You can choose between a variety of VM sizes, depending on your needs: general usage; memory optimization; CPU optimazation; etc.
Each size specification also affects the pricing. You are also limited to use either Windows or Linux systems (and their versions/variations).
All Azure VM have their own public IP address, meaning that you can access it from the outside - unless specifically set not to.

## Pricing
Azure offers a lot of configuration & princing options, ranging from cents to a few dollars per hour, depending on the VM instance you selected.
The VM instances with more space and/or compute power cost more, while the smaller ones and/or less powerfull are cheaper.

The standard payment way is the _Pay as you go_, but if you select a VM instance from a specific range (around the mid price to the high prices), you have the option to select a payment commitment of a specific time period (e.g.: reserve a VM for one year) to receive discounts.
This means that you can have the three following payment options for VMs:
* Pay as you go
* 1 year reserved (~60% discount)
* 3 year reserved (~78% discount)

If you are sure you're going to use a VM for a year or three years, selecting the respective payment commitment can save you money.
If you choose the _pay as you go_ option, you will be charged the standard way - per usage.

You can also select whether you want the compute payment to be done **monthly** or **upfront**.

## Creating a VM - Basics

When creating a VM, you will find the following options:

![creating-a-virtual-machine-basics](https://user-images.githubusercontent.com/13342183/135273657-92c11607-de48-48aa-9174-52568482a9b7.png)

### Project details

#### Subscription
A subscription is needed in order to receive the bills related to the VM usage.

####  Resource group
 A resource group is also needed to organize your VM into a _folder like structure_, giving you the option to manage a group of resources.

### Instance details

#### Virtual machine name

A VM in Azure has two distinct names - virtual machine name (used as AZ resource identifier) and guest host name (the name displayed when you, for example, access the VM through the Remote Desktop Connection). If you create the VM from the Azure Portal, both the VM name the guest host name will be the same.

#### Region

The geographical region where your Virtual Machine is deployed.

This is a very important configuration for mainly three reasons:
1) You will probably always want to create a VM near its users, so you acheive better perfomance and faster responses.
2) It can happen that your VM's data has a restriction and can't leave a specific region. This means that its data storage must be placed within your allowed regions.
3) It affects the costs (usually because of the costs of providing service in the respective region).

Note that you may not find all the existent sizes in every region.

### Availability options (depending on the region)

Allow us to tell Azure to physically place the VM farther apart from other machines we also created in order to achieve a higher availability (in case of problems that may occur, e.g.: power outage). This is optional. If you don't set it, Azure will place your VM in any availabilty zone or set within the selected region.

### Image

The Virtual Machine image, e.g.: Windows Server 2019, Ubuntu, etc.

### Azure Spot instance

It happens that sometimes Azure has some unused compute resources. Instead of simply leaving them unused, Azure offers its clients the ability to rent them temporarily at a discounted price until another client makes usage of it for the full price. This is the concept of _Spot instance_: the ability of using your VM for a short period time at discounted price while the VM resource is not being used by anyone.

You can select if it's either a _yes_ or _no_.

#### Size

The size & processing power configuration of your machine, e.g.: CPU, memory size, etc.
The recommended size is **Standard DS1**.

### Administrator account

In this section you can define the credentials for the administrator account - the account that has control over the entire VM.


### Inbound port rules

In this section you can configure if your VM is accessible from the public internet through network ports and, if yes, which network ports are accepting the public access.
You can choose multiple ports between the following:
* HTTP (80)
* HTTPS (443)
* SSH (22)
* RDP (3389)

Note: if you want your machine to be accessible through RDP, enable the RDP port.

### Save money

If you already have a Windows Server license, you can use it in this VM and save almost half of the costs.

## Creating a VM - Disks

All Virtual Machines require a disk to store the internal workings of it (e.g.: C:/, D:/, etc.).
You can select which disk configuration you want at the _Disk_ tab.

![creating-a-virtual-machine-disk](https://user-images.githubusercontent.com/13342183/135426773-abe847b8-8157-4200-b720-88e4b65d649a.png)

### OS disk type

The type of disk system you want.

**Azure recommmends Premium SSD**, but you can also select downgraded versions like _Standard SSD_ or _Standard HDD_ - only downgrade it if you really have a good reason for it.

### Encryption type

How your VM disk data is encrypted.

The default value is the **(Default) Encryption at-rest with platform-managed key**, where Azure manages the encryption for you.
If you have a requirement for you to manage the key (e.g.: a very high secure system like banks, bitcoin miner, etc.), you can choose the _Encryption at-rest with a customer-managed key_ and define the encryption (e.g.: using Azure Data Vault to generate keys).

### Data disks

Additional disks you can add to your VM. It's a separate disk that you can attach/detach from a VM to another.

You can set the disk configuration, such as its size, disk tier, max. IOPS and max throughput.
Note that if you have a minimum IOPS requirement and you're using an downgraded disk (e.g.: Standard HDD) it may be better to upgrade the disk instead of changing it for a bigger one.

## Creating a VM - Network

At the network configuration you can set its virtual network, public IP (optional), virtual inbound rules as well as some other configurations.

![creating-a-virtual-machine-network](https://user-images.githubusercontent.com/13342183/135428887-36d7ec29-6311-40cb-b77e-14b02479efb6.png)

## Creating a VM - Management

At the management tab you can define some management tasks and configurations, such as monitoring (e.g.: boot diagnostics) as well as auto-shutdown and backups.

![creating-a-virtual-machine-management](https://user-images.githubusercontent.com/13342183/135429692-f8b999bb-df92-4d65-8827-503d257edb3d.png)

Note that the **auto-shutdown** is usually a good idea if you don't need the VM to be up 24/7, e.g.: development machines.
You can set a specific interval where the VM is down so you can save some costs.

## PowerShell / Azure CloudShell

Some important commands to know are:
* _**Connect-AzAccount**_: authenticate yourself so you can perform actions on Azure.
* _**Get-AzVM**_: show all current created VMs. CLI version: _az vm list_.
* _**New-AzResourceGroup** -Name <name, e.g.: testgrp> -Location <location, e.g.: EastUS>_: creates a resource group (all VMs must be inside a resource group). CLI version: _az group create_.
* _**New-AzVM** -ResourceGroupName <res.group name, e.g.: testgrp> -Name <VM name, e.g.: testvm> -VirtualNetworkName <VM network name> -OpenPorts <open ports, e.g.: 3389>_: creates a new Azure VM. CLI version: _az vm create_.
* _**Stop-AzVM** -Name <VM name, e.g.: testvm> -ResourceGroupName <resource group name>_: stops a VM. CLI version: _az vm stop_.

---

[<< Previous page](skills-measured.md)
|
[Next page >>](www.google.com)
