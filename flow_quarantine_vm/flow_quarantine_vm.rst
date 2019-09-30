.. _flow_quarantine_vm:

-------------------
Flow: 检疫隔离虚拟机（Quarantine）
-------------------

*完成本实验预计需要10分钟.*

概述
++++++++

检疫隔离可以分配虚拟机受限的策略，为管理员提供了要么阻止所有流量或允许部分子网流量的一个选项。严格检疫隔离（Strict）阻止虚拟机的所有通信，而取证检疫隔离（Forensic）则允许预定义列表的进站和出站流量。

检疫隔离虚拟机
+++++++++++++++++

在本任务中，我们会把一台虚拟机做检疫隔离，并观察这台虚拟机的行为。我们也将检查检疫隔离策略里面的可配置选项。

#. 返回 *Initials*\ **-WinClient-0** 控制台.

#. 打开 **Command Prompt** 并运行 ``ping -t HAPROXY-VM-IP`` 以验证客户端与负载均衡器之间的连通性。

   .. note::

     如果PING测试不成功，你可能需要将 **Environment:Dev** 的入站规则更新为 **AppTier:**\ *Initials*-**TMLB** ，把 **ICMP** 流量的 **Type** 和 **Code** 置为 **Any** ，如下图所示。应用更新后的 **AppTaskMan-**\ *Initials* 策略，PING应该可以成功。

     .. figure:: images/41.png

#. 在 **Prism Central > Virtual Infrastructure > VMs**, 选择 *Initials*\ **-HAPROXY-0...** VM.

#. 单击 **Actions > Quarantine VMs**.

   .. figure:: images/42.png

#. 选择 **Forensic** 并点击 **Quarantine**.

   客户端和负载均衡器之间连续ping会怎样？ 您可以从客户端虚拟机访问任务管理器应用程序网页吗？

#. 在 **Prism Central**, 选择 :fa:`bars` **> Virtual Infrastructure > Policies > Security Policies > Quarantine** 以查看所有检疫隔离的虚拟机。

#. 点击 **Update** 以编辑检疫隔离策略（Quarantine policy）.

   为了说明Flow此特殊策略的功能，您将客户端虚拟机添加为“取证工具”。 在生产环境中，允许对检疫隔离虚拟机进行入站访问的虚拟机可用于运行安全性和取证套件，例如Kali Linux或SANS SIFT。

#. 在 **Inbound** 下, 点击 **+ Add Source**.

#. 填写以下字段:

   - **Add source by:** - 选择 **Subnet/IP**
   - 指定 *Your WinClient VM IP*\ /32

   这个源端可以连接到哪些目标端呢？取证（Forensic）和严格（Strict）检疫隔离模式有什么区别呢？

   请注意，把虚拟机添加到 **Strict** 检疫隔离策略后，会禁止所有与虚拟机间的进站和出站通信。 **Strict** 策略适用于网络上对环境产生威胁的虚拟机。

#. 点击 :fa:`plus-circle` 图标左边的 **Quarantine: Forensic** 来创建进站规则.

#. 单击 **Save** 以允许客户虚拟机与 **Quarantine: Forensic** 类别之间的任意端口上的任意协议。

   .. figure:: images/43.png

#. 单击 **Next > Apply Now** 以保存和应用更新的策略。

   添加源后，与负载均衡器之间的PING会发生什么呢？你可以访问任务管理器Web应用吗？ 

#. 通过选定Prism Central中的虚拟机，单击 **Actions > Unquarantine VMs** ，你可以从 **Quarantine: Forensic** 类别中移除负载均衡器虚拟机。

概要总结
+++++++++

- 本练习中，你使用Flow来检疫隔离的两种模式来隔离一台虚拟机（Strict严格和Forensic取证）
- 检疫隔离的策略的优先级高于应用策略。检疫隔离可以阻止被应用策略所允许的流量。
- 当虚拟机被检疫隔离时，取证模式（Forensic）是允许对其有限访问的关键。

