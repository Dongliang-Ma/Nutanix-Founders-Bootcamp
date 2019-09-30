.. _prism_central_overview:

-----------------------
Prism Central: 概述
-----------------------

Overview
++++++++

实验将介绍 Prism Central UI, 并熟悉它的布局和导航。

Prism Central
+++++++++++++

#. 打开 https://<*Prism-Central-IP*>:9440

#. 填写以下字段并单击 **Enter**:

- **Username** - admin
- **Password** - *HPOC Password*

#. 在登录Prism Central后, 请自己熟悉Prism UI.

#. 浏览 **Home** 界面的有关信息:

- Cluster Runway
- Cluster Quick Access
- Impacted Cluster | Alerts
- tasks

#. 浏览 **Explore** 界面:

- VMs
- Images
- Clusters
- Hosts
- Disks
- Storage Containers

.. figure:: images/nutanix_tech_overview_10.png

#. 查看其他部分，并快速浏览:

- Planning
- Analysis
- Apps (We will configure this later in the workshop)
- Alerts
- Tasks :fa:`circle-o`
- Search :fa:`search`
- Help :fa:`question`
- Configuration :fa:`cog`
- User :fa:`user`

.......................
Prism Central UI 浏览
.......................

如何找到被一个Prism Central实例管理的所有主机页面?

.. figure:: images/nutanix_tech_overview_11.png

.. note::

  如果这个Prism Central 实例管理了多个集群, 这个页面会显示被管理的所有机器的主机。

#. 在 **Prism Central > Explore**, 点击左手菜单的 **Hosts**.

你如何找到显示所有已部署的VMs页面. 这个页面是否和下图类似?

.. figure:: images/nutanix_tech_overview_12.png

#. 在 **Prism Central > Explore** , 点击左手菜单的 **VMs** .

哪个页面会显示系统中的最新活动？ 在此页面上，您可以监控任何任务的进度，并使用时间戳跟踪过去所执行的操作。 你能想出两种不同的方法吗？

#. 第一种方法, 在 **Prism Central > Home**, 点击 **View All Tasks**. 第二种方法, 点击 :fa:`circle-o`


概要总结
+++++++++

- Prism 是一个全面的UI界面
- 关键信息显示在正中明显位置。
- Prism Central 可以管理多个集群
