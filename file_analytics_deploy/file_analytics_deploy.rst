.. _file_analytics_deploy:

----------------------
File Analytics: 部署
----------------------

简介
++++++++

在本练习中，您将部署File Analytics VM，并扫描现有共享以构建仪表板。 您还将创建异常警报并查看文件服务器实例的审核详细信息。

部署 File Analytics
+++++++++++++++++++++

#. 在 **Prism** > **File Server** > 单击 **Deploy File Analytics**

   .. figure:: images/31.png

#. 选择 **Deploy**

   为了节省时间，File Analytics 2.0.0程序包已经上载到您的集群中。 可以从Nutanix门户网站下载文件二进制文件并手动上传。

#. 上传完成后，选择 **Install**

#. 填写详细信息

   - **Name** - Initials
   - **Storage Container** – Will automatically select the container used by your file server instance
   - **Network List** – Secondary - Managed

#. 选择 **Show Advanced Settings**

#. 确保 **DNS Resolver IP** 设置为Active Directory的IP, ntnxlab.local, domain controller/DNS IP address and **ONLY** that address.

#. 选择 **Deploy**

#. 你可以从 **Tasks** 页面监控部署情况。 Analytics VM部署大约需要5分钟。

#. In **Prism** > **File Server** > click **File Analytics**

   .. figure:: images/33.png

#. 在Enable File Analytics 页面上，输入您的域管理员，该域管理员也是您的文件服务器管理员。
   - **Username**: administrator
   - **Password**: nutanix/4u

   .. figure:: images/34.png

#. 选择 **Enable**
