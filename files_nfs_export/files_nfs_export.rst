.. _files_nfs_export:

------------------------
Files: 创建 NFS Export
------------------------

简介
++++++++

在本练习中，您将创建和测试NFSv4导出，该导出用于支持集群应用程序，存储应用程序数据（例如日志记录）或存储Linux客户端通常访问的其他非结构化文件数据。

使用 NFS Exports
+++++++++++++++++

创建Export
...................

#. 在 **Prism > File Server**, 单击 **+ Share/Export**.

#. 填写以下字段：

   - **Name** - logs
   - **Description (Optional)** - File share for system logs
   - **File Server** - *Initials*\ **-Files**
   - **Share Path (Optional)** - Leave blank
   - **Max Size (Optional)** - Leave blank
   - **Select Protocol** - NFS

   .. figure:: images/24.png

#. 单击 **Next**.

#. 填写以下字段：

   - 选择 **Enable Self Service Restore**
      - These snapshots appear as a .snapshot directory for NFS clients.
   - **Authentication** - System
   - **Default Access (For All Clients)** - No Access
   - 选择 **+ Add exceptions**
   - **Clients with Read-Write Access** - *The first 3 octets of your cluster network*\ .* (e.g. 10.38.1.\*)

   .. figure:: images/25.png

   默认情况下，NFS导出将允许对安装exports的任何主机进行读/写访问，但是可以将其限制为特定的IP或IP范围。

#. 单击 **Next**.

#. 阅读 **Summary** 并单击 **Create**.

测试 Export
..................

首先，您将提供一个CentOS VM用作Files exprot的客户端。

.. note:: 如果已在另一个实验部署 :ref:`linux_tools_vm` ，则可以将此VM用作NFS客户端。

#. 在 **Prism > VM > Table**, 点击 **+ Create VM**.

#. 填写以下字段:

   - **Name** - *Initials*\ -NFS-Client
   - **Description** - CentOS VM for testing Files NFS export
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 2 GiB
   - Select **+ Add New Disk**
      - **Operation** - Clone from Image Service
      - **Image** - CentOS
      - Select **Add**
   - Select **Add New NIC**
      - **VLAN Name** - Primary
      - Select **Add**

#. 点击 **Save**.

#. 选择 *Initials*\ **-NFS-Client** VM 并单击 **Power on**.

#. 在Prism中记下VM的IP地址，并使用以下凭据通过SSH连接：

   - **Username** - root
   - **Password** - nutanix/4u

#. 执行以下命令：

     .. code-block:: bash

       [root@CentOS ~]# yum install -y nfs-utils #This installs the NFSv4 client
       [root@CentOS ~]# mkdir /filesmnt
       [root@CentOS ~]# mount.nfs4 <Intials>-Files.ntnxlab.local:/ /filesmnt/
       [root@CentOS ~]# df -kh
       Filesystem                      Size  Used Avail Use% Mounted on
       /dev/mapper/centos_centos-root  8.5G  1.7G  6.8G  20% /
       devtmpfs                        1.9G     0  1.9G   0% /dev
       tmpfs                           1.9G     0  1.9G   0% /dev/shm
       tmpfs                           1.9G   17M  1.9G   1% /run
       tmpfs                           1.9G     0  1.9G   0% /sys/fs/cgroup
       /dev/sda1                       494M  141M  353M  29% /boot
       tmpfs                           377M     0  377M   0% /run/user/0
       *intials*-Files.ntnxlab.local:/             1.0T  7.0M  1.0T   1% /afsmnt
       [root@CentOS ~]# ls -l /filesmnt/
       total 1
       drwxrwxrwx. 2 root root 2 Mar  9 18:53 logs

#. 观察 **logs**目录已安装在 ``/filesmnt/logs``.

#. 重新启动VM，并观察到出口不再挂载。 要保持挂载，通过执行以下命令将其添加到``/etc/fstab``中：

       echo 'Intials-Files.ntnxlab.local:/ /filesmnt nfs4' >> /etc/fstab

#. 以下命令将添加100个2MB的文件，其中填充了随机数据 ``/filesmnt/logs``:

     .. code-block:: bash

       mkdir /filesmnt/logs/host1
       for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/filesmnt/logs/host1/file$i; done

#. 返回 **Prism > File Server > Share > logs** 监控性能和使用情况。

  请注意，利用率数据每10分钟更新一次。
