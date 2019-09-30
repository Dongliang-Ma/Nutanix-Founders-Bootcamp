.. _calm_projects:

------------
Calm: Projects
------------

简介
++++++++

.. note::

  在继续练习之前阅读 :ref:`calm_basics` ，您需要熟悉Nutanix Calm中使用的UI和常用术语。

  预计完成时间：**15 MINUTES**

在本练习中，您将配置一个包含在整个Bootcamp中创建的蓝图和应用程序Project。

生成一个Project
++++++++++++++++++

Projects 是将Calm与Nutanix的内置自助服务门户（SSP）功能集成的逻辑结构，允许管理员将基础结构资源以及Active Directory用户/组的角色/权限分配给特定的蓝图和应用程序。
#. 在Calm UI界面内, 从边栏选择|proj-icon| **Projects**。

   .. figure:: images/510projects1.png

#. 点击 **+ Create Project**

#. 填写以下字段：

   - **Project Name** - *initials*-Calm
   - **Description** - *initials*-Calm

#. 在 **Users, Groups, and Roles**下面, 点击 **+ User**.

#. 填写以下字段，然后单击 **Save**:

   - **Name** - SSP Admins
   - **Role** - Project Admin

#. 单击 **+ User**, 填写以下字段，然后单击 **Save**:

   - **Name** - SSP Developers
   - **Role** - Developer

#. 单击 **+ User**, 填写以下字段，然后单击 **Save**:

   - **Name** - SSP Power Users
   - **Role** - Consumer

#. 单击 **+ User**, 填写以下字段，然后单击 **Save**:

   - **Name** - SSP Basic Users
   - **Role** - Operator

   .. figure:: images/projects_name_users.png

#. 在 **Infrastructure**下, 单击蓝色的 **Select Provider** 按钮, 之后是 **Nutanix**.

#. 在出现的框中，单击白色 **Select Clusters & Subnets** 按钮, 并在弹出窗口中，选择您的AHV群集。 选择集群后，选择 **Primary** 网络，如果有的话，再选择 **Secondary**网络，然后单击 **Confirm**.

   .. figure:: images/projects_cluster_subnet_selection.png

#. 在 **Selected Subnets** 表内, 为 **Primary**网络选择 :fa:`star`  使其成为 **Calm** project里虚拟机中的默认虚拟网络 

   .. figure:: images/projects_infrastructure.png

#. 单击 **Save**.

.. note::

  单击 `here <https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v56:nuc-roles-responsibility-matrix-c.html>`_ 查看默认SSP角色和相关权限的完整列表。

概要总结
+++++++++

-Nutanix Calm是Nutanix堆栈的完全集成组件。 在Scale Out Prism Central部署中易于启用，高度可用，并且利用无中断的一键式升级来获得新功能和修复。

-通过使用分配给不同群集和用户的不同项目，管理员可以确保每次都以正确的方式部署工作负载。 例如，开发人员可以是开发/测试项目的项目管理员，因此他们可以完全控制部署到其开发集群或云中，同时具有对生产项目的只读访问权限，从而使他们可以访问日志，但无权 改变生产工作量。

.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
