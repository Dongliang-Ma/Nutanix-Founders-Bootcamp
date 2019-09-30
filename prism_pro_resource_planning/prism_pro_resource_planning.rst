.. _prism_pro_resource_planning:

--------------------------------
Prism Pro: 资源规划
--------------------------------

简介
++++++++

本实验将介绍Prism Central的资源规划功能，帮助您掌握集群利用率并更准确地预测集群扩展。

Lab 创建
+++++++++

该实验需要配置VM，稍后将在实验室中说明生成CPU和内存矩阵。

#. 在继续这个实验之前请按提示部署 :ref:`linux_tools_vm` 


#. 右键单击以下URL以打开新选项卡，然后导航到http://10.42.247.70上的网页，并在表单的“设置”部分中输入详细信息。 填写完所有字段后，单击“开始设置”。 这将为您的环境做好准备。 **在整个Prism Pro实验室中保持此选项卡打开，以便按照以后部分的指示返回。**

   .. figure:: images/ppro_08.png

#. 点击继续后，设置完成需要一些时间。 在此期间，切换回Prism Central并通过实验室。

Prism Central 资源规划
+++++++++++++++++++++++++++++++

Nutanix利用我们的X-Fit机器学习和数据分析作为Prism Pro的一部分。 我们利用该机器学习和数据分析来提供Cluster Runway资源倒计时和及时预测（What If规划）。

容量倒计时
...............

使用Prism Central的资源倒计时了解群集资源规划和建议的功能。

让我们查看集群的资源倒计时

#. 在 **Prism Central > Planning > Capacity Runway**.

  - 请注意资源倒计时摘要显示每个群集的剩余天数。
  - 在内存，CPU和存储空间不足之前，当前集群有多长时间？

#. 点击 **Prism-Pro-Cluster**集群。

. 您现在可以查看存储，CPU和内存的剩余资源。

   .. figure:: images/ppro_12.png

#. 选择“内存”选项卡时，您可以看到红色感叹号，指示此群集将耗尽内存的位置。 此时您可以将图表悬停在图表上，以查看这将发生在哪一天。

   .. figure:: images/ppro_13.png

#. 点击 **Optimize Resources**左边的按钮。 在这里，您可以看到环境中效率低下的虚拟机，并提供有关如何优化这些资源以尽可能高效的建议。

   .. figure:: images/ppro_14.png

#. 关闭优化资源弹出窗口。

What If 规划
................

#. 在页面左边的的 **Adjust Resources**下边, 点击 **Get Started**按钮. 我们现在可以使用它来开始规划新的工作负载，并了解将来如何扩展跑道。

#. 点击左边 **Workloads**选项下面的 **add/adjust**按钮。

   .. figure:: images/ppro_15.png

#. 为VDI添加一个并选择1000个用户。 您还可以设置应将此工作负载添加到系统的日期。 完成后保存此工作负载。

   .. figure:: images/ppro_16.png

   .. figure:: images/ppro_17.png

#. 添加另一个你选择的工作负载

#. 点击右边页面的 **Recommend** 按钮。

   .. figure:: images/ppro_18.png

#. 一旦建议书可用，在列表和图表视图之间切换，以更好地浏览您的场景。

   .. figure:: images/ppro_19.png

#. 点击右上角的 **Generate PDF** 按钮。 这将打开一个新选项卡，其中包含您已创建的方案/工作负载的PDF报告。

   .. figure:: images/ppro_19b.png

#. 查看报告

   .. figure:: images/ppro_20.png

概要总结
+++++++++

 -  Planning仪表板中的Capacity Runway视图允许您查看已注册集群的摘要资源倒计时信息，并访问有关每个集群的详细资源倒计时信息。
 -  规划仪表板中的“方案”视图允许您创建“假设”方案，以评估您指定的潜在工作负载的未来资源需求。
 -  您必须拥有Prism Pro许可才能使用资源规划工具。


