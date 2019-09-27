.. _calm_linux:

---------------------
Calm: Linux 工作负载
---------------------

*完成本实验的估计时间为60分钟。*

简介
++++++++

Nutanix Calm允许您跨私有云和公共云基础架构无缝选择、供应和管理业务应用程序。 Nutanix Calm提供应用程序生命周期，监视和修复，以管理您的异构基础架构，例如VM或裸机服务器。 Nutanix Calm支持多个平台，因此您可以使用单个自助服务和自动化界面来管理所有基础架构。

**在本实验中，您将通过构建和部署蓝图来探索Nutanix Calm的基础知识，该蓝图使用MySQL，nginix和HAProxy安装和配置多层Task Manager Web应用程序。**

创建蓝图
++++++++++++++++++++

蓝图是使用Nutanix Calm建模的每个应用程序的框架。 蓝图是模板，描述了在已创建的服务和应用程序上置备，配置和执行任务所需的所有步骤。 您可以创建一个代表您的应用程序体系结构的蓝图，然后重复运行该蓝图以创建实例，配置和启动您的应用程序。 蓝图还定义了应用程序及其底层基础结构的生命周期，从创建应用程序到对蓝图执行的操作直到应用程序终止为止。

您可以使用蓝图对各种复杂的应用程序进行建模。 从简单地配置单个虚拟机到配置和管理多节点，多层应用程序。

#. 在 **Prism Central**, 选择 :fa:`bars` **> Services > Calm**.

   .. figure:: images/1.png

#. 在左手工具栏选择 |blueprints| **Blueprints** 查看和管理 Calm 蓝图。

   .. note::

     将鼠标悬停在图标上将显示其标题。

#. 单击 **+ Create Blueprint > Multi VM/Pod Blueprint**.

#. 填写以下字段：

   - **Name** - *Initials*-CalmLinuxIntro
   - **Description** - [Task Manager Application](\http://@@{HAProxy.address}@@/)
   - **Project** - *Initials*-Calm

   .. figure:: images/2.png

#. 单击 **Proceed** 打开蓝图编辑器

   蓝图编辑器提供了各种组件的图形表示，使您可以在环境中可视化和配置组件及其依赖性。

创建凭证
++++++++++++++++++++

首先，您将创建一个证书，该证书将用于对最终将部署的CentOS VM进行Calm身份验证。 凭证对于每个蓝图都是唯一的，出于安全目的，**不会**将其作为蓝图的一部分导出。 每个蓝图至少需要1个凭证。

本练习使用“通用云” CentOS映像。 这是多个流行的Linux发行版的通用选项，这些发行版轻巧，支持基于Cloud-Init的配置并利用 `SSH keypair authentication <https://www.ssh.com/ssh/public-key-authentication>`_ 而不是密码。 基于密钥对的身份验证在所有公共云环境中都很普遍。

#. 单击 **Credentials**.

   .. figure:: images/3.png

#. 单击 **Credentials** :fa:`plus-circle` 并填写以下字段：

   - **Credential Name** - CENTOS
   - **Username** - centos
   - **Secret Type** - SSH Private Key
   - **Key** - Paste in your own private key, or use:

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

   .. figure:: images/4.png

#. 单击 **Save**, 然后 **Back**.

定义变量
++++++++++++++++++

变量允许蓝图的可扩展性，这意味着单个蓝图可以根据其变量的配置用于多种用途和环境。
变量可以是保存为蓝图一部分的静态值，也可以在**Runtime** （启动蓝图）时指定。变量特定于给定的**Application Profile**，这是将在其上部署蓝图的平台。例如，能够同时部署到AHV和AWS的蓝图将具有2个应用程序配置文件。每个配置文件可以具有单独的变量和VM配置。

默认情况下，变量存储为** String **，并且在“配置”窗格中可见。将变量设置为**Secret**将掩盖该值，并且非常适合诸如密码之类的变量。除了String和Secret选项外，还有Integer，Multi-line String，Date, Time, and Date Time **Data Types** 和更高级的**“输入类型” **，但是这些内容不在此范围之内。实验室。

可以在使用** @@ {variable_name} @@ **结构针对对象执行的脚本中使用变量。 Calm将展开并使用适当的值替换该变量，然后再发送到VM。

#. 在Blueprint Editor右边的 **Configuration Pane** ，在 **Variables**下面, 添加下面变量 (**Runtime** 通过切换 **Running Man** 标识到蓝色来指定):

   +------------------------+-------------------------------+------------+-------------+
   | **Variable Name**      | **Data Type** | **Value**     | **Secret** | **Runtime** |
   +------------------------+-------------------------------+------------+-------------+
   | User_initials          | String        | xyz           |            |      X      |
   +------------------------+-------------------------------+------------+-------------+
   | Mysql\_user            | String        | root          |            |             |
   +------------------------+-------------------------------+------------+-------------+
   | Mysql\_password        | String        | nutanix/4u    |     X      |             |
   +------------------------+-------------------------------+------------+-------------+
   | Database\_name         | String        | homestead     |            |             |
   +------------------------+-------------------------------+------------+-------------+

   .. figure:: images/5.png

#. 单击 **Save**.

添加可下载的图像
+++++++++++++++++++++++++++

可以基于磁盘映像部署AHV中的VM。 使用Calm，您可以通过URI选择可下载图像。 在应用程序部署期间，Prism Central将自动下载并创建指定的映像。 如果群集上已经存在具有相同URI的图像，它将跳过下载并改用本地图像。

#. 在顶部工具栏中，单击**Configuration > Downloadable Image Configuration** :fa:`plus-circle` 并填写以下字段：

   - **Package Name** - CentOS_7_Cloud
   - **Description** - CentOS 7 Cloud Image
   - **Image Name** - CentOS_7_Cloud
   - **Image Type** - Disk Image
   - **Architecture** - X86_64
   - **Source URI** - http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2
   - **Product Name** - CentOS
   - **Product Version** - 7

   .. note::

      此通用云镜像（Generic Cloud image）与大多数Nutanix预播应用程序蓝图使用的映像相同。

   .. figure:: images/6.png

#. 单击 **Save**, 之后 **Back**.

创建Service
+++++++++++++++++

Services 是虚拟机实例，现有计算机或裸机，您可以使用Nutanix Calm进行配置和配置。

在本练习中，您将创建组成应用程序的数据库，Web服务器和负载平衡器服务。

创建数据库服务
.............................

#. 在 **Application Overview > Services**, 单击 :fa:`plus-circle` 增加新的 Service.

   默认情况下，“应用程序概述”位于蓝图编辑器的右下角，用于创建和管理蓝图层，例如服务，应用程序配置文件和操作。

   .. figure:: images/7.png

   注意 **Service1** 出现在 **Workspace** 并且 **Configuration Pane** 反映所选服务的配置。

#. 填写以下字段：

   - **Service Name** - MySQL
   - **Name** - MySQLAHV

   .. note::
      这定义了Calm中基底的名称。 名称只能包含字母数字字符，空格和下划线。

   - **Cloud** - Nutanix
   - **OS** - Linux
   - **VM Name** - @@{User_initials}@@-MYSQL-@@{calm_array_index}@@-@@{calm_time}@@

   .. note::

     Runtime是将使用您先前提供的变量 **User_initials** ，用于在VM名称前加上首字母缩写。 它还将使用内置宏来提供数组索引（用于横向扩展服务）和时间戳。

   - **Image** - CentOS_7_Cloud
   - **Device Type** - Disk
   - **Device Bus** - SCSI
   - Select **Bootable**
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory (GiB)** - 4
   - Select **Guest Customization**

     - **Type** - Cloud-init
     - **Script** -

       .. code-block:: bash

         #cloud-config
         users:
           - name: centos
             ssh-authorized-keys:
               - @@{CENTOS.public_key}@@
             sudo: ['ALL=(ALL) NOPASSWD:ALL']

       .. note::

         使用SSH私钥凭据时，Calm可以将该私钥解码为匹配的公钥，并通过@@ {Credential_Name.public_key} @@宏访问已解码的值。 然后利用Cloud-Init将SSH公钥值填充为授权密钥，从而允许使用相应的私钥向主机进行身份验证。

   - 选择 **Network Adapters (NICs)**下面的 :fa:`plus-circle` 
   - **NIC 1** - Primary
   - **Credential** - CENTOS

#. 单击 **Save**.

   .. note::

    如果在保存蓝图后出现错误或警告，请将鼠标悬停在顶部工具栏中的图标上，以查看问题列表。 解决所有问题，然后再次**保存**蓝图。

     .. figure:: images/8.png

   现在，您已经完成了与服务关联的VM的部署详细信息，下一步是告诉Calm如何在VM上安装应用程序。

#. 在Workspace窗格中选择**MySQL**服务图标后，滚动到**Configuration Panel**的顶部，然后选择**Package**选项卡。

    软件包是在服务上安装的配置和应用程序，通常是通过在服务VM上执行脚本来完成的。

#. 填写 **MySQL_PACKAGE** 作为 **Package Name** 并点击 **Configure install**.

   - **Package Name** - MYSQL_PACKAGE

   .. figure:: images/9.png

   请注意，在Workspace窗格MySQL服务上出现的**Package install**字段。

#. 选择 **+ Task**, 并填写以下字段 **Configuration Panel** 以定义Calm将在MySQL Service VM上远程执行的脚本：

   - **Task Name** - Install_sql
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo yum install -y "http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm"
       sudo yum update -y
       sudo setenforce 0
       sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
       sudo systemctl stop firewalld || true
       sudo systemctl disable firewalld || true
       sudo yum install -y mysql-community-server.x86_64

       sudo /bin/systemctl start mysqld
       sudo /bin/systemctl enable mysqld

       #Mysql secure installation
       mysql -u root<<-EOF

       UPDATE mysql.user SET Password=PASSWORD('@@{Mysql_password}@@') WHERE User='@@{Mysql_user}@@';
       DELETE FROM mysql.user WHERE User='@@{Mysql_user}@@' AND Host NOT IN ('localhost', '127.0.0.1', '::1');
       DELETE FROM mysql.user WHERE User='';
       DELETE FROM mysql.db WHERE Db='test' OR Db='test\_%';

       FLUSH PRIVILEGES;
       EOF

       mysql -u @@{Mysql_user}@@ -p@@{Mysql_password}@@ <<-EOF
       CREATE DATABASE @@{Database_name}@@;
       GRANT ALL PRIVILEGES ON homestead.* TO '@@{Database_name}@@'@'%' identified by 'secret';

       FLUSH PRIVILEGES;
       EOF

   .. figure:: images/10.png

   .. note::
      您可以单击脚本字段上的** Pop Out **图标以获得更大的窗口，以查看/编辑脚本。

   查看脚本，您将看到该软件包将安装MySQL，配置凭据并根据练习中指定的变量创建数据库。

#.  再次在“工作区”窗格中选择 **MySQL**服务图标，然后在 **Configuration Panel**中选择**Package**选项卡。

#.  单击 **Configure uninstall**.

#.  单击 **+ Task**, 并填写以下字段 **Configuration Panel**:

   - **Task Name** - Uninstall_sql
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       echo "Goodbye!"

   .. figure:: images/11.png

   .. note::
      卸载脚本可用于删除程序包，更新网络服务（如DHCP和DNS），从Active Directory中删除条目等。此简单示例未使用该脚本。

#. 单击 **Save**. 如果存在验证问题（例如缺少字段或不可接受的字符），系统将提示您特定的错误。

创建Web服务器服务
................................

现在，您将按照类似的步骤定义Web服务器服务。

#. 在 **Application Overview > Services**, 添加其他服务。

#. 选择新服务，然后在“**Configuration Panel**中填写以下**VM** 字段： 

   - **Service Name** - WebServer
   - **Name** - WebServerAHV
   - **Cloud** - Nutanix
   - **OS** - Linux
   - **VM Name** - @@{User_initials}@@-WebServer-@@{calm_array_index}@@
   - **Image** - CentOS_7_Cloud
   - **Device Type** - Disk
   - **Device Bus** - SCSI
   - 选择 **Bootable**
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory (GiB)** - 4
   - 选择 **Guest Customization**

     - **Type** - Cloud-init
     - **Script** -

       .. code-block:: bash

         #cloud-config
         users:
           - name: centos
             ssh-authorized-keys:
               - @@{CENTOS.public_key}@@
             sudo: ['ALL=(ALL) NOPASSWD:ALL']

   - 选择**Network Adapters (NICs)**下面的 :fa:`plus-circle`  
   - **NIC 1** - Primary
   - **Credential** - CENTOS

#. 选择 **Package** 选项卡。

#. 填写 **Package Name** 并单击 **Configure install**.

   - **Package Name** - WebServer_PACKAGE

#. 选择 **+ Task**, 并填写以下字段 **Configuration Panel**:

   - **Name Task** - Install_WebServer
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo yum update -y
       sudo yum -y install epel-release
       sudo setenforce 0
       sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
       sudo systemctl stop firewalld || true
       sudo systemctl disable firewalld || true
       sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
       sudo yum update -y
       sudo yum install -y nginx php56w-fpm php56w-cli php56w-mcrypt php56w-mysql php56w-mbstring php56w-dom git unzip

       sudo mkdir -p /var/www/laravel
       echo "server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;
       root /var/www/laravel/public/;
        index index.php index.html index.htm;
       location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
        }
        # pass the PHP scripts to FastCGI server listening on /var/run/php5-fpm.sock
        location ~ \.php$ {
        try_files \$uri /index.php =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)\$;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
        include fastcgi_params;
        }
       }" | sudo tee /etc/nginx/conf.d/laravel.conf
       sudo sed -i 's/80 default_server/80/g' /etc/nginx/nginx.conf
       if `grep "cgi.fix_pathinfo" /etc/php.ini` ; then
        sudo sed -i 's/cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php.ini
       else
        sudo sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php.ini
       fi

       sudo systemctl enable php-fpm
       sudo systemctl enable nginx
       sudo systemctl restart php-fpm
       sudo systemctl restart nginx

       if [ ! -e /usr/local/bin/composer ]
       then
        curl -sS https://getcomposer.org/installer | php
        sudo mv composer.phar /usr/local/bin/composer
        sudo chmod +x /usr/local/bin/composer
       fi

       sudo git clone https://github.com/ideadevice/quickstart-basic.git /var/www/laravel
       sudo sed -i 's/DB_HOST=.*/DB_HOST=@@{MySQL.address}@@/' /var/www/laravel/.env

       sudo su - -c "cd /var/www/laravel; composer install"
       if [ "@@{calm_array_index}@@" == "0" ]; then
        sudo su - -c "cd /var/www/laravel; php artisan migrate"
       fi

       sudo chown -R nginx:nginx /var/www/laravel
       sudo chmod -R 777 /var/www/laravel/
       sudo systemctl restart nginx

   此脚本将安装PHP和Nginx来创建Web服务器，然后创建基于Laravel的Web应用程序。
    然后，它配置Web应用程序设置，包括使用通过** @@ {MySQL.address} @@ **宏访问的MySQL IP地址更新** DB_HOST **。

#. 选择 **Package** 选项卡并单击 **Configure uninstall**.

#. 选择 **+ Task**, 在 **Configuration Panel**填写以下字段:

   - **Name Task** - Uninstall_WebServer
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo rm -rf /var/www/laravel
       sudo yum erase -y nginx

   对于许多应用程序，通常需要扩展给定的服务（例如Web层）以处理更多并发用户。 借助Calm，可以轻松地部署包含给定服务的多个副本的阵列。

#. 在Workspace窗格中选择**WebServer**服务图标后，滚动到**Configuration Panel**的顶部，然后选择“服务”选项卡。With the **WebServer** service icon selected in the Workspace pane, scroll to the top of the **Configuration Panel**, and select the **Service** tab.

#. Under **Deployment Config > Number of Replicas**, increase the **Min** value from 1 to 2 and the **Max** value from 1 to 4.

   .. figure:: images/12.png

   This change will provision a minimum of 2 WebServer VMs for each deployment of the application, and allow the array to grow up to a total of 4 WebServer VMs.

   .. note::

     Scaling an application in and out will require additional scripting so that the application understands how to leverage the additional VMs.

#. Click **Save**.

.. _haproxyinstall:

Creating the Load Balancer Service
..................................

To take advantage of a scale out web tier, your application needs to be able to load balance connections across multiple web server VMs. HAProxy is a free, open source TCP/HTTP load balancer used to distribute workloads across multiple servers. It can be used anywhere from small, simple deployments to large web-scale environments such as GitHub, Instagram, and Twitter.

#. In **Application Overview > Services**, add an additional service.

#. Select the new service and fill out the following **VM** fields in the **Configuration Panel**:

   - **Service Name** - HAProxy
   - **Name** - HAProxyAHV
   - **Cloud** - Nutanix
   - **OS** - Linux
   - **VM Name** - @@{User_initials}@@-HAProxy-@@{calm_array_index}@@
   - **Image** - CentOS\_7\_Cloud
   - **Device Type** - Disk
   - **Device Bus** - SCSI
   - Select **Bootable**
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory (GiB)** - 4
   - Select **Guest Customization**

     - **Type** - Cloud-init
     - **Script** -

       .. code-block:: bash

         #cloud-config
         users:
           - name: centos
             ssh-authorized-keys:
               - @@{CENTOS.public_key}@@
             sudo: ['ALL=(ALL) NOPASSWD:ALL']

   - Select :fa:`plus-circle` under **Network Adapters (NICs)**
   - **NIC 1** - Primary
   - **Credential** - CENTOS

#. Select the **Package** tab.

#. Specify a **Package Name** and click **Configure install**.

   - **Package Name** - HAProxy_PACKAGE

#. Select **+ Task**, and fill out the following fields in the **Configuration Panel**:

   - **Name Task** - Install_HAProxy
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo yum update -y
       sudo yum install -y haproxy
       sudo setenforce 0
       sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
       sudo systemctl stop firewalld || true
       sudo systemctl disable firewalld || true

       echo "global
        log 127.0.0.1 local0
        log 127.0.0.1 local1 notice
        maxconn 4096
        quiet
        user haproxy
        group haproxy
       defaults
        log global
        mode http
        retries 3
        timeout client 50s
        timeout connect 5s
        timeout server 50s
        option dontlognull
        option httplog
        option redispatch
        balance roundrobin
       # Set up application listeners here.
       listen admin
        bind 127.0.0.1:22002
        mode http
        stats uri /
       frontend http
        maxconn 2000
        bind 0.0.0.0:80
        default_backend servers-http
       backend servers-http" | sudo tee /etc/haproxy/haproxy.cfg

       hosts=$(echo "@@{WebServer.address}@@" | tr "," "\n")
       port=80

       for host in $hosts
         do echo " server host-${host} ${host}:${port} weight 1 maxconn 100 check" | sudo tee -a /etc/haproxy/haproxy.cfg
       done

       sudo systemctl daemon-reload
       sudo systemctl enable haproxy
       sudo systemctl restart haproxy

   Note the use of the @@{WebServer.address}@@ macro in the script above. The macro returns a comma delimited list of all IPs of the VMs within that service. The script then uses the `tr <https://www.geeksforgeeks.org/tr-command-unixlinux-examples/>`_ command to replace commas with carriage returns. The result is an array, **$hosts**, containing strings of all WebServer IP addresses. Those addresses are then each added to the **HAProxy** configuration file.

#. Select the **Package** tab and click **Configure uninstall**.

#. Select **+ Task**, and fill out the following fields in the **Configuration Panel**:

   - **Name Task** - Uninstall_HAProxy
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo
       yum -y erase haproxy

#. Click **Save**.

Adding Dependencies
+++++++++++++++++++

As our application will require the database to be running before the web server starts, our Blueprint requires a dependency to enforce this ordering.  There are a couple of ways to do this, one of which you've already done without likely realizing it.

#. In the **Application Overview > Application Profile** section, expand the **Default** Application Profile and click the **Create** Action.

   .. figure:: images/13.png

   Take note of the **Orange Orchestration Edge** going from the **MySQL Start** task to the **WebServer Package Install** task. This edge was automatically created by Calm due to the **@@{MySQL.address}@@** macro reference in the **WebServer Package Install** task. Since the system needs to know the IP Address of the MySQL service prior to being able to proceed with the WebServer Install task, Calm intelligently creates the orchestration edge for you. This requires the MySQL service to be started prior to moving on to the WebServer Install task.

#. Return to the **HAProxy Package Install** task, why are orchestration edges automatically created between the WebServer and HAProxy services?

#. Next, select the **Stop** Profile Action.

   Note that lack of orchestration edges between services when stopping an application. Why might issuing shutdown commands to all services within the application simultaneously create an issue?

#. Click on each Profile Action to take note of the current presence (or lack thereof) of the orchestration edges.

   .. figure:: images/14.png

   To resolve this, you'll manually define a dependencies between services.

#. Select the **WebServer** Service and click the **Create Dependency** icon that appears above the Service icon, and then click on the **MySQL** service.

   .. figure:: images/15.png

#. This represents that the **WebServer** service "depends" upon the **MySQL** service, meaning the **MySQL** service will start before, and stop after, the **WebServer** service.

#. Now create a dependency for the **HAProxy** service to depend on the **WebServer** service.

#. Click **Save**.

#. Re-visit the Profile Actions and confirm the edges now properly reflect the dependencies between the services, as shown below:

   .. figure:: images/16.png

   Drawing the white dependency arrows will cause Calm to create orchestration edges for all **System Defined Profile Actions** (Create, Start, Restart, Stop, Delete, and Soft Delete).

Launching and Managing the Application
++++++++++++++++++++++++++++++++++++++

#. From the upper toolbar in the Blueprint Editor, click **Launch**.

#. Specify a unique **Application Name** (e.g. *Initials*\ -CalmLinuxIntro1) and your **User_initials** Runtime variable value for VM naming.

#. Click **Create**.

   The **Audit** tab can be used to monitor the deployment of the application.

   Why don't all of the CentOS based services deploy at the same time following the download of the disk image?

#. Once the application reaches a **Running** status, navigate to the **Services** tab and select the **HAProxy** service to determine the IP address of your load balancer.

#. In a new browser tab or window, navigate to \http://<HAProxy-IP>, and verify your Task Manager application is functioning.

   .. note::

     You can also click the link in the Description of the Application.

   .. figure:: images/17.png

Takeaways
+++++++++

您应该了解** Nutanix Calm **的关键要点是什么？

-Nutanix Calm作为Prism的本机组件，建立在该平台上并发扬光大。 Acropolis提供的简单性使Calm专注于应用程序，而不是试图掩盖基础架构管理的复杂性。

-Calm蓝图易于使用。在60分钟内，您从零开始进行了完整的基础架构堆栈部署。由于Calm使用标准工具（bash，PowerShell，Python等）进行配置，因此无需学习任何新语言，因此您可以立即应用已有的技能和代码。

-尽管视觉效果不佳，但即使是单个VM蓝图也会对客户产生巨大影响。印度的一家银行正在将Calm用于单VM部署，从而将这些应用程序的部署时间从3天减少到2小时。请记住，当今许多客户很少或根本没有自动化（或者他们拥有的自动化非常复杂/难以理解，因此限制了它的采用）。这意味着Calm可以立即，立即，即时地为他们提供帮助。

-“多云应用程序自动化和生命周期管理”听起来很吓人。 “未来”听起来很棒，但是许多操作员看不到通往那里的道路。聆听客户今天所苦恼的事情（备份需要专业技能，VM部署需要很长时间，升级很困难），并讲解Calm如何提供帮助；跳到多云自动化的故事，将Calm从“我现在需要这个”推到“一旦事情平静下来，让我们稍后再评估一下”（而且事情永远不会真正“安静下来”。）

-蓝图编辑器提供了一个简单的UI，用于为可能复杂的应用程序建模。

-蓝图与SSP项目相关，可用于实施配额和基于角色的访问控制。

-具有蓝图安装和配置二进制文件意味着不再为单个应用程序创建特定的映像。相反，可以通过对蓝图或安装脚本的更改来修改应用程序，这两种方法都可以存储在源代码存储库中。

-变量允许自定义应用程序的另一个维度，而无需编辑基础蓝图。

-有多种对VM进行身份验证的方法（密钥或密码），具体取决于源映像。

-可以实时监视应用程序状态。

-应用程序通常跨多个VM，每个VM负责不同的服务。 Calm能够自动化和协调完整的应用程序。

-服务之间的依赖关系可以在蓝图编辑器中轻松建模。

-用户可以快速调配整个应用程序堆栈以进行生产或测试，以获得可重复的结果，而不会浪费时间进行手动配置。

-有兴趣使用Calm进行更多应用生命周期操作吗？看看 :ref:`calm_day2`!


.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/blueprints.png
.. |projects| image:: images/projects.png
