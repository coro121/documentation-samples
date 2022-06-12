# DDS operating system administration

The Deployable Defensive System (DDS) is a modular fly-away computing cluster that is purpose-built for conducting Defensive Cyber Operations (DCO) missions. This kit provides a platform with hardware and software for the United States Army and its DoD mission partners.

## Operating system administration

System administration is based upon Red Hat Enterprise Linux (RHEL), a lightweight, enterprise-supported Linux Operating System (OS) developed by Red Hat. This OS gives users the flexibility of the Linux environment while  maintaining a level of support, security, and stability for production purposes.

### Common administration commands

When using RHEL, there are many commands, concepts, and tools to familiarize yourself with as operators. These commands are common across RHEL and RHVH Operating Systems. However, other distributions of Linux may vary. Below, is a cheat sheet of commands related to common administration tasks operators may need to perform.

#### Common command-line commands

| Command                            | Use                                                                                               |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| `ssh [ip or hostname]`             | Secure shell, an encrypted network protocol allowing for remote login and command execution.   |
| `sudo [command] Sudo -s`           | Run a command with root permissions. Switch to root user mode                                  |
| `pwd`                              | Print working directory                                                                        |
| `whoami`                           | Displays the logged in user id                                                                 |
| `cd /cd [target]`                  | Change the directory to "target" directory                                                     |
| `cd /cure/ansible_main`            | Change the directory to the root of the filesystem                                             |
| `Ls`                               | View list of files in directory                                                                |
| `Clear`                            | Clears the terminal screen                                                                     |
| `cat [filename] cat passwords.yml` | Displays the contents of filename to standard out                                              |
| `cp [source_file] [target file]`   | Copies a specific file to a target location                                                    |
| `mkdir my_directory`               | Create a directory "my_directory"                                                              |
| `rm mymistake.txt`                 | Removes a file 'mymistake.txt'                                                                 |
| `mv [source_file] [target_file]`   | Move a file or directory                                                                       |
| `ip a ip -4 a`                     | View network interface information. View only network interfaces with IPV4 address               |
| `ifup [nic] ifup p1p1 ifdown p1p1` | Turn on specified network interface card, Turn on Sonnet Thunderbolt, Turn off Sonnet Thunderbolt|
| `find . -name [file]`              | Find file or directory by name                                                                   |
| `subscription-manager [command]`   | Red Hat Subscription Manager use `-help` for list of commands                                      |
| `systemctl start name.service`     | Start a service                                                                                  |
| `systemctl stop name.service`      | Stop a service                                                                                   |
| `systemctl enable name.service`    | Enable service (persists reboots)                                                                |
| `systemctl disable name.service`   | Disable service (persists reboots)                                                               |
| `systemctl shutdown`               | System shutdown                                                                                  |
| `systemctl restart`                | System restart                                                                                   |

### User management

User and password management is critical for maintaining kit operations and access to all infrastructure, services, and tools. Passwords for all infrastructure and services deployed by automation are stored in **/opt/test/ansible_main/passwords.yml**. The majority of users should be managed within **IdM** to enable authentication across the cluster. Operators should not change local user passwords or app passwords unless necessary; they are randomly generated upon installation. However, the loss of a root password requires a specific set of tasks to recover.

#### Reset root password

If the root password is known, log in as root and use the **`passwd`** command to create a new root password. However if the root password is lost, create a new one by taking the following steps:

1. Restart or turn on the system and enter **`‘e’`** to edit the installed OS boot-loader, (the first boot option), when the boot menu displays.

| <img src="https://user-images.githubusercontent.com/10658186/127578182-79978437-8b98-4700-9e76-fa4fca0710ac.png" alt="RHEL Boot Selector" width="450px"> |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _RHEL Boot Selector_                                                                                                                                                                 |

2. Towards the bottom of the screen, locate the grub options (indicated by the **`'linux16 /vmlinuz-\<kernel>'`** precursor) and add the **`rd.break`** command to the end of the line. This enables edits to the initial ramdisk (**`initrd`**) environment.

| <img src="https://user-images.githubusercontent.com/10658186/127577907-35fa3481-7b68-47fb-a80f-328c22ca6cae.png" width="450px"> |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _RHEL Edit Boot Options_                                                                                                                                                                   |

3. Press **Ctrl-x** to start with the option you added, allowing access to a root shell within **`initramfs`**.

4. After the shell has been accessed, remount the **`/sysroot`** partition with read/write permissions. (It is currently mounted with read-only permissions):

```python
switch_root:/# mount -o remount,rw /sysroot
```

5. With the partition remounted, change the root directory of the process to **`/sysconfig`** with the chroot command:

```python
switch_root:/# chroot /sysroot
```

6. Reset the root user password.

**Warning**
 Do NOT restart! The next step is vital to maintaining a working system.

7. By default, SELinux is running and will need to be updated after the password has been created. To fix the **`/etc/shadow`** file, enter the following command:

```python
sh-4.2# touch /.autorelabel
```

8. Finally, exit the `chroot` and `initram` shells. Log in with the new root password after the host boots into RHEL.

```python
sh-4.2# exit
exit
switch_root:/# exit
logout
```

### Network management

Networking is a vital part of the RHEL 7.7 OS and can be configured many ways. The most common administrative tasks are described below.

#### Viewing IP address information

1. To view current network configuration, for all interfaces, use the following command:

```python
[user@host ~]# ip addr
```

2. To find information regarding a specific interface, type the interface name after the command, example:

```python
[user@host ~]# ip addr eth0
```

#### Configuring network interfaces

In RHEL, network address configuration is stored within the following directory:

```python
/etc/sysconfig/network-scripts/
```

Each interface should have its own **`ifcfg-<interface>`** file that stores all relevant configuration persistent across reboots. After editing the file, restart the network for the changes to take effect. Below is an example file for an interface named **`eth0`** with a static external IP address of **`10.1.51.50`**:

```python
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=10.1.51.50
NETMASK=255.255.255.0
GATEWAY=10.1.51.1
DNS1=10.1.51.10
```

✎ **Note**
The **`ONBOOT=yes`** command must be included if the interface should be activated after a restart or network restart. If set to **`‘no’`**, the interface will not automatically show!
