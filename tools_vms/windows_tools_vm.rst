.. _windows_tools_vm:

----------------
Windows 工具 VM
----------------

简介
+++++++++

此Windows Server 2012 R2映像预装有许多工具，包括：

- Microsoft Remote Server Administration Tools (RSAT)
- PuTTY, CyberDuck, WinSCP
- Sublime Text 3, Visual Studio Code
- OpenOffice
- Python
- pgAdmin
- Chocolatey Package Manager

如果指示这样做，请在 ** Lab Setup **中将此虚拟机部署到您分配的群集上。

.. raw:: html

  <strong><font color="red">Only deploy the VM once, it does not need to be cleaned up as part of any lab completion.</font></strong>

部署工具VM
++++++++++++++++++

在 **Prism Central** > 选择:fa:`bars` **> Virtual Infrastructure > VMs**, 并单击 **Create VM**.

填写以下字段：

- **Name** - *Initials*-Windows-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 4 GiB

- 选择 **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - ToolsVM.qcow2
    - 选择 **Add**

- 选择 **Add New NIC**
    - **VLAN Name** - Secondary
    - 选择 **Add**

单击 **Save** 创建VM.

启动VM.

使用以下凭据通过RDP或控制台会话登录到VM：

- **Username** - NTNXLAB\\Administrator
- **password** - nutanix/4u
