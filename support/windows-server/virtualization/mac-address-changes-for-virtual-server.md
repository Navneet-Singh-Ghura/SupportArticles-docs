---
title: Fails to connect to virtual server after failover
description: Helps solve an issue where users in a different subnet are unable to connect to the virtual server after a failover from one cluster node to another.
ms.date: 09/08/2020
author: Deland-Han
ms.author: delhan
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika
ms.prod-support-area-path: High availability virtual machines
ms.technology: HyperV
---
# MAC Address Changes for Virtual Server During a Failover with Clustering

This article provides a solution to an issue where users in a different subnet are unable to connect to the virtual server after a failover from one cluster node to another.

_Original product version:_ &nbsp; Windows Server 2012 R2  
_Original KB number:_ &nbsp; 244331

## Symptoms

After a failover from one cluster node to another, users in a different subnet may be unable to connect to the virtual server.

## Cause

Server Clusters in Windows 2003 Enterprise Server or Datacenter Server, Failover Clustering in Windows Server 2008 Enterprise or Datacenter and Failover Clustering in Windows Server 2008 R2 Enterprise or Datacenter performs a gratuitous Address Resolution Protocol (ARP) request when a failover occurs. However, some devices (such as switches) may not forward the gratuitous ARP request to other devices. This causes devices on the other side of the switch or router to have the incorrect MAC address for the virtual server that has failed over. Often, this situation corrects itself after a router or switch sees the failure and updates its ARP cache by performing a broadcast. Most routers and switches are configured not to forward ARP traffic between subnets to prevent ARP storms from occurring.

## Resolution

Gratuitous ARP requests must be forwarded across networks so that all devices receive the updated MAC-to-IP address mappings. Contact your hardware manufacturer for information about how to change your switch or router's configuration so that gratuitous ARP requests are passed to all networks.

## More information

In a cluster, each computer (or cluster node) has a network adapter attached to the corporate network, and each cluster node has its own IP address, network name (NetBIOS name), and MAC address. The virtual server has an IP address and network name, but uses the MAC address of the cluster node that is the current owner of the virtual server resources.

[169017](https://support.microsoft.com/help/169017) Information on Groups and Resources Using Microsoft Cluster Server  
When a failover occurs, the cluster server of the node that is receiving IP resources sends a gratuitous ARP request so that all devices (computers, routers, and switches) are updated, and a new MAC address is assigned to an existing IP address. If a switch or router does not pass the updated MAC-to-IP address mappings, other network devices contain the old MAC address for the cluster node that is down.

For additional information, click the article numbers below to view the articles in the Microsoft Knowledge Base:

[199773](https://support.microsoft.com/help/199773) Behavior of Gratuitous ARP in Windows NT 4.0  
 [168567](https://support.microsoft.com/help/168567) Clustering Information on IP Address Failover