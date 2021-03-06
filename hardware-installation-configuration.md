# Deployable defensive system hardware installation and configuration

## Introduction

The Deployable Defensive System (DDS) is a modular fly-away computing cluster that is purpose-built for conducting Defensive Cyber Operations (DCO) missions. This kit provides a platform with hardware and software for the United States Army and its DoD mission partners.

## Installation and configuration

The DDS Modular (Large) kit provides an ample amount of computing and storage for handling traffic via distributed collection and analysis. Using a total of eight nodes, it's designed to aggregate network traffic above 15 Gbps. It can also be broken into smaller, powerful sensors distributed to separate collection points across one or more networks.

### Connect to the S4112-T switch

| Cables Needed for T-Switch Connections                        |
|---------------------------------------------------------------|
| **(9) CAT 6 Cables** (Model: 70596) - Indicated by the green line          |
| **(2) QSFP Breakout Cables** (Model: 36768) - Indicated by the purple line |

1. Begin by plugging a **CAT6** cable into **Port 1** on the **S4112-T** switch and the other end of the cable into the **Operator Switch**.

| <img src="https://user-images.githubusercontent.com/10658186/127578645-b51b80ef-0f04-4a2e-876e-c58b745cdf8f.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 1 T Switch_                                                                                                              |_

2. Next, use 8 additional **CAT6** cables to connect **Nodes 1-8 IPMI** into **Ports 5-12** on the **T** switch.

| <img src="https://user-images.githubusercontent.com/10658186/127578814-1868c6bd-fca8-426c-ba46-293b78a59286.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 2 T Switch_                                                                                                            |

3. Use the **QSFP Breakout** cable to connect **Nodes 1-4 eno 7** into **Port 13** on the **T** Switch.

| <img src="https://user-images.githubusercontent.com/10658186/127578968-29839bc8-47de-4e08-8000-9ffc9d22d3bc.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 3 T Switch_                                                                                                            |

4. Using the other **QSFP Breakout** cable, connect **Nodes 5-8 eno 7** into **Port 14**.

| <img src="https://user-images.githubusercontent.com/10658186/127579097-7e3712e2-bc35-43dd-93d7-da5e8e43a990.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 4 T Switch_                                                                                                            |

### Connect to the S4112-F switch

| Cable Needed for F-Switch Connections                         |
|---------------------------------------------------------------|
| **(2) QSFP Breakout Cables** (Model: 36768) - Indicated by the purple line |
| **(1) SFP DAC Cable** (Model: 40141) - Indicated by the blue line               |

1. Use the **SFP DAC** cable to connect the **Thunderbolt** adapter to **Port 1** on the **S4112-F** switch. Take the **USB** cable from the **Thunderbolt** adapter and connect to the **Deployment Laptop**.

| <img src="https://user-images.githubusercontent.com/10658186/127579560-67d98322-1d90-4982-b6aa-feb26e06773a.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 1 F Switch_                                                                                                            |

2. Use a **QSFP Breakout** cable to connect **Nodes 5-8 eno 8** into **Port 13**.

| <img src="https://user-images.githubusercontent.com/10658186/127582352-096f23d8-8a7f-4eb9-8abb-24c62f2c6048.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 2 F Switch_                                                                                                            |

3. Take the second **QSFP Breakout** cable and connect **Nodes 1-4 eno 8** into **Port 14**.

| <img src="https://user-images.githubusercontent.com/10658186/127582427-c7384938-81ee-4422-8685-f54d60078ad5.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Step 3 F Switch_                                                                                                            |

### Connect both switches

| Cable Needed for S4112-T to S4112-F Connection    |
|---------------------------------------------------|
| **(1) 100G Cable** (Model: 50484) - Indicated by the red line |

1. Use the **100G** cable to connect the **S4112-T** switch, **Port 15**, to the **S4112-F** switch, **Port 15**.

| <img src="https://user-images.githubusercontent.com/10658186/127582862-237d41c2-5cfa-419c-aa88-82eeca7378b3.png" width="300px"> |
|---------------------------------------------------------------------------------------------------------------------------|
| _Step 1 Both Switches_                                                                                                    |

### Connect to the Gigamon Network Tap

| Cable Needed for Gigamon Connection             |
|-------------------------------------------------|
| **(2) SFP DAC Cables** (Model: 40141) - Indicated by blue line |

1. Using **SFP DAC** cables, connect **Node 7 eno 8** to the **Gigamon Tap Port A Monitor/Tool**.

2. Connect **Node 7 eno 7** to **Port B-Monitor/Tool**.

| <img src="https://user-images.githubusercontent.com/10658186/127582544-29d04937-a169-4fea-8553-2e1bb620d04a.png" width="500px"> |
|------------------------------------------------------------------------------------------------------------------------------|
| _Gigamon Tap_                                                                                                                |

### System power-up sequence

To power up the system sequence, follow the Installation and configuration procedure, described [earlier](https://github.com/coro121/documentation-samples/blob/2fcfe89b480bc67d19d55f9fbdb15f2d29221919/hardware-installation-configuration.md#installation-and-configuration). Next, turn on the switches, the laptop, and lastly, the servers.

1. Start all nodes and wait about 5 minutes to complete loading.

2. SSH into a node, enter the `gluster` command-line tool and open each volume.

> **Tip**: The volume list will show which volumes are available.

  ```python
  [root@node1 ~]# gluster
  gluster> volume start <volume name>
  ```

3. SSH into each node, in separate tabs, and use the **`ovirt-ha-broker`** and **`ovirt-ha-agent`** commands to enter maintenance mode.

```python
systemctl start ovirt-ha-broker; systemctl start ovirt-ha-agent
```

4. Exit the maintenance mode of the hosted engine using the `--set-maintenance` command.

```python
hosted-engine --set-maintenance --mode=none
```

The hosted engine virtual machine can take some time to start. You can verify the process by typing

```python
hosted-engine --vm-status
```

5. Using a web browser, log in to the hosted engine by entering **`[kit number]cpb.mil.`**

6. From the UI, activate all storage domains by selecting **`Storage > Domains`**.

7. Start all virtual machines by selecting **`Computer > Virtual Machines`**.

8. Last, open each virtual machine and then select **Run**. Wait until all the nodes are turned on.

### System power-down sequence

The power-down sequence is dependent on what operating system and platform are deployed on the DDS-M kit. Proper power-up and power-down procedures should always be followed to prevent the loss of data.

1. From a web browser, go to the hosted engine and select **Administration Portal**.

2. Turn all virtual machines off by selecting **`Compute > Virtual Machines`**. Open each virtual machine and select **Shutdown**.

> **Tip:** Wait until all virtual machines are turned off before proceeding to the next step.

3. Change the storage domains to maintenance mode, except for the **`hosted_storage`** domain.

4. Use SSH to login to a node, enter the next commands to open maintenance mode, and turn off the hosted engine.

```python
hosted-engine --set-maintenance --mode=global
hosted-engine --vm-shutdown
```

5. Confirm the hosted engine is shut down by entering the `--vm-status` command.

```python
hosted-engine --vm-status
```

6. Login to all nodes using SSH.

> **Tip:** It's best to log in using separate tabs for each virtual machine.

7. Shut down all services using the `systemctl` command.

```python
systemctl stop ovirt-ha-broker;systemctl stop ovirt-ha-agent
```

8. On each node, disconnect storage by typing the `disconnect-storage` command.

```python
hosted-engine --disconnect-storage
```

9. From one of the nodes, open the `gluster` command-line tool.

```python
gluster
```

10. View the names of the volumes by typing the `gluster > volume list` command.

```python
[root@node1 ~]# gluster
gluster > volume list
data
engine
vmstore
```

11. Stop each volume using the `volume stop` command.

```python
volume stop <volume name>
```

12. Turn off each system.
