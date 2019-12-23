.. title:: Nutanix Essentials Bootcamp

.. toctree::
  :maxdepth: 2
  :caption: Prism Central
  :name: _prism_central
  :hidden:

  what_is_prism_central/what_is_prism_central
  prism_central_overview/prism_central_overview
  prism_central_dashboards_reports/prism_central_dashboards_reports

.. toctree::
  :maxdepth: 2
  :caption: Prism Pro
  :name: _prism_pro
  :hidden:

  what_is_prism_pro/what_is_prism_pro
  prism_pro_efficiency_anomaly/prism_pro_efficiency_anomaly
  prism_pro_resource_planning/prism_pro_resource_planning
  prism_pro_xplay/prism_pro_xplay

.. toctree::
  :maxdepth: 2
  :caption: Flow
  :name: _flow
  :hidden:

  what_is_flow/what_is_flow
  flow_enable/flow_enable
  flow_secure_app/flow_secure_app
  flow_isolate_environments/flow_isolate_environments
  flow_quarantine_vm/flow_quarantine_vm

.. toctree::
  :maxdepth: 2
  :caption: Era
  :name: _era
  :hidden:

  era/era

.. toctree::
  :maxdepth: 2
  :caption:  Files Labs
  :name: _files_labs
  :hidden:

  files_deploy/files_deploy
  files_smb_share/files_smb_share
  files_nfs_export/files_nfs_export
  file_analytics_deploy/file_analytics_deploy
  file_analytics_scan/file_analytics_scan

.. toctree::
  :maxdepth: 2
  :caption: Calm
  :name: _calm
  :hidden:

  what_is_calm/what_is_calm
  calm_enable/calm_enable
  calm_projects/calm_projects
  calm_linux/calm_linux
  calm_win/calm_win


.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:



.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
  appendix/basics
  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm_cloud-init
  taskman/taskman

.. _getting_started:

---------------
开始实验
---------------

欢迎来到Nutanix训练营！本实验手册伴随着讲师指导的培训，介绍了Nutanix解决方案和许多常见的管理任务。 每个部分都有一个课程和实验，为您提供实践练习。 讲师会讲解练习并回答您可能遇到的任何其他问题。


版本
++++++++++

- Nutanix Bootcamp基于以下软件版本:
    - AOS & PC 5.11

- 可选的实验更新:


日程
++++++

- 通过实验掌握在Nutanix环境下Prism Central统一管理平台的特性及测试方法。

- 通过实验掌握在Nutanix环境下Flow原生网络服务的特性及测试方法。

- 通过实验掌握在Nutanix环境下Era数据库管理服务平台的特性及测试方法。

- 通过实验掌握在Nutanix环境下Files文件存储服务的特性及测试方法。

- 通过实验掌握在Nutanix环境下Calm以应用为中心的自动化服务的特性及测试方法。

简介
+++++++++++++

环境说明
+++++++++++++++++++

Nutanix Workshop旨在Nutanix Hosted POC环境中运行。 将为您的群集配置完成练习所需的所有必要图像，网络和VM。


网络
..........

Hosted POC 集群遵循标准命名约定:

- **Cluster Name** - POC\ *XX*
- **Subnet** - 10. **55** .\ *XX*\ .0
- **Cluster IP** - 10. **55** .\ *XX*\ .37

示例:

- **Cluster Name** - POC056
- **Subnet** - 10.55.56.0
- **Cluster IP** - 10.55.56.37

在整个Workshop期间，有多个实例需要用 *XX* 替换正确的子网，例如:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.55.\ *XX*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.55.\ *XX*\ .39
     - **PC** VM IP, Prism Central
   * - 10.55.\ *XX*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

每个群集配置有2个可用于VM的VLAN:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.55.\ *XX*\ .1/25
    - 0
    - 10.55.\ *XX*\ .50-10.21.\ *XX*\ .124
  * - Secondary
    - 10.55.\ *XX*\ .129/25
    - *XX1*
    - 10.55.\ *XX*\ .132-10.21.\ *XX*\ .253

帐号信息
...........

*<Cluster Password>* 由Workshop的负责人提供.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<password>*
   * - Prism Central
     - admin
     - *<password>*
   * - Controller VM
     - nutanix
     - *<password>*
   * - Prism Central VM
     - nutanix
     - *<password>*


访问说明
+++++++++++++++++++

可以通过VPN方式访问Nutanix Hosted POC环境:

Pulse Secure Access Client 下载地址

https://pan.baidu.com/s/1WmeZ4NizhJpWnYBCHaBqqQ  密码:4dln。 

Type: Policy Secure (UAC) or Connection Server(VPN)
Name: X-Labs - PHX
Server URL: xlv-uswest1.nutanix.com

OR

Type: Policy Secure (UAC) or Connection Server(VPN)
Name: X-Labs - RTP
Server URL: xlv-useast1.nutanix.com

用户名和密码

20 x VPN User Accounts: 
RTP-POC0XX-User01, RTP-POC0XX-User02 … RTP-POC0XX-User20 。(XX是您的集群ID)

VDI/VPN User Password: 
techX2019!



Nutanix 版本信息
++++++++++++++++++++

- **AHV Version** - AHV 20170830.279 (5.10+)
- **AOS Version** - 5.11
- **PC Version** - 5.11
