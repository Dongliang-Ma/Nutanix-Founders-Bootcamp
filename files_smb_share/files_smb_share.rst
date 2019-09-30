.. _files_smb_share:

----------------------------
Files: 创建 SMB Share
----------------------------

简介
++++++++

在本练习中，您将创建和测试SMB Share，用于支持主目录，用户配置文件以及其他非结构化文件数据，例如Windows客户端通常访问的部门Share。

使用 SMB Shares
++++++++++++++++

创建 Share
..................

#. 在 **Prism > File Server**, 单击 **+ Share/Export**.

#. 填写以下字段：

   - **Name** - Marketing
   - **Description (Optional)** - Departmental share for marketing team
   - **File Server** - *Initials*\ **-Files**
   - **Share Path (Optional)** - Leave blank. This field allows you to specify an existing path in which to create the nested share.
   - **Max Size (Optional)** - Leave blank. This field allows you to set a hard quota for the individual share.
   - **Select Protocol** - SMB

   .. figure:: images/14.png

#. 单击 **Next**.

#. 选择 **Enable Access Based Enumeration** 和 **Self Service Restore**.

   .. figure:: images/15.png

   在创建部门Share时，应将其创建为 **Standard** Share。 这意味Share中的所有顶级目录和文件以及与Share的连接均由单个文件VM提供。

   **Distributed** Share适用于home主目录，用户配置文件和应用程序文件夹。 这种类型的Share在所有文件VM上分布顶级目录，并在文件群集内的所有文件VM之间平衡连接的负载。
   **Access Based Enumeration (ABE)** 确保只有给定用户具有读取权限的文件和文件夹对该用户可见。 Windows文件Share通常启用此功能。

   **Self Service Restore** 允许用户利用Windows先前版本轻松地将单个文件还原到基于Nutanix快照的先前版本。

#. 单击 **Next**.

#. 查看 **Summary** 并单击 **Create**.

   .. figure:: images/16.png

验证 Share
.................

#. 通过RDP或控制台连接 *Initials*\ **-ToolsVM**。

   .. note::

     ToolsVM已加入 **NTNXLAB.local** 域。 您可以使用任何加入域的VM来完成以下步骤。

#. 在 **File Explorer**打开 ``\\<Intials>-Files.ntnxlab.local\``。

   .. figure:: images/17.png

#. 通过将上一步中下载的SampleData_Small.zip文件解压缩到Share中，以测试对Marketing Share的访问。

   .. figure:: images/18.png

    - **NTNXLAB\\Administrator** 在文件群集部署期间，该用户被指定为文件管理员，默认情况下授予该用户对所有Share的读/写访问权限。
    - 管理其他用户的访问权限与任何其他SMB Share相同。

#. 右击 **Marketing > Properties**.

#. 选择 **Security** 选项卡并点击 **Advanced**.

   .. figure:: images/19.png

#. 选择 **Users (**\ *Initials*\ **-Files\\Users)** 并单击 **Remove**。

#. 单击 **Add**.

#. 单击 **Select a principal** 在 **Object Name** 字段填写 **Everyone** 。 单击 **OK**。

   .. figure:: images/20.png

#. 填写以下字段，然后单击 **OK**:

   - **Type** - Allow
   - **Applies to** - This folder only
   - 选择 **Read & execute**
   - 选择 **List folder contents**
   - 选择 **Read**
   - 选择 **Write**

   .. figure:: images/21.png

#. 单击 **OK > OK > OK** 保存权限更改。

   现在，所有用户都可以在Marketing Share中创建文件夹和文件。

    许多人利用share来利用配额以确保公平使用资源是很常见的。 通过文件，可以为Active Directory内的单个用户或特定的Active Directory安全组按份额设置软配额或硬配额。

#. 在 **Prism > File Server > Share > Marketing**, 单击 **+ Add Quota Policy**.

#. 填写以下字段，然后单击 **Save**:

   - 选择 **Group**
   - **User or Group** - SSP Developers
   - **Quota** - 10 GiB
   - **Enforcement Type** - Hard Limit

   .. figure:: images/22.png

#. 单击 **Save**.

#. 在仍选择“市场份额”的情况下，查看 **Share Details**，**Usage** 和 **Performance** 选项卡以了解每个share的可用情况，包括文件和连接的数量，一段时间内的存储利用率 ，延迟，吞吐量和IOPS。


   .. figure:: images/23.png
