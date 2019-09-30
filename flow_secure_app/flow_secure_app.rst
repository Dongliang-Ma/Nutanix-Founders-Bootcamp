.. _flow_secure_app:

----------------
Flow: 应用安全
----------------

*完成本实验预计需要30分钟*

概述
++++++++

Flow是一个应用为中心的网络安全产品，与Nutanix AHV和Prism Central紧密结合。Flow对运行在AHV上的虚拟机提供了丰富的网络流量可视化 ，自动化和安全功能。
微分段是Flow的组件，使用简单的策略管理来保护虚拟网络安全。使用Prism Central的多个类别（逻辑组），你可以创建功能强大的分布式防火墙，给管理员提供应用为中心的策略管理工具，用以保护虚拟机流量安全。
将其与Calm结合，可以自动化部署创建时就受保护的应用程序。
本实验中，你将创建一个安全策略以限制应用虚拟机之间的通信。

实验设置
+++++++++

本实验取决于多层 **Task Manger** Web应用的可用性。

Refer to the :ref:`taskman` lab for instructions on importing and launching the completed **Task Manager** blueprint. 有关导入和启动已完成的 **Task Manager** 蓝图，参考 :ref:`taskman` 实验说明。 

启动 **Task Manager** 部署后即可开始本实验。 **您无需等待蓝图部署完成即可开始本实验。**  

保护应用程序
+++++++++++++++++++++++

Flow提供了多种开箱即用的系统类比，例如AppType，AppTier和Environment，可以用来快速对虚拟机分组。立即开始使用这些预定义的类别，或添加您自己的类别用来自定义分组。

定义类别值
........................

Prism Central 使用类别来作为元数据，以标记虚拟机，并确定如何应用策略。

#. 在 **Prism Central**, 选择 :fa:`bars` **> Virtual Infrastructure > Categories**.

#. 选择 **AppType** 复选框并单击 **Actions > Update**.

   .. figure:: images/12.png

#. 单击上一个值旁边的 :fa:`plus-circle` 图标，添加额外的类别值。

#. 指定 *Initials*-**TaskMan** 值名称。

   .. figure:: images/13.png

#. 点击 **Save**.

#. 选择 **AppTier** 复选框并点击 **Actions > Update**.

#. 单击上一个值旁边的 :fa:`plus-circle` 图标，添加额外的类别值。

#. 指定 *Initials*-**TMWeb**  值名称。这个类别会被用于应用的Web层。

#. 单击 :fa:`plus-circle` 并指定 *Initials*-**TMDB**. 这个类别会被用于应用的MySQL数据库。

#. 点击 :fa:`plus-circle` 并指定 *Initials*-**TMLB**. 这个类别会用于应用的HAProxy load balancer.

   .. figure:: images/14.png

#. 点击 **Save**.

创建安全策略
..........................

Nutanix Flow包含策略驱动的安全性框架，该框架使用以工作负载为中心的方法，而不是以网络为中心。因此，无论它们的网络配置如何更改以及它们在数据中心中的位置如何，它都可以检查往返VM的流量。 以工作负载为中心，与网络无关的方法还使虚拟化团队能够实施这些安全策略，而不必依赖网络安全团队。

安全策略适用于类别（虚拟机的逻辑分组），而不适用于虚拟机本身。 因此，在给定类别中启动多少个虚拟机是无关紧要的。 类别中虚拟机的相关联的流量可以受到保护，无需任何规模的管理干预。

在你等待Task Manager应用通过Calm蓝图部署的过程中，创建保护应用的安全策略。

#. 在 **Prism Central** 下, 选择 :fa:`bars` **> Policies > Security Policies**.

#. 单击 **Create Security Policy > Secure Applications (App Policy) > Create**.

#. 填写以下字段:

   - **Name** - *Initials*-AppTaskMan
   - **Purpose** - Restrict unnecessary access to Task Manager
   - **Secure this app** - AppType: *Initials*-TaskMan
   -  **不要** 选择 **Filter the app type by category**.

   .. figure:: images/18.png

#. 点击 **Next**.

#. 如果有提示, 在 **Create App Security Policy** 向导教程图上点击 **OK, Got it!** 。

#. 为了更详细配置安全策略，点击 **Set rules on App Tiers** ，而不是对应用所有的组件应用相同的规则。

   .. figure:: images/19.png

#. 点击 **+ Add Tier**.

#. 从下拉菜单中选择 **AppTier:**\ *Initials*-**TMLB** .

#. 对 **AppTier:**\ *Initials*-**TMWeb** 和 **AppTier:**\ *Initials*-**TMDB** 重复 7-8 。

   .. figure:: images/20.png

   接下来你将定义 **Inbound** 规则，该规则定义了允许与你创建的应用程序通讯的源端。你可以允许所有的进站流量，或定义源白名单。默认情况下，安全策略是默认阻止所有进站流量的。

   在本场景下，我们将允许生产网络上所有客户端上端口80的进站TCP流量。

#. 在 **Inbound** 下, 点击 **+ Add Source**.

#. 指定 **Environment:Production** 并点击 **Add**.

   .. note::

     源可以通过IP或子网指定，但类别提供了更多的灵活性，因为数据可以跟随虚拟机而与其网络位置变化无关 。

#. 创建进站策略, 选择 **AppTier:**\ *Initials*-**TMLB** 旁边的图标e **+** 。

   .. figure:: images/21.png

#. 填写以下字段:

   - **Protocol** - TCP
   - **Ports** - 80

   .. figure:: images/22.png

   .. note::

     可以将多协议和端口添加到单个规则。

#. 单击 **Save**.

   Calm的工作流可能也需要访问这些虚拟机，包括横向扩展，纵向扩展，或升级。Calm通过SSH（TCP端口 22）与这些虚拟机通信。

#. 在 **Inbound** 下, 点击 **+ Add Source**.

#. 填写以下字段:

   - **Add source by:** - 选择 **Subnet/IP**
   - 指定 *Your Prism Central IP*\ /32

   .. note::

      **/32** 表示单个IP，而不是一个子网。

   .. figure:: images/23.png

#. 点击 **Add**.

#. 选择 **AppTier:**\ *Initials*-**TMLB** 旁边的图标 **+**  ，指定 **TCP** 端口 **22** 并单击 **Save**.

#. 对 **AppTier:**\ *Initials*-**TMWeb** 和 **AppTier:**\ *Initials*-**TMDB** 重复步骤18，以允许Calm跟Web层和数据库虚拟机通信。

   .. figure:: images/24.png

   默认情况下，安全策略允许应用发送所有出站流量到任意目的地。应用程序唯一需要的出站通信是数据库虚拟机能够与DNS服务器通信。

#. 在  **Outbound**, 从下拉菜单中选择 **Whitelist Only** , 并点击 **+ Add Destination**.

#. 填写以下字段:

   - **Add source by:** - 选择 **Subnet/IP**
   - 指定 *Your Domain Controller IP*\ /32

   .. figure:: images/25.png

#. 点击 **Add**.

#. 选择 **AppTier:**\ *Initials*- **TMDB** 右边的 **+** 图标, 指定 **UDP** 端口 **53** 并点击 **Save** ，以允许DNS流量.

   .. figure:: images/26.png

   应用的每一层都与其他层进行通信，该策略必须允许该流量。某些层，如负载均衡器和Web不需要在同一层内进行通信。

#. 定义 intra-app 通信, 点击 **Set Rules within App**.

   .. figure:: images/27.png

#. 点击 **AppTier:**\ *Initials*-**TMLB** 并选择 **No** ，以防止本层虚拟机间的通信。在本层只有一个负载均衡器。

#.  **AppTier:**\ *Initials*-**TMLB** 依旧被选中, 点击 **AppTier:**\ *Initials*-**TMWeb** 右边的 :fa:`plus-circle` 图标创建分层之间的规则.

#. 填写以下字段以允许负载均衡器Web层之间的TCP端口80上的通信：

   - **Protocol** - TCP
   - **Ports** - 80

   .. figure:: images/28.png

#. 点击 **Save**.

#. 点击 **AppTier:**\ *Initials*-**TMWeb** 并选择 **No** 以防止本层虚拟机之间的通讯。当有多个Web虚拟机时，他们之间不需要通信。

#. 当 **AppTier:**\ *Initials*-**TMWeb** 依旧被选中, 点击 **AppTier:**\ *Initials*-**TMDB** 右边的 :fa:`plus-circle` 图标创建另一个分层之间的规则。

#. 填写一下字段以允许TCP端口3306上的通信，从而允许Web服务器和MySQL数据库之间的数据库连接：

   - **Protocol** - TCP
   - **Ports** - 3306

   .. figure:: images/29.png

#. 点击 **Save**.

#. 点击 **Next** 审核安全策略。

#. 点击 **Save and Monitor** 并保存策略。

分配类别值
.........................

.. note::

到这个时候，你的应用蓝图应该已经完成部署。如果还没有完成，请等待直至完成。

现在，你需要将先前创建的类别应用于那些从任务管理器蓝图部署的虚拟机。Flow类别可以作为Calm蓝图的一部分进行分配，但本实验的目的是为了理解对环境中现有的虚拟机进行分配类别。

#. 在 **Prism Central**, 选择 :fa:`bars` **> Virtual Infrastructure > VMs**.

#. 点击 **Filters** 并搜索 *Initials-* 来罗列你的虚拟机.

   .. figure:: images/15.png

#. 使用复选框，选择与应用（HAProxy, MYSQL, WebServer-0, WebServer-1）相关联的4台虚拟机。

   .. figure:: images/16.png

   .. note::

     您还可以使用 **Label** 功能，将来可以更快地搜索该组虚拟机。

     .. figure:: images/16b.png

#. 搜索栏中指定 **AppType:**\ *Initials*-**TaskMan** 并点击 **Save** 图标将类别批量分配这4台虚拟机。

#. 只选定 *Initials*\ **-HAProxy** 虚拟机, 选择 **Actions > Manage Categories**, 指定 **AppTier:**\ *Initials*-**TMLB** 类别并点击  **Save** 。

   .. figure:: images/17.png

#. 重复步骤 5 分配 **AppTier:**\ *Initials*-**TMWeb** 给你的web层虚拟机.

#. 重复步骤 5 分配 **AppTier:**\ *Initials*-**TMDB** 给你的MySQL虚拟机.

#. 最后, 步骤 5 分配 **Environment:Dev** 给你 Windows客户端虚拟机.

监控和应用安全策略
+++++++++++++++++++++++++++++++++++++++++

在应用Flow策略之前, 您需要确保任务管理器应用按预期正常工作.

应用测试
.......................

#. 从 **Prism Central > Virtual Infrastructure > VMs** , 记录 *Initials*\ **-HAPROXY-0...** 和 *Initials*\ **-MYSQL-0...** 虚拟机的IP地址.

#. 启用 *Initials*\ **-WinClient-0** 虚拟机控制台.

   这台虚拟机是任务管理器应用蓝图创建的一部分。

#. 从 *Initials*\ **-WinClient-0** 控制台中打开浏览器并访问 \http://*HAPROXY-VM-IP*/.

#. 验证应用已加载并且任务可以被添加和删除。

   .. figure:: images/30.png

#. 打开 **Command Prompt** 并运行 ``ping -t MYSQL-VM-IP`` 验证客户端与数据库之间的连通性. 保持ping继续运行。

#. 打开另外一个 **Command Prompt** 窗口并运行 ``ping -t HAPROXY-VM-IP`` 以验证客户端与负载均衡器之间的连通性。保持ping继续运行。

   .. figure:: images/31.png

使用Flow可视化
........................

#. 返回 **Prism Central** 并选择 :fa:`bars` **> Virtual Infrastructure > Policies > Security Policies >**\ *Initials*-**AppTaskMan**.

#. 验证 **Environment: Dev** 显示为入站来源。来源和黄色线表明已监测到来自你客户虚拟机的流量。

   .. figure:: images/32.png

#. 将鼠标停留在 **Environment: Dev** 与 **AppTier:**\ *Initials*-**TMLB** 对连接线上查看协议和连接信息。

#. 点击这条黄色流线查看过去24小时内连接尝试图表。

   .. figure:: images/33.png

   是否还有其他检测到的出站流量？将鼠标悬停在这些连接上并确定哪些端口正在被使用。

#. 点击 **Update** 以编辑策略.

   .. figure:: images/34.png

#. 单击 **Next** 并等待检测到的流量进行填充。

#. 鼠标悬停在连接到 **AppTier:**\ *Initials*-**TMLB** 的 **Environment: Dev** 源上并点击出现的 :fa:`check` 图标.

   .. figure:: images/35.png

#. 点击 **OK** 完成添加规则.

    **Environment: Dev** 源应该变成蓝色, 表示它已是策略的一部分。 将鼠标悬停在流线上，并验证是否同时显示了ICMP（ping通信）和TCP端口80。

#. 点击 **Next > Save and Monitor** 以更新策略.

应用Flow策略
......................

In order to enforce the policy you have defined, the policy must be applied.为了执行已定义的策略，必须应用策略 。

#. 选择 *Initials*-**AppTaskMan**  并单击 **Actions > Apply**.

   .. figure:: images/36.png

#. 在确认对话框内输入  **APPLY** 并单击 **OK** 开始阻止流量.

#. 返回 *Initials*\ **-WinClient-0** 控制台.

   从Windows客户端到数据库服务器的持续ping通信会发生什么？ 该流量被阻止了吗？

#. 验证Windows客户虚拟机通过web浏览器仍然可以访问任务管理器应用和负载均衡器的IP。 

   您是否可以输入需要web服务器和数据库通信的新任务？

概要总结
+++++++++

- 微分段技术可提供额外的保护，抵御源自数据中心内部并从一台计算机横向传播到另一台计算机的恶意威胁。
- Prism Central下创建的类别在Calm蓝图内部是可使用的。 
- 在Prism Central，安全策略利用了基于文本的策略。
- Flow能够限制运行在AHV平台上虚拟机的特定端口和协议上的流量。
- 在 **Save and Monitor** 模式下创建的策略, 意味着流量实际上没有被阻止，除非应用了策略。这有助于了解连接数和保证无意阻止任意流量。


