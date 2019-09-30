.. _taskman:

----------------------
部署Task Manager
----------------------

*完成本实验的估计时间为10分钟。*

简介
++++++++

**此练习将引导您完成导入和启动Calm蓝图的工作，以部署用于多个实验室的简单Task Manager应用程序。 您无需完成此练习，除非指示您转而准备另一个实验室。**

验证默认 Project
+++++++++++++++++++++++++++++

在 **Prism Central**, 选择 :fa:`bars` **> Services > Calm**.

.. figure:: images/0.png

在左手工具栏点击 |projects| **Projects** 并选择 **default** project.

.. note::

  将鼠标悬停在图标上将显示其标题。

在 **AHV Cluster** 下确认从下拉列表中选择了您分配的集群，否则选择它。

.. figure:: images/1.png

在 **Network** 下, 验证 **Primary** 和 **Secondary** 网络都被选择，**Primary** 网络为默认。 此外, 进行如下所示的选择。

.. figure:: images/2.png

如果进行了更改，请单击 **Save**.

导入蓝图
+++++++++++++++++++++++

右击 :download:`this link <TaskManager.json>` 并 **Save Link As...** 下载本练习中使用的示例应用程序的蓝图。

点击左侧工具栏中 |blueprints| **Blueprints** ，以查看可用的Calm蓝图。

点击 **Upload Blueprint** 并选择之前下载好的 **TaskManager.json**。

填写以下字段

- **Blueprint Name** - *Initials*-TaskManager
- **Project** - default

.. figure:: images/3.png

点击 **Upload**.

.. note::

  如果您在尝试上传蓝图时遇到错误，请刷新浏览器，然后重试。

配置蓝图
+++++++++++++++++++++++++

在启动蓝图之前，必须首先提供指定的信息，这些信息未存储在导出的Calm蓝图中，包括凭证。

在左边 **Application Profile** 窗格, 填写以下字段：

- **Mysql_password** - nutanix/4u

.. figure:: images/4.png

选择右边窗格的 **WinClient** 服务, 在 **VM** 选项卡下, 确保 **Image** 设置为 **Windows10** 镜像，如下所示。

.. figure:: images/4b.png

在 **Network Adapters (NICs)** 下, 确保 **NIC 1** 设置为 **Primary** ，如下所示.

.. figure:: images/4c.png

选择 **WebServer**, **HAProxy**, 和 **MySQL** 服务并确保 **NIC 1** 设置为 **Primary**.

.. figure:: images/4d.png

点击 **Save**.

.. figure:: images/5.png

点击 **Credentials**.

.. figure:: images/6.png

通过点击其名字扩展 **CENTOS** 认证。 将以下密钥复制并粘贴到“ SSH私钥”字段中：

::

  -----BEGIN RSA PRIVATE KEY-----
  MIIEowIBAAKCAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNG
  ZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK
  6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9
  HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7Gy
  hCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNR
  uz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5OrwIBJQKCAQB++q2WCkCmbtByyrAp
  6ktiukjTL6MGGGhjX/PgYA5IvINX1SvtU0NZnb7FAntiSz7GFrODQyFPQ0jL3bq0
  MrwzRDA6x+cPzMb/7RvBEIGdadfFjbAVaMqfAsul5SpBokKFLxU6lDb2CMdhS67c
  1K2Hv0qKLpHL0vAdEZQ2nFAMWETvVMzl0o1dQmyGzA0GTY8VYdCRsUbwNgvFMvBj
  8T/svzjpASDifa7IXlGaLrXfCH584zt7y+qjJ05O1G0NFslQ9n2wi7F93N8rHxgl
  JDE4OhfyaDyLL1UdBlBpjYPSUbX7D5NExLggWEVFEwx4JRaK6+aDdFDKbSBIidHf
  h45NAoGBANjANRKLBtcxmW4foK5ILTuFkOaowqj+2AIgT1ezCVpErHDFg0bkuvDk
  QVdsAJRX5//luSO30dI0OWWGjgmIUXD7iej0sjAPJjRAv8ai+MYyaLfkdqv1Oj5c
  oDC3KjmSdXTuWSYNvarsW+Uf2v7zlZlWesTnpV6gkZH3tX86iuiZAoGBAKM0mKX0
  EjFkJH65Ym7gIED2CUyuFqq4WsCUD2RakpYZyIBKZGr8MRni3I4z6Hqm+rxVW6Dj
  uFGQe5GhgPvO23UG1Y6nm0VkYgZq81TraZc/oMzignSC95w7OsLaLn6qp32Fje1M
  Ez2Yn0T3dDcu1twY8OoDuvWx5LFMJ3NoRJaHAoGBAJ4rZP+xj17DVElxBo0EPK7k
  7TKygDYhwDjnJSRSN0HfFg0agmQqXucjGuzEbyAkeN1Um9vLU+xrTHqEyIN/Jqxk
  hztKxzfTtBhK7M84p7M5iq+0jfMau8ykdOVHZAB/odHeXLrnbrr/gVQsAKw1NdDC
  kPCNXP/c9JrzB+c4juEVAoGBAJGPxmp/vTL4c5OebIxnCAKWP6VBUnyWliFhdYME
  rECvNkjoZ2ZWjKhijVw8Il+OAjlFNgwJXzP9Z0qJIAMuHa2QeUfhmFKlo4ku9LOF
  2rdUbNJpKD5m+IRsLX1az4W6zLwPVRHp56WjzFJEfGiRjzMBfOxkMSBSjbLjDm3Z
  iUf7AoGBALjvtjapDwlEa5/CFvzOVGFq4L/OJTBEBGx/SA4HUc3TFTtlY2hvTDPZ
  dQr/JBzLBUjCOBVuUuH3uW7hGhW+DnlzrfbfJATaRR8Ht6VU651T+Gbrr8EqNpCP
  gmznERCNf9Kaxl/hlyV5dZBe/2LIK+/jLGNu9EJLoraaCBFshJKF
  -----END RSA PRIVATE KEY-----

点击其名字扩展 **WIN_VM_CRED** 认证。 输入 **nutanix/4u** 作为 **Password**.

.. figure:: images/7.png

点击 **Save**.

蓝图保存后, 点击 **Back**.

.. figure:: images/8.png

运行蓝图
+++++++++++++++

在提供认证后, **Publish**, **Download**, 和 **Launch** 现在可以从工具栏中使用。 点击 **Launch**.

填写以下字段:

- **Name of the Application** - *Initials*-TaskManager1
- **User_initials** - *Initials*

.. figure:: images/9.png

点击 **Create**.

您可以通过以下方式监视应用程序部署的状态： |applications| **Applications** 点击你应用程序的名字。

设置完整的应用程序大约需要15分钟。 在配置应用程序时，继续进行实验的下一部分。

.. |projects| image:: images/projects.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/applications.png
