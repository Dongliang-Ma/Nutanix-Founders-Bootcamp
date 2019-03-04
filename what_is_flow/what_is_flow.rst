.. _what_is_flow:

------------
What Is Flow
------------

Overview
++++++++

Flow provides security using microsegmentation for VMs on AHV, combining centralized management (Prism Central) with distributed enforcement (Per Node), application centric policy management using Categories (logical grouping), rich visualization, and automation (APIs).

What is the State of Things Today
+++++++++++++++++++++++++++++++++

Why Microsegmentation?
......................

.. figure:: images/what_is_flow_01.png

Securing HCI with Flow + Microsegmentation
..........................................

.. figure:: images/what_is_flow_02.png

Flow: Under the hood
....................

- No Change to Physical Infrastructure
- Nohting to install. Flow comes with Native AHV and OVS
- Group VMs based on Application Topology (independent of Physical Infrastructure, VLANs, and Subnets)
- Define Policies for Application tiers such as Web, App, and DB
- Policies automatically applied to all existing and new VMs in the group
- Rules are pushed (distributed) for local enforcement on each node
- Centrally Manage, Operate, Govern

.. figure:: images/what_is_flow_03.png

Why Flow?
.........

.. figure:: images/what_is_flow_04.png

Use Case: App Segmentation
..........................

.. figure:: images/what_is_flow_05.png

Use Case: VDI Microsegmentation
...............................

.. figure:: images/what_is_flow_06.png

Use Case: Network & Security Automation
.......................................

.. figure:: images/what_is_flow_07.png

Isolation Policy
................

- Isolation - Isolation policies restrict two defined groups of VMs from communicating with each other

.. figure:: images/what_is_flow_08.png

Application Security Policy
...........................

- Application - Flexible security policy, defining both inbound traffic sources and outbound destinations for a single or multi-tiered application.

.. figure:: images/what_is_flow_09.png

Quarantine Policy
.................

- Quarantine - Programmatic or manual restriction of network connections

.. figure:: images/what_is_flow_10.png
