.. _calm_win:

-----------------------
Calm: Windows 工作负载
-----------------------

*完成本实验的估计时间为60分钟。*

简介
++++++++

在本练习中，您将通过构建和部署可安装和配置多层的蓝图，探索在Nutanix Calm中使用Windows工作负载的基础知识。 本实验假设您熟悉基本的Calm功能或已完成 :ref:`calm_linux` **lab.**

创建蓝图
++++++++++++++++++++++

#. 在Calm中，创建一个新的 **Multi VM/Pod Blueprint**.

#. 填写以下字段，然后单击 **Proceed** 启动蓝图编辑器：

   - **Name** - *Initials*-CalmWindowsIntro
   - **Description** - [BugNET](\http://@@{MSIIS.address}@@/bugnet)
   - **Project** - *Initials*-Calm

   .. note::

     使用提供的描述值将创建指向BugNET应用程序的超链接，以在部署完成后启动。

#. 点击 **Credentials** 并创建以下两个凭证：

   +---------------------+---------------------+---------------------+
   | **Credential Name** | WIN_VM_CRED         | SQL_CRED            |
   +---------------------+---------------------+---------------------+
   | **Username**        | Administrator       | Administrator       |
   +---------------------+---------------------+---------------------+
   | **Secret Type**     | Password            | Password            |
   +---------------------+---------------------+---------------------+
   | **Password**        | nutanix/4u          | Str0ngSQL/4u$       |
   +---------------------+---------------------+---------------------+

   .. figure:: images/credentials.png

#. 点击 **Save** 并返回 Blueprint Editor.

#. 点击 **Configuration** 并创建以下 **Downloadable Image Configuration**:

   - **Package Name** - MSSQL2014_ISO
   - **Description** - Microsoft SQL 2014 Installation ISO
   - **Image Name** - MSSQL2014.iso
   - **Image Type** - ISO Image
   - **Architecture** - X86_64
   - **Source URI** - http://download.microsoft.com/download/7/9/F/79F4584A-A957-436B-8534-3397F33790A6/SQLServer2014SP3-FullSlipstream-x64-ENU.iso
   - **Product Name** - MSSQL
   - **Product Version** - 2014
   - **Checksum Algorithm** - *Leave blank*
   - **Checksum Value** - *Leave blank*

   .. figure:: images/downloadable_image_config.png

#. 点击 **Save** 然后返回到Blueprint Editor.

#. 使用 **Default** 应用程序配置文件，指定以下内容 **Variables** 到 **Configuration Panel**:

   +---------------------+---------------+----------------+---------------+---------------+
   | **Name**            | **Data Type** | **Value**      | **Secret**    | **Runtime**   |
   +=====================+===============+=================+==============+===============+
   | DbName              | String        | BugNET         | No            | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+
   | DbUsername          | String        | BugNETUser     | No            | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+
   | DbPassword          | String        | Nutanix/4u$    | Yes           | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+
   | User_initials       | String        | *Leave blank*  | No            | Yes           |
   +---------------------+---------------+----------------+---------------+---------------+

   .. figure:: images/variables.png

#. 点击 **Save**.

添加 Services
+++++++++++++++

#. 在 **Application Overview > Services**下, 点击 :fa:`plus-circle` 两次以加入两个新Service。

   .. figure:: images/create_service.png

#. 使用下表填写每个服务的**VM**字段：

   +------------------------------+---------------------------+---------------------------+
   | **Service Name**             | **MSSQL**                 | **MSIIS**                 |
   +------------------------------+---------------------------+---------------------------+
   | **Name**                     | MSSQL2014                 | MSIIS8                    |
   +------------------------------+---------------------------+---------------------------+
   | **Cloud**                    | Nutanix                   | Nutanix                   |
   +------------------------------+---------------------------+---------------------------+
   | **Operating System**         | Windows                   | Windows                   |
   +------------------------------+---------------------------+---------------------------+
   | **VM Name**                  | @@{User_initials}@@-MSSQL | @@{User_initials}@@-MSIIS |
   +------------------------------+---------------------------+---------------------------+
   | **Number of Images**         | 2                         | 1                         |
   +------------------------------+---------------------------+---------------------------+
   | **Image 1**                  | Windows2012R2             | Windows2012R2             |
   +------------------------------+---------------------------+---------------------------+
   | **Device Type 1**            | DISK                      | DISK                      |
   +------------------------------+---------------------------+---------------------------+
   | **Device Bus 1**             | SCSI                      | SCSI                      |
   +------------------------------+---------------------------+---------------------------+
   | **Bootable 1**               | Yes                       | Yes                       |
   +------------------------------+---------------------------+---------------------------+
   | **Image 2**                  | MSSQL2014_ISO             | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **Device Type 2**            | CD-ROM                    | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **Device Bus 2**             | IDE                       | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **Bootable 2**               | No                        | N/A                       |
   +------------------------------+---------------------------+---------------------------+
   | **vCPUs**                    | 2                         | 2                         |
   +------------------------------+---------------------------+---------------------------+
   | **Cores per vCPU**           | 2                         | 2                         |
   +------------------------------+---------------------------+---------------------------+
   | **Memory (GiB)**             | 6                         | 6                         |
   +------------------------------+---------------------------+---------------------------+
   | **Guest Customization**      | Yes                       | Yes                       |
   +------------------------------+---------------------------+---------------------------+
   | **Type**                     | Sysprep                   | Sysprep                   |
   +------------------------------+---------------------------+---------------------------+
   | **Install Type**             | Prepared                  | Prepared                  |
   +------------------------------+---------------------------+---------------------------+
   | **Script**                   | *Copy script below table* | *Copy script below table* |
   +------------------------------+---------------------------+---------------------------+
   | **Additional vDisks**        | 1                         | 1                         |
   +------------------------------+---------------------------+---------------------------+
   | **Device Type**              | DISK                      | DISK                      |
   +------------------------------+---------------------------+---------------------------+
   | **Device Buse**              | SCSI                      | SCSI                      |
   +------------------------------+---------------------------+---------------------------+
   | **Size (GiB)**               | 100                       | 100                       |
   +------------------------------+---------------------------+---------------------------+
   | **VGPUs**                    | None                      | None                      |
   +------------------------------+---------------------------+---------------------------+
   | **Categories**               | None                      | None                      |
   +------------------------------+---------------------------+---------------------------+
   | **Network Adapters**         | 1                         | 1                         |
   +------------------------------+---------------------------+---------------------------+
   | **NIC 1**                    | Primary                   | Primary                   |
   +------------------------------+---------------------------+---------------------------+
   | **Check log-in upon create** | Yes                       | Yes                       |
   +------------------------------+---------------------------+---------------------------+
   | **Credential**               | WIN_VM_CRED               | WIN_VM_CRED               |
   +------------------------------+---------------------------+---------------------------+
   | **Address**                  | NIC 1                     | NIC 1                     |
   +------------------------------+---------------------------+---------------------------+
   | **Connection Type**          | Windows (Powershell)      | Windows (Powershell)      |
   +------------------------------+---------------------------+---------------------------+
   | **Connection Port**          | 5985                      | 5985                      |
   +------------------------------+---------------------------+---------------------------+
   | **Delay (in seconds)**       | Increase to **90**        | Increase to **90**        |
   +------------------------------+---------------------------+---------------------------+

   .. code-block:: XML
     :caption: Sysprep Script

     <?xml version="1.0" encoding="UTF-8"?>
     <unattend xmlns="urn:schemas-microsoft-com:unattend">
       <settings pass="specialize">
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <ComputerName>@@{name}@@</ComputerName>
             <RegisteredOrganization>Nutanix</RegisteredOrganization>
             <RegisteredOwner>Acropolis</RegisteredOwner>
             <TimeZone>UTC</TimeZone>
          </component>
          <component xmlns="" name="Microsoft-Windows-TerminalServices-LocalSessionManager" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">
             <fDenyTSConnections>false</fDenyTSConnections>
          </component>
          <component xmlns="" name="Microsoft-Windows-TerminalServices-RDP-WinStationExtensions" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">
             <UserAuthentication>0</UserAuthentication>
          </component>
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Networking-MPSSVC-Svc" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <FirewallGroups>
                <FirewallGroup wcm:action="add" wcm:keyValue="RemoteDesktop">
                   <Active>true</Active>
                   <Profile>all</Profile>
                   <Group>@FirewallAPI.dll,-28752</Group>
                </FirewallGroup>
             </FirewallGroups>
          </component>
       </settings>
       <settings pass="oobeSystem">
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <UserAccounts>
                <AdministratorPassword>
                   <Value>@@{WIN_VM_CRED.secret}@@</Value>
                   <PlainText>true</PlainText>
                </AdministratorPassword>
             </UserAccounts>
             <AutoLogon>
                <Password>
                   <Value>@@{WIN_VM_CRED.secret}@@</Value>
                   <PlainText>true</PlainText>
                </Password>
                <Enabled>true</Enabled>
                <Username>Administrator</Username>
             </AutoLogon>
             <FirstLogonCommands>
                <SynchronousCommand wcm:action="add">
                   <CommandLine>cmd.exe /c netsh firewall add portopening TCP 5985 "Port 5985"</CommandLine>
                   <Description>Win RM port open</Description>
                   <Order>1</Order>
                   <RequiresUserInput>true</RequiresUserInput>
                </SynchronousCommand>
                <SynchronousCommand wcm:action="add">
                   <CommandLine>powershell -Command "Enable-PSRemoting -SkipNetworkProfileCheck -Force"</CommandLine>
                   <Description>Enable PS-Remoting</Description>
                   <Order>2</Order>
                   <RequiresUserInput>true</RequiresUserInput>
                </SynchronousCommand>
                <SynchronousCommand wcm:action="add">
                   <CommandLine>powershell -Command "Set-ExecutionPolicy -ExecutionPolicy RemoteSigned"</CommandLine>
                   <Description>Enable Remote-Signing</Description>
                   <Order>3</Order>
                   <RequiresUserInput>false</RequiresUserInput>
                </SynchronousCommand>
             </FirstLogonCommands>
             <OOBE>
                <HideEULAPage>true</HideEULAPage>
                <SkipMachineOOBE>true</SkipMachineOOBE>
             </OOBE>
          </component>
          <component xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="Microsoft-Windows-International-Core" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">
             <InputLocale>en-US</InputLocale>
             <SystemLocale>en-US</SystemLocale>
             <UILanguageFallback>en-us</UILanguageFallback>
             <UILanguage>en-US</UILanguage>
                <UserLocale>en-US</UserLocale>
          </component>
       </settings>
     </unattend>

   花一点时间查看Sysprep脚本。 您可以看到配置为使用WIN_VM_CRED密码自动登录到本地Administrator帐户的VM。 虽然此练习不会将VM加入到Active Directory域中，但是您可以使用Sysprep或Package Install任务脚本来自动加入域。

    此外，防火墙已配置为允许端口5985（Calm用于对主机执行PowerShell脚本）。 对于熟悉Calm早期版本的用户，不再需要 **Karan** 服务VM才能将PowerShell命令代理到服务VM。 相反，Calm引入了对在远程主机上运行PowerShell脚本的本机支持。

    与:ref:`calm_linux` 实验中任务管理器中的应用类似, 您想要确保数据库在IIS Web服务器设置之前可用。

#. 在Blueprint Editor, 选择 **MSIIS** 服务并创建对 **MSSQL** service的依赖关系。

   .. figure:: images/services.png

定义 Package Install
++++++++++++++++++++++++

对于以下7个脚本中的 **每个**脚本（对于MSSSQL为3个脚本，对于MSIIS为4个脚本），字段将相同：

- **Type** - Execute
- **Script Type** - PowerShell
- **Credential** - WIN_VM_CRED

.. note::

  如果您使用的是加入域的VM，则在将VM加入域之后，将需要单独的域凭据来执行PowerShell脚本。

#. 选择 **MSSQL** 服务 在 **Configuration Panel**打开**Package**。

#. 为软件包命名，然后单击**Configure install**以开始添加安装任务。

   您将添加多个脚本来完成每个安装。 使用多个脚本可以使用Calm **Task Library**简化跨多个服务或蓝图的代码维护和应用。 任务库允许您创建模块化脚本来实现某些常用功能，例如加入域或配置常用OS设置。

#. 在 **MSSQL > Package Install**下, 点击 **+ Task** 并填写以下字段：
   - **Task Name** - InitializeDisk1
   - **Script** -

   .. code-block:: powershell

     Get-Disk -Number 1 | Initialize-Disk -ErrorAction SilentlyContinue
     New-Partition -DiskNumber 1 -UseMaximumSize -AssignDriveLetter -ErrorAction SilentlyContinue | Format-Volume -Confirm:$false

   上面的脚本仅执行在服务的VM配置期间添加的额外100GB VDisk的初始化和格式。

#. 点击 **Publish To Library > Publish** 将此任务脚本保存到任务库中以备将来使用。

#. 重复点击 **+ Task** 添加其余两个脚本：

   - **Task Name** - InstallMSSQL
   - **Script** -

   .. code-block:: powershell

     $DriveLetter = $(Get-Partition -DiskNumber 1 -PartitionNumber 2 | select DriveLetter -ExpandProperty DriveLetter)
     $edition = "Standard"
     $HOSTNAME=$(hostname)
     $PackageName = "MsSqlServer2014Standard"
     $Prerequisites = "Net-Framework-Core"
     $silentArgs = "/IACCEPTSQLSERVERLICENSETERMS /Q /ACTION=install /FEATURES=SQLENGINE,SSMS,ADV_SSMS,CONN,IS,BC,SDK,BOL /SECURITYMODE=sql /SAPWD=`"@@{SQL_CRED.secret}@@`" /ASSYSADMINACCOUNTS=`"@@{SQL_CRED.username}@@`" /SQLSYSADMINACCOUNTS=`"@@{SQL_CRED.username}@@`" /INSTANCEID=MSSQLSERVER /INSTANCENAME=MSSQLSERVER /UPDATEENABLED=False /INDICATEPROGRESS /TCPENABLED=1 /INSTALLSQLDATADIR=`"${DriveLetter}:\Microsoft SQL Server`""
     $setupDriveLetter = "D:"
     $setupPath = "$setupDriveLetter\setup.exe"
     $validExitCodes = @(0)

     if ($Prerequisites){
     Install-WindowsFeature -IncludeAllSubFeature -ErrorAction Stop $Prerequisites
     }

     Write-Output "Installing $PackageName...."

     $install = Start-Process -FilePath $setupPath -ArgumentList $silentArgs -Wait -NoNewWindow -PassThru
     $install.WaitForExit()

     $exitCode = $install.ExitCode
     $install.Dispose()

     Write-Output "Command [`"$setupPath`" $silentArgs] exited with `'$exitCode`'."
     if ($validExitCodes -notcontains $exitCode) {
     Write-Output "Running [`"$setupPath`" $silentArgs] was not successful. Exit code was '$exitCode'. See log for possible error messages."
     exit 1
     }

   查看上面的脚本，您可以看到它正在执行SQL Server的自动安装，使用SQL_CRED凭据详细信息，并使用额外的100GB VDisk存放SQL数据文件。

   根据Nutanix生产数据库部署的最佳做法，还需要在VM /安装中添加哪些内容？

   - **Task Name** - FirewallRules
   - **Script** -

   .. code-block:: powershell

     New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433 -Action allow
     New-NetFirewallRule -DisplayName "SQL Admin Connection" -Direction Inbound -Protocol TCP -LocalPort 1434 -Action allow
     New-NetFirewallRule -DisplayName "SQL Database Management" -Direction Inbound -Protocol UDP -LocalPort 1434 -Action allow
     New-NetFirewallRule -DisplayName "SQL Service Broker" -Direction Inbound -Protocol TCP -LocalPort 4022 -Action allow
     New-NetFirewallRule -DisplayName "SQL Debugger/RPC" -Direction Inbound -Protocol TCP -LocalPort 135 -Action allow
     New-NetFirewallRule -DisplayName "SQL Browser" -Direction Inbound -Protocol TCP -LocalPort 2382 -Action allow

   查看上面的脚本，您可以看到它允许通过Windows防火墙进行关键SQL服务的入站访问。

    完成后，您的MSSQL服务应如下所示：

   .. figure:: images/mssql_package_install.png

#. 选择 **MSIIS**服务，然后在 **Configuration Panel**中打开 **Package**选项卡。

#. 为软件包命名，然后单击 **Configure install**以开始添加安装任务。

#. 在 **MSSQL > Package Install**下, 单击 **+ Task**.

#. 与安装MSSQL服务的第一步类似，您将需要初始化并格式化其他100GB VDisk。 单击而不是为此任务手动指定相同的脚本，请单击 **Browse Library**.

#. 选择 **InitializeDisk1** 您先前发布的任务，然后单击 **Select > Copy**.

   .. figure:: images/task_library.png

   .. note::

     如果发布的任务中存在Calm宏，则任务库还使您能够提供变量定义。

#. 指定 **Name** 和 **Credential**, 然后重复点击 **+ Task** 添加其余三个脚本：

   - **Task Name** - InstallWebPI
   - **Script** -

   .. code-block:: powershell

     # Install WPI
     New-Item c:/msi -Type Directory
     Invoke-WebRequest 'http://download.microsoft.com/download/C/F/F/CFF3A0B8-99D4-41A2-AE1A-496C08BEB904/WebPlatformInstaller_amd64_en-US.msi' -OutFile c:/msi/WebPlatformInstaller_amd64_en-US.msi
     Start-Process 'c:/msi/WebPlatformInstaller_amd64_en-US.msi' '/qn' -PassThru | Wait-Process
     cd 'C:/Program Files/Microsoft/Web Platform Installer'; .\WebpiCmd.exe /Install /Products:'UrlRewrite2,ARRv3_0' /AcceptEULA /Log:c:/msi/WebpiCmd.log

   上面的脚本将安装Microsoft Web Platform Installer（WebPI），该WebPI用于下载，安装和更新Microsoft Web Platform的组件，包括Internet信息服务（IIS），IIS媒体平台技术，SQL Server Express，.NET Framework 和Visual Web Developer。

   - **Task Name** - InstallNetFeatures
   - **Script** -

   .. code-block:: powershell

     # Enable Repair via Windows Update
     $servicing = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Servicing"
     New-Item -Path $servicing -Force
     Set-ItemProperty -Path $servicing -Name RepairContentServerSource -Value 2

     # Install Features
     Install-WindowsFeature -Name NET-Framework-Core
     Install-WindowsFeature -Name NET-WCF-Services45 -IncludeAllSubFeature

   上面的脚本在VM上安装.NET Framework 4.5。

   - **Task Name** - InstallBugNetApp
   - **Script** -

   .. code-block:: powershell

     # Create the installation configuration file
     $configFile = "AppPath[@]Default Web Site/bugnet
     DbServer[@]@@{MSSQL.address}@@
     DbName[@]@@{DbName}@@
     DbUsername[@]@@{DbUsername}@@
     Database Password[@]@@{DbPassword}@@
     DbAdminUsername[@]sa
     DbAdminPassword[@]@@{SQL_CRED.secret}@@"

     echo $configFile >> BugNET0.app

     # Install the application via Web PI
     WebpiCmd-x64.exe /Install /UseRemoteDatabase /Application:BugNET@BugNET0.app /AcceptEula

   上面的脚本使用您在练习开始时定义的Application Profile变量来填充Bug Tracker应用程序的配置文件。 然后，它利用WebPI从以下位置安装应用程序： `Microsoft Web App Gallery <https://webgallery.microsoft.com/gallery>`_. 只需进行最小的更改，您就可以利用Gallery中许多受欢迎的应用程序，包括CMS，电子商务，Wiki，票务等应用程序。

   完成后，您的MSIIS服务应如下所示：

   .. figure:: images/msiis_package_install.png

#. 单击 **Save**.

运行蓝图
+++++++++++++++++++++++

#. 在蓝图编辑器的上方工具栏中，单击 **Launch**.

#. 指定唯一 **Application Name** (e.g. *Initials*\ -BugNET) and your **User_initials** Runtime为VM命名的变量值。

#. 单击 **Create**.

   **Audit** 选项卡可用于监视应用程序的部署。 该应用程序大约需要20分钟才能部署。

#. 一旦“创建”操作完成，并且应用程序处于 **Running**状态，请在新选项卡中打开 **BugNET**链接。

   .. figure:: images/bugnet_link.png

#. 系统将显示 **Installation Status Report**页面。 等待它报告“安装完成” **Installation Complete**，然后单击底部的链接以访问该应用程序。
   .. figure:: images/bugnet_setup.png

  恭喜！ 现在，您有了一个功能齐全的错误跟踪应用程序，可以利用Microsoft SQL Server和IIS自动进行配置。

   .. figure:: images/bugnet_app.png

(Optional) Scale Out IIS Tier
+++++++++++++++++++++++++++++

Leveraging the same approach from the :ref:`calm_linux` lab of having multiple web server replicas, can you add a CentOS based HAProxy service to this blueprint to allow for load balancing across multiple IIS servers?

(Optional) 通过Era管理 MSSQL 
++++++++++++++++++++++++++++++++++

完成 :ref:`Era` 实验室，对Era的功能和操作有基本的了解。

使用默认凭据（ ** admin / password **）登录到BugNET应用程序，然后按照向导创建一个新项目。

您刚刚部署了生产BugNET应用程序，现在希望使用最新的可用生产数据快速部署多个开发/测试实例。

您是否可以构建一个利用SQL Server数据库的Era克隆版本的蓝图？

 **提示**

- 首先克隆您现有的蓝图！
- 在Era中注册SQL Server源数据库时，此部署使用默认的MSSQLServer实例名称。 您可以使用Windows身份验证通过WIN_VM_CRED凭据访问SQL Server实例。
- 在“静默”中添加服务时，其中一种“云”类型使用的是“现有VM”。 现有的VM仅需要VM的IP地址和登录凭据。
- 克隆时，Windows Server 2012 R2 VM的Windows许可证密钥为``W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9''。
- 您可以使用半自动方法，其中对克隆的数据库IP使用** Runtime **变量。 在这种情况下，您将创建源数据库的克隆，等待它返回IP地址，并在运行时为蓝图提供指定的IP。
- 您可以使用完全自动化的方法，在其中为“现有VM”创建“软件包安装任务”。 该任务可以执行对Era的API调用以启动数据库克隆操作并返回IP地址。

-不要忘记依赖项！

概要总结
+++++++++

-Calm为Windows工作负载提供了与Linux工作负载相同的应用程序部署和生命周期管理优势。

-Calm可以在Windows终结点上本地执行远程PowerShell脚本，而无需基于Windows的代理。

.. |projects| image:: images/projects.png
