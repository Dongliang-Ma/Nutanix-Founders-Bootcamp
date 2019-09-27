.. _files_deploy:

-------------
Files: 部署Nutanix Files功能
-------------

*完成本节所有文件实验所需的时间约为：1小时*

概览
++++++++

传统架构中，文件共享存储往往容易成为IT部门的另一个孤岛，从而遇到与SAN存储中使用过程中相同的扩展问题和缺乏持续创新的困扰，并带来了很多不必要的复杂性。 Nutanix认为企业云中没有孤岛存在的空间。通过将文件存储演变为一种应用程序，并运行在经过验证的HCI核心之上，Nutanix Files可通过一键式管理，同时为用户提供高性能，可伸缩性和快速创新的文件共享存储服务。

**在本实验中，您将逐步完成Nutanix Files功能的部署，并练习如何管理SMB共享和NFS共享，以及如何进行配置扩展，并探索全新的文件功能。该实验还将提供有关部署，配置和用例中需要特别关注的注意事项。**

.. _deploying_files:

实验前置条件
+++++++++
本实验需要使用在实验Windows_tools_vm章节中配置的应用程序。

如果在此实验之前还未尚未部署此VM，请在继续本练习之前，先根据该链接的步骤完成前置实验条件。

部署Nutanix Files服务
++++++++++++

#. 在 **Prism > File Server** 菜单中, 点击 **+ File Server** 以打开 **New File Server Pre-Check** 对话框.

   .. figure:: images/1.png

    为了节省时间，Files 3.5.2应用程序包已经上载到您的集群中，如果您发现没有此程序包，请到my.nutanix.com中进行下载。程序包二进制文件可以直接通过Prism下载，也可以在Nutanix support portal中进行下载并手动上传。

   .. figure:: images/2.png

    此外，超融合群集的 **Data Services** IP地址已经预先配置为（* 10.XX.YY.38 *）。在Nutanix Files群集中，存储需要通过iSCSI协议，通过Volume Group 提供给文件虚拟机，因此依赖于此数据服务IP。
   .. 注::

     如果是在您自己的环境中而不是预先准备的HPOC环境中进行部署，则可以通过以下方式轻松配置您自己环境的数据服务IP：选择> fa：`gear` **>Cluster Details** ，指定 
**iSCSI Data Services IP** ，然后单击 **Save** 。当前，数据服务IP要求必须与CVM在同一子网中。

   最后，文件将确保在群集上至少配置了1个网络。建议至少配置2个分段网络，以便将客户端网络和存储网络进行隔离。

#. 点击 **Continue**.

   .. figure:: images/3.png

#. 填写以下字段:

   - **Name** - *Intials*-Files (e.g. XYZ-Files)
   - **Domain** - ntnxlab.local
   - **File Server Size** - 1 TiB

   .. figure:: images/4.png

   .. 注::

     单击 **Custom Configuration** 将允许您根据用户数和吞吐量目标更改文件VM的放大和缩小尺寸. 它还允许手动调整“Files”群集的大小。

     .. figure:: images/5.png

#. 点击 **Next**.

#. 为 **Client Network** 选择 **Secondary - Managed** VLAN .

   每个Files VM将在客户端网络上使用一个IP

   .. 注::

    在HPOC环境中，如果需要采用不同的客户端网络和存储网络，则将客户端网络分配至第二VLAN至关重要

    在生产环境中，通常会使用专用虚拟网络来部署Files，以隔离用户客户端流程和存储通讯流量。当使用两个隔离网络时，根据设计，Files将禁止客户端访问存储网络，这意味着，分配给第一网络的所有VM将无法访问共享。


   .. 注::

     由于本实验使用通过AHV管理的网络，因此不需要配置单个IP。但在ESXi环境中，或在使用不受管理的AHV网络时，您将需要手工指定网络详细信息和可用IP信息，如下所示

     .. figure:: images/6.png

#. 将群集的 **Domain Controller** IP（位于：ref：`stagingdetails`中）指定为“ DNS解析器IP”（例如10.XX.YY.40）。保留默认（群集）NTP服务器
   .. raw:: html

     <strong><font color="red">为使Files群集成功找到并加入NTNXLAB.local域，将DNS解析器IP设置为您的集群的域控制器的虚拟机IP是至关重要的。默认情况下，此字段设置为Nutanix群集配置的主DNS服务器IP，此值不正确，将不起作用。</font></strong>

   .. figure:: images/7.png

#. 点击 **Next**.

#. 选择存储网络的 **Primary - Managed** VLAN.

   Each Files VM will consume a single IP on the storage network, plus 1 additional IP for the cluster.
   每个文件VM将在存储网络上消耗一个IP地址，同时整个群集还需要额外分配1个IP地址。

   .. figure:: images/8.png

#. 点击 **Next**.

#. F填写以下字段:

   - 选择**Use SMB Protocol**
   - **用户名** - Administrator@ntnxlab.local
   - **密码** - nutanix/4u
   - 选择 **Make this user a File Server admin**
   - 选择 **Use NFS Protocol**
   - **User Management and Authentication** - 选择非托管模式(unmanaged)

   .. figure:: images/9.png

   .. 注:: 在非托管模式下，仅通过UID / GID来标识用户，在Files 3.5版本中，Files可同时支持NFSv3 和 NFSv4

#. 点击 **Next**.

   默认情况下，Files将自动创建一个“保护域”，并为Files群集的每天创建Daily的快照并保留最后两个快照。在部署完成后，可以修改快照日程计划并定义远程复制站点。

   .. figure:: images/10.png

#. 点击 **Create** 以开始文件部署.

#. 在 **Prism > Tasks** 中监视部署进度

   部署大约需要10分钟.

   .. figure:: images/11.png

   .. 注::

   如果您收到有关DNS记录验证失败的警告，可以放心地忽略。共享群集没有使用与文件群集相同的DNS服务器，因此无法解析在部署Files服务时创建的DNS条目。

#. 在等待Files 服务器部署过程中，如果您尚未部署Windows Tools 虚拟机，则可以在此时间进行

#. 通过RDP协议或远程控制台连接到Windows Tools VM

#. 将用于文件分析的示例文件下载到工具VM：

   - `https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip <https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip>`_

#. 将文件分析json和qcow文件下载到Tools VM

   - `nutanix-file-analytics-2.0.0-metadata.json <http://10.42.194.11/workshop_staging/fileanalytics-2.0.0.json>`_
   - `nutanix-file-analytics-2.0.0.qcow2 <http://10.42.194.11/workshop_staging/nutanix-file_analytics-el7.6-release-2.0.0.qcow2>`_

#. Upon completion, return to **Prism > File Server** and select the *Initials*\ **-Files** server and 点击 **Protect**.

完成后，返回到“ Prism> File Server”，然后选择*Initials*\ **-Files** 服务器，然后单击 ** Protect ** ”。

   .. figure:: images/12.png

#. 观察默认的自助服务还原的日程计划，此功能同时控制Windows以前版本功能的快照计划。支持Windwos先前版本的功能，允许最终用户无需联系存储或备份管理员，就可以实现对文件的恢复和回滚操作。请注意，这些本地快照不能保护在本地集群出故障时，文件服务器集群不受影响，并且可以支持整个文件系统的数据可以直接复制到远程站点。点击 **Close** 。
   .. figure:: images/13.png

小贴士
+++++++++

关于 ** Nutanix Files** ，您应该了解哪些关键知识？

-Files 可以快速部署在现有Nutanix群集之上，从而为用户共享，主目录，部门共享，应用程序和任何其他通用文件存储需求提供SMB和NFS存储。
-Files不是单点解决方案。VM，文件，块和对象存储都可以在同一平台上使用相同的管理工具来交付，从而降低了复杂性和管理孤岛

.. _files_deploy:

-------------
Files: Deploy
-------------

*Estimated time to complete all Files labs: 1 HOUR*

Overview
++++++++

Traditionally, file storage has been yet another silo within IT, introducing unnecessary complexity and suffering from the same issues of scale and lack of continuous innovation seen in SAN storage. Nutanix believes there is no room for silos in the Enterprise Cloud. By approaching file storage as an app, running in software on top of a proven HCI core, Nutanix Files  delivers high performance, scalability, and rapid innovation through One Click management.

**In this lab you will step through a Files deployment, manage SMB shares and NFS exports, scale out the environment, and explore upcoming Files features. The lab will provide key considerations around deployment, configuration, and use cases.**

.. _deploying_files:

Lab Setup
+++++++++

This lab requires applications provisioned as part of the :ref:`windows_tools_vm`.

If you have not yet deployed this VM, see the linked steps before proceeding with the lab.

Deploy Files
++++++++++++

#. In **Prism > File Server**, click **+ File Server** to open the **New File Server Pre-Check** dialogue.

   .. figure:: images/1.png

   For the purpose of saving time, the Files 3.5.2 package has already been uploaded to your cluster. Files binaries can be downloaded directly through Prism or uploaded manually.

   .. figure:: images/2.png

   Additionally, the cluster's **Data Services** IP Address has already been configured (*10.XX.YY.38*). In a Files cluster, storage is presented to the Files VMs as a Volume Group via iSCSI, hence the dependency on the Data Services IP.

   .. note::

     If staging your own environment, the Data Services IP can be easily configured by selecting :fa:`gear` **> Cluster Details**, specifying the **iSCSI Data Services IP**, and clicking **Save**. Currently, the Data Services IP must be in the same subnet as your CVMs.

   Lastly Files will ensure that at least 1 network has been configured on the cluster. A minimum of 2 networks are recommended to have segmentation between the client side and storage side networks.

#. Click **Continue**.

   .. figure:: images/3.png

#. Fill out the following fields:

   - **Name** - *Intials*-Files (e.g. XYZ-Files)
   - **Domain** - ntnxlab.local
   - **File Server Size** - 1 TiB

   .. figure:: images/4.png

   .. note::

     Clicking **Custom Configuration** will allow you to alter the scale up and scale out sizing of the Files VMs based on User and Throughput targets. It also allows for manual sizing of the Files cluster.

     .. figure:: images/5.png

#. Click **Next**.

#. Select the **Secondary - Managed** VLAN for the **Client Network**.

   Each Files VM will consume a single IP on the client network.

   .. note::

     In the HPOC environment it is critical to use the secondary VLAN for the client network if using separate client and storage networks.

     It is typically desirable in production environments to deploy Files with dedicated virtual networks for client and storage traffic. When using two networks, Files will, by design, disallow client traffic the storage network, meaning VMs assigned to the primary network will be unable to access shares.

   .. note::

     As this is an AHV managed network, configuration of individual IPs is not necessary. In an ESXi environment, or using an unmanaged AHV network, you would specify the network details and available IPs as shown below.

     .. figure:: images/6.png

#. Specify your cluster's **Domain Controller** VM IP (found in :ref:`stagingdetails`) as the **DNS Resolver IP** (e.g. 10.XX.YY.40). Leave the default (cluster) NTP Server.

   .. raw:: html

     <strong><font color="red">In order for the Files cluster to successfully find and join the NTNXLAB.local domain it is critical that the DNS Resolver IP is set to the Domain Controller VM IP FOR YOUR CLUSTER. By default, this field is set to the primary Name Server IP configured for the Nutanix cluster, this value is incorrect and will not work.</font></strong>

   .. figure:: images/7.png

#. Click **Next**.

#. Select the **Primary - Managed** VLAN for the Storage Network.

   Each Files VM will consume a single IP on the storage network, plus 1 additional IP for the cluster.

   .. figure:: images/8.png

#. Click **Next**.

#. Fill out the following fields:

   - Select **Use SMB Protocol**
   - **Username** - Administrator@ntnxlab.local
   - **Password** - nutanix/4u
   - Select **Make this user a File Server admin**
   - Select **Use NFS Protocol**
   - **User Management and Authentication** - Unmanaged

   .. figure:: images/9.png

   .. note:: In unmanaged mode, users are only identified by UID/GID. In Files 3.5, Files supports both NFSv3 and NFSv4

#. Click **Next**.

   By default, Files will automatically create a Protection Domain to take daily snapshots of the Files cluster and retain the previous 2 snapshots. After deployment, the snapshot schedule can be modified and remote replication sites can be defined.

   .. figure:: images/10.png

#. Click **Create** to begin the Files deployment.

#. Monitor deployment progress in **Prism > Tasks**.

   Deployment should take approximately 10 minutes.

   .. figure:: images/11.png

   .. note::

     If you receive a warning regarding DNS record validation failure, this can be safely ignored. The shared cluster does not use the same DNS servers as your Files cluster, and as a result is unable to resolve the DNS entries created when deploying Files.

#. While waiting for the file server deployment, if you have not already done so deploy the Windows Tools VM.

#. Connect to the Windows Tools VM via RDP or console

#. Download the sample files for File Analytics to the Tools VM:

   - `https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip <https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip>`_

#. Download the File Analytics json and qcow files to the Tools VM

   - `nutanix-file-analytics-2.0.0-metadata.json <http://10.42.194.11/workshop_staging/fileanalytics-2.0.0.json>`_
   - `nutanix-file-analytics-2.0.0.qcow2 <http://10.42.194.11/workshop_staging/nutanix-file_analytics-el7.6-release-2.0.0.qcow2>`_

#. Upon completion, return to **Prism > File Server** and select the *Initials*\ **-Files** server and click **Protect**.

   .. figure:: images/12.png

#. Observe the default Self Service Restore schedules, this feature controls the snapshot schedule for Windows' Previous Versions functionality. Supporting Previous Versions allows end users to roll back changes to files without engaging storage or backup administrators. Note these local snapshots do not protect the file server cluster from local failures and that replication of the entire file server cluster can be performed to remote Nutanix clusters. Click **Close**.

   .. figure:: images/13.png

Takeaways
+++++++++

What are the key things you should know about **Nutanix Files**?

- Files can be rapidly deployed on top of existing Nutanix clusters, providing SMB and NFS storage for user shares, home directories, departmental shares, applications, and any other general purpose file storage needs.
- Files is not a point solution. VM, File, Block, and Object storage can all be delivered by the same platform using the same management tools, reducing complexity and management silos.
