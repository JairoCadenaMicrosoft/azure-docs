---
<<<<<<< HEAD
title: Mount Azure File Storage on Linux VMs using SMB with Azure CLI 1.0 | Microsoft Docs
description: How to mount Azure File Storage on Linux VMs using SMB.
=======
title: Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0 | Microsoft Docs
description: How to mount Azure File storage on Linux VMs by using SMB
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: ''

ms.assetid:
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/07/2016
ms.author: v-livech

---

<<<<<<< HEAD
# Mount Azure File Storage on Linux VMs using SMB using the Azure CLI 1.0

This article shows how to utilize the Azure File Storage service on a Linux VM using an SMB mount.  Azure File storage offers file shares in the cloud using the standard SMB protocol.  The requirements are:

- [an Azure account](https://azure.microsoft.com/pricing/free-trial/)

- [SSH public and private key files](virtual-machines-linux-mac-create-ssh-keys.md)


## CLI versions to complete the task
You can complete the task using one of the following CLI versions:

- [Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)
- [Azure CLI 2.0 (Preview)](virtual-machines-linux-mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)- our next generation CLI for the resource management deployment model


## Quick Commands

If you need to quickly accomplish the task, the following section details the commands needed. More detailed information and context for each step can be found the rest of the document, [starting here](virtual-machines-linux-mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough).

Prerequisites: Resource Group, VNet, NSG with SSH inbound, Subnet, Azure Storage Account, Azure Storage Account keys, Azure File Storage share, and a Linux VM. Replace any examples with your own settings.

Create a directory for the local mount
=======
# Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0

This article shows how to mount Azure File storage on a Linux VM by using the Server Message Block (SMB) protocol. File storage offers file shares in the cloud via the standard SMB protocol. The requirements are:

* An [Azure account](https://azure.microsoft.com/pricing/free-trial/)
* [Secure Shell (SSH) public and private key files](virtual-machines-linux-mac-create-ssh-keys.md)

## CLI versions to use
You can complete the task by using one of the following command-line interface (CLI) versions:

* Azure CLI 1.0: The CLI for the classic and Azure Resource Manager deployment models. Learn about it in the "Quick commands" section.
* [Azure CLI 2.0 Preview](virtual-machines-linux-mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): The next-generation CLI for the Resource Manager deployment model.


## Quick commands

To accomplish the task quickly, follow the steps in this section. For more detailed information and context, begin at the ["Detailed walkthrough"](virtual-machines-linux-mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) section.

### Prerequisites
* A resource group
* An Azure virtual network
* A network security group with an SSH inbound
* A subnet
* An Azure storage account
* Azure storage account keys
* An Azure File storage share
* A Linux VM

Replace any examples with your own settings.

### Create a directory for the local mount
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a

```bash
mkdir -p /mnt/mymountpoint
```

<<<<<<< HEAD
Mount the Azure File Storage SMB share to the mountpoint
=======
### Mount the File storage SMB share to the mount point
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

<<<<<<< HEAD
To persist the mount after a reboot, add a line to the `/etc/fstab`
=======
### Persist the mount after a reboot
Add the following line to `/etc/fstab`:
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## Detailed walkthrough

<<<<<<< HEAD
Azure File storage offers file shares in the cloud using the standard SMB protocol.  And with the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.  Using an SMB mount on Linux allows for easy backups to a robust, permanent archiving storage location that is supported by an SLA.  

Moving files from a VM to an SMB mount hosted on Azure File Storage is a great way to debug logs as that same SMB share can be locally mounted to your Mac, Linux, or Windows workstation.  SMB would not be the best solution to stream Linux or application logs in real time as the SMB protocol is not built for logging duties as heavy as that.  A dedicated unified logging layer tool like Fluentd would be a better choice over SMB to collect Linux and application logging output.

For this detailed walkthrough, we create the prerequisites needed to first create the Azure File Storage share, and then mount it via SMB on a Linux VM.

## Create the Azure Storage account

```azurecli
azure storage account create myStorageAccount \
--sku-name lrs \
--kind storage \
-l westus \
-g myResourceGroup
```

## Show the Storage account keys

The Azure Storage Account keys are created in pairs, when the storage account is created.  The storage account keys are created in pairs so that the keys can be rotated without any service interruption.  Once you rotate keys to the second key in the pair, you create a new key pair.  New storage account keys are always created in pairs, ensuring you always have at least one unused storage key ready to rotate to.

```azurecli
azure storage account keys list myStorageAccount \
--resource-group myResourceGroup
```

## Create the Azure File Storage Share

Create the File Storage share, which contains the SMB share.  The quota is always in GigaBytes (GBs).

```azurecli
azure storage share create mystorageshare \
--quota 10 \
--account-name myStorageAccount \
--account-key nPOgPR<--snip-->4Q==
```

## Create the mount point directory

A local directory is needed on the Linux filesystem to mount the SMB share to.  Anything written or read from the local mount directory is forwarded to the SMB share hosted on Azure File Storage.

```bash
sudo mkdir -p /mnt/mymountdirectory
```

## Mount the SMB share

```azurecli
sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
```

## Persist the SMB mount through reboots

Once you reboot the Linux VM, the mounted SMB share is unmounted during shutdown.  To remount the SMB share on boot, you must add a line to the Linux `/etc/fstab`.  Linux uses the `fstab` file to list what filesystems it needs to mount during bootup.  Adding the SMB share ensures that the Azure File Storage share is a permanently mounted filesystem for the Linux VM.  Adding the Azure File Storage SMB share to a new VM is possible using `cloud-init`.

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## Next Steps

- [Using cloud-init to customize a Linux VM during creation](virtual-machines-linux-using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Add a disk to a Linux VM](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Encrypt disks on a Linux VM using the Azure CLI](virtual-machines-linux-encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
=======
File storage offers file shares in the cloud that use the standard SMB protocol. With the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0. When you use an SMB mount on Linux, you get easy backups to a robust, permanent archiving storage location that is supported by an SLA.

Moving files from a VM to an SMB mount that's hosted on File storage is a great way to debug logs. That's because the same SMB share can be mounted locally to your Mac, Linux, or Windows workstation. SMB isn't the best solution for streaming Linux or application logs in real time, because the SMB protocol is not built to handle such heavy logging duties. A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.

For this detailed walkthrough, we create the prerequisites needed to first create the File storage share, and then mount it via SMB on a Linux VM.

1. Create an Azure storage account by using the following code:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. Show the storage account keys.

    When you create a storage account, the account keys are created in pairs so that they can be rotated without any service interruption. When you switch to the second key in the pair, you create a new key pair. New storage account keys are always created in pairs, ensuring that you always have at least one unused storage key ready to switch to. To show the storage account keys, use the following code:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. Create the File storage share.

    The File storage share contains the SMB share. The quota is always expressed in gigabytes (GB). To create the File storage share, use the following code:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Create the mount-point directory.

    You must create a local directory in the Linux file system to mount the SMB share to. Anything written or read from the local mount directory is forwarded to the SMB share that's hosted on File storage. To create the directory, use the following code:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Mount the SMB share by using the following code:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. Persist the SMB mount through reboots.

    When you reboot the Linux VM, the mounted SMB share is unmounted during shutdown. To remount the SMB share on boot, you must add a line to the Linux /etc/fstab. Linux uses the fstab file to list the file systems that it needs to mount during the boot process. Adding the SMB share ensures that the File storage share is a permanently mounted file system for the Linux VM. Adding the File storage SMB share to a new VM is possible when you use cloud-init.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## Next steps

- [Using cloud-init to customize a Linux VM during creation](virtual-machines-linux-using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Add a disk to a Linux VM](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Encrypt disks on a Linux VM by using the Azure CLI](virtual-machines-linux-encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>>>>>>> b7b2f61289b9aedbeb062daac265526d1c43dd4a
