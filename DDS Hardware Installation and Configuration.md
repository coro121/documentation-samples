# DDS Hardware Installation and Configuration

## Introduction

The Deployable Defensive System (DDS) is a modular fly-away computing cluster that is purpose-built for conducting Defensive Cyber Operations (DCO) missions. This kit provides a platform with hardware and software for the US Army and their DoD mission partners. 

## Safety Summary

The following are general safety precautions and instructions not related to any specific procedure and, therefore, do not appear elsewhere in this manual. These are recommended precautions and procedures that personnel must understand and apply during many phases of operation and maintenance.

## Installation and Configuration

The DDS Modular (Large) kit provides an ample amount of compute and storage for handling traffic via distributed collection/analysis. Using a total of eight nodes, it is designed to aggregate network traffic in excess of 15 Gbps. It can also be broken into smaller, powerful sensors distributed to separate collection points across one or more networks.

### Connecting to the S4112-T Switch

| Cables Needed for T-Switch Connections                        |
|---------------------------------------------------------------|
| **(9) CAT 6 Cables** (Model: 70596) - Indicated by green line          |
| **(2) QSFP Breakout Cables** (Model: 36768) - Indicated by purple line |

1. Begin by plugging a **CAT6** cable into **Port 1** on the **S4112-T** switch and the other end into the **Operator Switch**

| <img src="https://user-images.githubusercontent.com/10658186/127578645-b51b80ef-0f04-4a2e-876e-c58b745cdf8f.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 1 T Switch_                                                                                                              |_

2. Using a **CAT6** cable, connect **Nodes 1-8 IPMI** into **Ports 5-12** on the **T** switch ***change image**

| <img src="https://user-images.githubusercontent.com/10658186/127578814-1868c6bd-fca8-426c-ba46-293b78a59286.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 2 T Switch_                                                                                                            |

3. Using **QSFP Breakout** cable, connect **Nodes 1-4 eno 7** into **Port 13** on the **T** Switch

| <img src="https://user-images.githubusercontent.com/10658186/127578968-29839bc8-47de-4e08-8000-9ffc9d22d3bc.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 3 T Switch_                                                                                                            |

4. Using a **QSFP Breakout** cable, connect **Nodes 5-8 eno 7** into **Port 14**

| <img src="https://user-images.githubusercontent.com/10658186/127579097-7e3712e2-bc35-43dd-93d7-da5e8e43a990.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 4 T Switch_                                                                                                            |

### Connecting to the S4112-F Switch

| Cable Needed for F-Switch Connections                         |
|---------------------------------------------------------------|
| **(2) QSFP Breakout Cables** (Model: 36768) - Indicated by purple line |
| **(1) SFP DAC Cable** (Model: 40141) - Indicated by blue line               |

1. Using an **SFP DAC** cable, connect the **Thunderbolt** adapter to **Port 1** on the **S4112-F** switch. Take the **USB** cable from the **Thunderbolt** adapter and connect to the **Deployment Laptop**

| <img src="https://user-images.githubusercontent.com/10658186/127579560-67d98322-1d90-4982-b6aa-feb26e06773a.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 1 F Switch_                                                                                                            |

2. Using a **QSFP Breakout** cable, connect **Nodes 5-8 eno 8** into **Port 13**

| <img src="https://user-images.githubusercontent.com/10658186/127582352-096f23d8-8a7f-4eb9-8abb-24c62f2c6048.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 2 F Switch_                                                                                                            |

3. Using a **QSFP Breakout** cable, connect **Nodes 1-4 eno 8** into **Port 14**

| <img src="https://user-images.githubusercontent.com/10658186/127582427-c7384938-81ee-4422-8685-f54d60078ad5.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 3 F Switch_                                                                                                            |

### Connecting Both Switches

| Cable Needed for S4112-T to S4112-F Connection    |
|---------------------------------------------------|
| **(1) 100G Cable** (Model: 50484) - Indicated by red line |

1. Using a **100G** cable, connect the **S4112-T** switch, **Port 15** to the **S4112-F** switch, **Port 15**

| <img src="https://user-images.githubusercontent.com/10658186/127582862-237d41c2-5cfa-419c-aa88-82eeca7378b3.png" width="700px"> |
|---------------------------------------------------------------------------------------------------------------------------|
| _Step 1 Both Switches_                                                                                                    |

### Connecting to the Gigamon Network Tap

| Cable Needed for Gigamon Connection             |
|-------------------------------------------------|
| **(2) SFP DAC Cables** (Model: 40141) - Indicated by blue line |

1. Using an **SFP Dac** cable, connect **Node 7 eno 8** to the **Gigamon Tap Port A Monitor/Tool**. Connect **Node 7 eno 7** to **Port B-Monitor/Tool**

| <img src="https://user-images.githubusercontent.com/10658186/127582544-29d04937-a169-4fea-8553-2e1bb620d04a.png" width="700px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Gigamon Tap_                                                                                                                |

### System Power Up Sequence

To power up the system, begin by cabling to switches and nodes first. Next, power on the switches, followed by the laptop. Lastly, power on servers.
Again, this document assumes that the NERD CURE software is deployed and providing a hyperconverged RHEV platform.

1. Power on all nodes and wait approximately 5 minutes for them to power up.

2. SSH into one of the nodes, enter the gluster cli, and start each volume.

✎ **Note**: The volume list will show which volumes are available

  ```python
  [root@node1 ~]# gluster
  gluster> volume start <volume name>
  ```

3. SSH into each node, in separate tabs, and start the **`ovirt-ha-broker`** and the **`ovirt-ha-agent`**

   ```python
    systemctl start ovirt-ha-broker; systemctl start ovirt-ha-agent
   ```

4. Exit maintenance mode of the hosted engine

    ```python
    hosted-engine --set-maintenance --mode=none
    ```

5. The hosted engine VM can take some time to boot up. You can verify the process by entering:

    ```python
    hosted-engine --vm-status
    ```

6. With a web browser, log onto the hosted engine by entering **`[kit number]cpb.mil.`** Turn from the GUI and activate all storage domains by going to **`Storage > Domains`** and activating all storage domains

7. Start all virtual machines by selecting **`Computer > Virtual Machines`**, choosing each one, and selecting **Run**. Wait until all nodes are turned on.

### Power Down Sequence

The power down sequence is dependent upon what operating system and platform is deployed on the DDS-M kit. For purpose of this document it is assumed that the NERD CURE software is deployed and providing a hyperconverged RHEV platform. Proper startup/shutdown procedures should always be followed to prevent the loss of data.

1. With a web browser, go to hosted engine and select **Administration Portal**. Turn off all virtual machines by selecting **`Compute > Virtual Machines`**, choosing each one and selecting **Shutdown**. Wait until they are all turned off.

2. Move the storage domains to maintenance mode except for the **`hosted_storage`** domain

3. SSH into one of the nodes and run the following commands to enter into maintenance mode and shutdown the hosted engine.

   ```python
   hosted-engine --set-maintenance --mode=global
   hosted-engine --vm-shutdown
   ```

4. Confirm the hosted engine is shut down by entering the following

    ```python
    hosted-engine --vm-status
    ```

5. Logon to all nodes with SSH (it is best to do this in multiple tabs with each tab being a separate VM). Proceed with shutting down services:

    ```python
    systemctl top ovirt-ha-broker;systemctl stop ovirt-ha-agent
    ```

6. On each node, disconnect storage using the following command:

    ```python
    hosted-engine --disconnect-storage
    ```

7. From one of the nodes, open the gluster cli

    ```python
    gluster
    ```

8. Get the names of the volumes with the following command:

    ```python
   [root@node1 ~]# gluster
   gluster> volume list
   data
   engine
   vmstore
   ```

9. Stop each volume with the command

    ```python
    volume stop <volume name>
    ```

10. Turn off each system

