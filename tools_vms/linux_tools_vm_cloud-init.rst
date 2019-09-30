.. _linux_tools_vm:

---------------
Linux工具VM
---------------

简介
+++++++++

此CentOS VM镜像将与用于支持多个实验室练习的软件包一起暂存。

如果指示这样做，请在 ** Lab Setup **中将此虚拟机部署到您分配的群集上。

.. raw:: html

  <strong><font color="red">Only deploy the VM once, it does not need to be cleaned up as part of any lab completion.</font></strong>

部署 CentOS
++++++++++++++++

在 **Prism Central** > 选择 :fa:`bars` **> Virtual Infrastructure > VMs**, 并点击 **Create VM**.

填写以下字段：

- **Name** - *Initials*-Linux-ToolsVM
- **Description** - (Optional) Description for your VM.
- **vCPU(s)** - 1
- **Number of Cores per vCPU** - 2
- **Memory** - 2 GiB

- 选择 **+ Add New Disk**
    - **Type** - DISK
    - **Operation** - Clone from Image Service
    - **Image** - CentOS7.qcow2
    - 选择 **Add**

- 选择 **Add New NIC**
    - **VLAN Name** - Secondary
    - 选择 **Add**

- 检查 **Custom Script**
    - 选择 **Type or Paste Script**
    - 粘贴以下脚本并点击 **Save**

.. code-block:: bash

  #cloud-config
  users:
    - name: nutanix
      sudo: ['ALL=(ALL) NOPASSWD:ALL']
      ssh-authorized-keys:
        - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCm+7N2tjmJw5jhPmD8MS6urZQJB42ABh73ffGQSJ0XUHgdEDfjUDFkLK0wyJCe0sF5QJnh07UQn0F0BUnBi+VwehPGeODh6S43OP5YS/14L0fyntFI06B9lckx/ygRNu82sHxXCX+6VVUFPOPC+sz6j1DQswKY9d4cEYnaMBGSzqRxrqAIf6aWIKTJTYKPFY0zaUZ6ow2iwS0Nlh5EqaXsEBWkqMmr7/auP9GV/adUgzFrGLJklYBdfH575SIK6/PZL6wNT0jE9LmFlEm7dI01ZWPclBuV16FzRyrnzmWr/ebY62A04vYBtR0vyfEfsW2ZgxgD6aAE6+ytj0v19y0elRtOaeTySN/HlXh7owKWCHnlXNpTUiSDP8SQ8LRARkhQu3KEDL0ppGCrSF87oFkp1gPzf92U+UK3LaNMMjZXMOy0zLoLEdLtbQo6S8iHggDoX4NI4sWWxcX0mtadvjy/nIOvskk9IXasQh0u0MT9ARQY5VXPluKDtEVdeow9UbvgJ1xxNkphUgsWjCiy+sjgapsuZvWqKM6TPT1i24XYaau+/Fa0vhjLb8vCMWrrtkRwGt4re243NDYcYWTzVZUFuUK0w1wqt77KgjCCeyJdsZNwrh15v780Fjqpec3EGVA0xyNbF0jn/tsnYy9jPh/6Cv767EratI97JhUxoB4gXw== no-reply@acme.com
      lock-passwd: false
      passwd: $6$4guEcDvX$HBHMFKXp4x/Eutj0OW5JGC6f1toudbYs.q.WkvXGbUxUTzNcHawKRRwrPehIxSXHVc70jFOp3yb8yZgjGUuET.

  yum_repos:
    epel-release:
      baseurl: http://download.fedoraproject.org/pub/epel/7/$basearch
      enabled: true
      failovermethod: priority
      gpgcheck: true
      gpgkey: http://download.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7
      name: Extra Packages for Enterprise Linux 7 - Release

  package_update: true
  package_upgrade: true

  hostname: centos7-tools-vm

  packages:
    - gcc-c++
    - make
    - unzip
    - bash-completion
    - python-pip
    - s3cmd
    - stress
    - awscli
    - ntp
    - ntpdate
    - nodejs
    - python36
    - python36-setuptools

  runcmd:
    - npm install -g request express
    - systemctl stop firewalld
    - systemctl disable firewalld
    - /sbin/setenforce 0
    - sed -i -e 's/enforcing/disabled/g' /etc/selinux/config
    - /bin/python3.6 -m ensurepip
    - pip install -U pip
    - pip install boto3 python-magic
    - ntpdate -u -s 0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org
    - systemctl restart ntpd

  final_message: CentOS 7 Tools Machine setup successfully!


点击 **Save** 创建VM

开启VM

验证工具安装
++++++++++++++++++++

打开控制台

观看the Cloud-Init 脚本的运行, 一旦看到 **CentOS 7 Tools Machine setup successfully!** 即完成部署。
