.. _files_nfs_export:

------------------------
Files: Create NFS Export
------------------------

Overview
++++++++

In this exercise you will create and test a NFSv4 export, used to support clustered applications, store application data such as logging, or storing other unstructured file data commonly accessed by Linux clients.

Using NFS Exports
+++++++++++++++++

Creating the Export
...................

#. In **Prism > File Server**, click **+ Share/Export**.

#. Fill out the following fields:

   - **Name** - logs
   - **Description (Optional)** - File share for system logs
   - **File Server** - *Initials*\ **-Files**
   - **Share Path (Optional)** - Leave blank
   - **Max Size (Optional)** - Leave blank
   - **Select Protocol** - NFS

   .. figure:: images/23.png

#. Click **Next**.

#. Fill out the following fields:

   - Select **Use "Distributed" share/export type instead of "Standard"**
   - **Authentication** - System
   - **Default Access (For All Clients)** - No Access
   - Select **+ Add exceptions**
   - **Clients with Read-Write Access** - *The first 3 octets of your cluster network*\ .* (e.g. 10.42.78.\*)

   .. figure:: images/24.png

   A Distributed share type is more appropriate in this scenario if you have a dedicated top level directory for each host saving their logs on this share, allowing for effective load balancing across the Files cluster.

   By default an NFS export will allow read/write access to any host that mounts the export, but this can be restricted to specific IPs or IP ranges.

#. Review the **Summary** and click **Create**.

Testing the Export
..................

You will first provision a CentOS VM to use as a client for your Files export.

.. note::

  If you have already deployed the :ref:`linux_tools_vm` as part of another lab, you may use this VM as your NFS client instead.

#. In **Prism > VM > Table**, click **+ Create VM**.

#. Fill out the following fields:

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

     - **VLAN Name** - Secondary
     - Select **Add**

#. Click **Save**.

#. Select the *Initials*\ **-NFS-Client** VM and click **Power on**.

#. Note the IP address of the VM in Prism, and connect via SSH using the following credentials:

   - **Username** - root
   - **Password** - nutanix/4u

#. Execute the following:

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

#. Observe that the **logs** directory is mounted in ``/filesmnt/logs``.

#. Reboot the VM and observe the export is no longer mounted. To persist the mount, add it to ``/etc/fstab`` by executing the following:

     .. code-block:: bash

       echo 'Intials-Files.ntnxlab.local:/ /filesmnt nfs4' >> /etc/fstab

#. The following command will add 100 2MB files filled with random data to ``/filesmnt/logs``:

     .. code-block:: bash

       mkdir /filesmnt/logs/host1
       for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/filesmnt/logs/host1/file$i; done

#. Return to **Prism > File Server > Share > logs** to monitor performance and usage.

   Note that the utilization data is updated every 10 minutes.

(Optional) Expanding a Files Cluster
++++++++++++++++++++++++++++++++++++

Files offers the ability to scale up and scale out a deployment. Scaling up the CPU and memory of Files VMs allows an environment to support higher storage throughput and number of concurrent sessions. Currently, Files VMs can be scaled up to a maximum of 12 vCPU and 96GB of RAM each.

The true power of Files scalability is the ability to simply add more Files VMs, scaling out much like the underlying Nutanix distributed storage fabric. An individual Files cluster can scale out up to the number of physical nodes in the Nutanix cluster, ensuring that no more than 1 Files VM runs on a single node during normal operation.

#. Return to **Prism > File Server** and select *Initials*\ **-Files**.

#. Click **Update > Number of File Server VMs**.

   .. figure:: images/25.png

#. Increment the number of Files VMs from 3 to 4 and click **Next**.

   .. figure:: images/26.png

   Note that an additional IP will be consumed for both the client and storage networks to support the added Files VM.

#. Click **Next > Save**.

   The cluster will now deploy and power on a 4th Files VM. Status can be monitored in **Prism > Tasks**.

   .. note::

     Files cluster expansion should take approximately 10 minutes to complete.

   Following the expansion, verify client connections can now be load balanced to the new VM.

#. Connect to your *Initials*\ **-ToolsVM** via RDP or console.

#. Open **Control Panel > Administrative Tools > DNS**.

#. Fill out the following fields and click **OK**:

   - Select **The following computer**
   - Specify **dc.ntnxlab.local**
   - Select **Connect to the specified computer now**

   .. figure:: images/28.png

#. Open **DC.ntnxlab.local > Forward Lookup Zones > ntnxlab.local** and verify there are now four entries for *Initials*\ -**files**. Files leverages round robin DNS to load balance connections across Files VMs.

   .. figure:: images/29.png

   .. note::

     If only three entries are present, you can automatically update DNS entries from **Prism > File Server** by selecting your Files cluster and clicking **DNS**.
