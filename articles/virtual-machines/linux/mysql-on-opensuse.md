---
title: Install MySQL on an OpenSUSE VM in Azure | Microsoft Docs
description: Learn to install MySQL on an OpenSUSE Linux VMirtual machine in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager

ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: cynthn

---
# Install MySQL on a virtual machine running OpenSUSE Linux in Azure

[MySQL](http://www.mysql.com) is a popular, open-source SQL database. This tutorial shows you how to create a virtual machine running OpenSUSE Linux, then install MySQL.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

If you choose to install and use the CLI locally, you need Azure CLI version 2.0 or later. To find the version, run `az --version`. If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).

## Create a virtual machine running OpenSUSE Linux

First, create a resource group. In this example, we are naming the resource group *mySQSUSEResourceGroup* and creating it in the *East US* region.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

Create the VM. In this example, we are naming the VM *myVM*. We are also going to use a VM size *Standard_D2s_v3*, but you should choose the [VM size](sizes.md) you think is most appropriate for your workload.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

You also need to add a rule to the network security group to allow traffic over port 3306 for MySQL.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## Connect to the VM

You'll use SSH to connect to the VM. In this example, the public IP address of the VM is *10.111.112.113*. You can see the IP address in the output when you created the VM.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## Update the VM
 
After you're connected to the VM, install system updates and patches. 
   
```bash
sudo zypper update
```

Follow the prompts to update your VM.

## Install MySQL 


Install the MySQL in the VM over SSH. Reply to prompts as appropriate.

```bash
sudo zypper install mysql
```
 
Set MySQL to start when the system boots. 

```bash
sudo systemctl enable mysql
```
Verify that MySQL is enabled.

```bash
systemctl is-enabled mysql
```

This should return: enabled.


## MySQL password

After installation, the MySQL root password is empty by default. Run the **mysql\_secure\_installation** script to secure MySQL. The script prompts you to change the MySQL root password, remove anonymous user accounts, disable remote root logins, remove test databases, and reload the privileges table. 


```bash
mysql_secure_installation
```

## Log in to MySQL

You can now log in and enter the MySQL prompt.

```bash  
mysql -u root -p
```
This switches you to the MySQL prompt where you can issue SQL statements to interact with the database.

Now, create a new MySQL user.

```   
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
The semi-colon (;) at the end of the line is crucial for ending the command.


## Create a database


Create a database and grant the `mysqluser` user permissions.

```   
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Database user names and passwords are only used by scripts connecting to the database.  Database user account names do not necessarily represent actual user accounts on the system.

Enable log in from another computer. In this example, the IP address of the computer that we want to log in from is *10.112.113.114*.

```   
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
To exit the MySQL database administration utility, type:

```    
quit
```


## Next steps
For details about MySQL, see the [MySQL Documentation](http://dev.mysql.com/doc/index-topic.html).




