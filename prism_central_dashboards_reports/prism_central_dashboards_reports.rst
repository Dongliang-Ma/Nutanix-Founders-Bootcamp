.. _prism_central_dashboards_reports:

-------------------------------------
Prism Central: 仪表板和报表
-------------------------------------

简介
++++++++

本实验将介绍Prism Central的面板和报表功能.

Prism Central 面板
++++++++++++++++++++++++

Prism Central 仪表板允许您快速查看在Prism Central实例下注册的系统的当前状态。

您可以从仪表板获得的信息类型取决于您的兴趣。它可以包括与集群运行状况相关的任何事件的警报，一直到性能信息。

.. figure:: images/510PC1.png

创建定制化仪表板
.........................

仪表板也可以定制化

#. 点击 **Home X**

主仪表板是系统首次安装时显示的第一个仪表板。

#. 此仪表板也可以自定义，但我们会创建新的仪表板。

#. 点击 **Manage Dashboards** 打开“管理仪表板”窗口

#. 点击 **+ New Dashboard**

#. 输入 "Dashboard-*intials*" 作为名称，然后单击 **Save**.

.. figure:: images/510PC2.png

#. 现在我们添加一些窗口小部件.


.. note::

  窗口小部件指的是仪表板的不同部分。窗口小部件可以是图形，警报板，也可以是显示信息块的信息表。您可以选择让小部件通知您感兴趣的任何信息，只需从可用的实体中进行选择即可。

#. 点击 **Add Widgets**.

#. From the list of available Widgets on the left hand side, notice there are three types of Widgets:

- CUSTOM WIDGETS
- CLUSTER WIDGETS
- APP WIDGETS

#. 选择 **CUSTOM WIDGETS** 里您可能感兴趣的任何小部件，并注意右侧显示一组自定义参数以指定。

#. 根据需要在选项中进行选择，然后单击 **Add to Dashboard**.

.. figure:: images/510PC4.png

#. 接下来，从 **CLUSTER WIDGETS**列表中选择另一个小部件。 这次单击 **Or, Add & Return to Dashboard.** 返回新创建的仪表板。

.. note::

  Prism Central的右上角有一个 **+ Add Widgets**按钮，可以随时添加更多小部件。

  另请注意，将鼠标悬停在仪表板上的任何小部件上都会在右上角显示一个灰色的X，可用于删除小部件。

.. figure:: images/510PC5.png

Prism Central 报表
+++++++++++++++++++++

Prism Central允许您生成有关群集环境的历史报告，可用于帮助管理员监控他们管理的群集的运行状况和性能。此类报告可包括资源消耗，异常行为和其他有价值的操作见解。这些报告可以手动生成，也可以从Prism Central自动生成，以便在最方便时通过电子邮件发送。

#. 在 **Prism Central  > Operations > Reports**.

#. 在那里，您将看到两个预定义的报告可供您立即使用:

- Cluster Efficiency Summary
- Environment Summary

.. figure:: images/510reports1.png

#. 让我们运行 **Cluster Efficiency Summary** 报表.

#. 选择 **Cluster Efficiency Summary**, 点击在 **Actions** 下拉菜单中的 **Run** .

.. figure:: images/monitoring_01.png

#. 接下来，填写以下字段并从 **Actions** 下拉菜单单击 **Run** :

- **Report instance Name** - *initials* - Cluster Efficiency 
- **Time Period for Report** - Last 24 Hours

#. 点击 **Run**.

#. 现在我们来运行 **Environment Summary** 报表.

#. 选择 **Environment Summary**, 从 **Actions**下拉菜单点击 **Run** .

#. 接下来，填写以下字段并单击 **Run**:

- **Report instance Name** - *initials* - Environment Summary
- **Time Period for Report** - Last 24 Hours

#. 点击 **Run**.

#. 报告完成后，选择每个报告，然后执行以下操作:

#. 点击 **View Instances.** 从 **Actions** 的下拉菜单.

- 要在单独的选项卡中查看报告，请单击报告的名称。
- 要下载报告，请选中其复选框，然后单击右上页面的 **Download**。

#. 查看您在本练习中创建的报告的内容。

生成定制化报表
......................

#. 要创建新的自定义报告，请单击 **+ New Report**.

#. 修改报表名字 **New Report** 为 *initials*-**Report**

.. figure:: images/510reports3.png

#. 从 **CUSTOM VIEWS** 目录左边, 点击 **Line Chart**填写下面信息:

- **Entity Type** - Cluster
- **Metric** - Memory Usage
- **Tittle** - *initials* - Cluster Memory Usage
- **Number of Entities** – 10
- **Sort Order** - Ascending

#. 点击 **Add**

.. figure:: images/510reports2.png

#. 从 **PRE-DEFINED VIEWS**, 点击任何你感兴趣的entities对象。

.. note::

  由于这些是预定义的，因此不需要额外的配置步骤，它们会立即添加到报告中。

#. 点击位于右边角落的 **Add Schedule** 按钮添加自动生成报告计划。

#. 选择任何所需的频率，时间和持续时间以运行报告。

.. figure:: images/510reports4.png

.. note:: 

  如果在Prism Central中正确配置了SMTP，则此自动报告也可以发送到输入的任何有效电子邮件地址。

#. 定制完你的报表之后点击 **Save** 。

#. 现在您的报告已保存，但请注意，它没有任何实例。 这是因为我们还没有运行报告。

#. 点击右上角的 **Run**来运行报告。

.. figure:: images/510reports5.png

.. note::

  克隆报告对于利用现有报告并对其进行编辑以进一步进行自定义非常有用。

#. 报告完成后，您将通过单 **下载**下的**PDF**看到报告的第一个实例可供查看。

#. 然后单击右上角的X退出。

#. 如果您按原样保留报告，它将自动运行并以设置的特定频率和时间发送到提供的电子邮件地址。

#. 如果需要不同的颜色或徽标，也可以在 **Report Settings**下自定义报告。


概要总结
+++++++++

- Prism Central可自定义仪表板允许您使用他们关心的信息设置用户和团队特定仪表板。
- Prism Central报告管理功能使您能够根据配置的计划配置和提供包含有关基础结构资源的信息的历史报告。
