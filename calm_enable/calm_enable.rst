.. _calm_enable：

------------
Calm：Enable
------------

概述
++++++++

.. note:: 回顾：参考：`calm_basics`，然后继续实验，熟悉Nutanix Calm中使用的UI和常用术语。

预计完成时间：** 10分钟**

在本练习中，您将启用Nutanix Calm。

启用应用程序管理
+++++++++++++++++++++++

Calm内置于Prism Central，无需额外的设备或控制台即可管理。在使用Calm开始在您的环境中管理应用程序之前，必须启用该服务。

.. note:: 每个Prism Central实例只能启用一次Calm。如果**启用应用程序管理**旁边显示绿色复选标记，则表示已为正在使用的Prism Central实例启用了Calm。继续：ref：`calm_projects`。

#. 在 **Prism Central** 中, 点击 **?** 的下拉菜单, 在Prism Central中展开 **New** 并且选择 **Enable app management**.

#. 单击 **Enable**。

.. figure :: images / 510enable1.png

#. 选择 **Enable App Management**，然后单击 **Save**。

.. note:: Nutanix Calm是一个单独许可的产品，可以与Acropolis Starter，Pro或Ultimate版本一起使用。在需要额外许可之前，每个Prism Central实例可以免费管理多达25个VM。

.. figure :: images / 510enable2.png

#. 你应该得到Calm正在启用的确认，这需要5到10分钟。

.. figure :: images / 510enable3.png

添加Active Directory
+++++++++++++++++++++++

当我们等待Calm启用时，我们将添加一个Active Directory服务器。虽然Calmr基本使用不需要这样做，但是需要进行任何基于角色的访问控制，因此最好进行设置。

#.  单击** 齿轮图标 **然后** Authentication **。

.. figure :: images / 510enable4.png

#. 在弹出窗口中，单击 **New Directory**。

.. figure :: images / 510enable5.png

#. 填写以下字段，然后单击 **Save**：

- **Directory Type** - Active Directory
- **Name** - NTNXLAB
- **Domain** - ntnxlab.local
- **Directory URL** - ldaps://*<DC-VM-IP>*
- **Search Type** - Non Recursive
- **Username** - Administrator@ntnxlab.local
- **Password** - nutanix/4u

.. figure :: images / 510enable6.png

#. 刷新浏览器并从导航栏中选择 ** Calm **。如果Calm仍在启用，请等待一分钟，然后重试。

.. figure:: images/510enable7.png
