.. _flow_enable:

-------------
Flow: Enable
-------------

*完成本实验需要大约10分钟。*

概述
++++++++

本练习中，你需要启用Flow.

启用 Flow
++++++++++++++++++++++++++

Flow内置于Prism Central中，不需要额外的应用或控制台来管理。在开始用Flow保护你的环境之前，必须先启用该服务。

.. note::

  每个Prism Central实例，Flow只能启用一次。 如果 **Microsegmentation** 旁边有显示绿色复选框，这意味着对于该Prism Central的Flow服务已经被启用。前往:ref:`flow_secure_app`.
  

#. 在 **Prism Central**, 点击 **?** 下拉菜单并选择 **Microsegmentation**.

   .. figure:: images/10.png
   
   启用Flow，每个Prism Central虚拟机需要额外的1GB内存，但对于用户来说不需要额外的操作，这个操作是自动的。此外，启用流程会验证每台AHV主机至少有1GB的可用RAM。在启用窗口中会罗列具有Flow功能的AHV集群。

#. 选择 **Enable Microsegmentation** 并点击 **Enable**.

   .. figure:: images/11.png

概要总结
+++++++++

- 微分段，Flow的一部分，是一个非集中式的安全实施框架，由Prism Central集中式管理。
- 集群中启用Flow后，即可通过Prism Central管理界面创建安全策略轻松保护虚拟机。安全策略是基于简单文本的类别来定义虚拟机组。这些功能用作标签，可以轻松将其应用于虚拟机，而无需任何其他网络设置。