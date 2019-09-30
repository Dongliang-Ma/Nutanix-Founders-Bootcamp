.. _prism_pro_xplay:

--------------------------------------------
Prism Pro: X-Play
--------------------------------------------

简介
++++++++



通过X-Play增加受限VM的内存 
+++++++++++++++++++++++++

在本实验中，我们现在将使用X-Play创建一个Playbook，以便在检测到内存不足时自动将内存添加到之前创建的VM中。

#. 使用搜索栏导航到 **Playbooks**页面。

   .. figure:: images/ppro_26.png

#. 我们将从创建Playbook开始。 单击表视图顶部的 **Create Playbook**。

   .. figure:: images/ppro_27.png

#. 选择Alert作为触发器。

   .. figure:: images/ppro_28.png

#. 搜索并选择 **VM {vm_name} Memory Constrained** 作为警报策略，因为这是我们希望采取自动化步骤进行修复的问题。

   .. figure:: images/ppro_29.png

#. 选择 *Special VM* 单选按钮，然后选择您为实验创建的VM。 这将使得只有在您的VM上引发的警报才会触发此Playbook。

   .. figure:: images/ppro_29b.png

#. 我们首先需要对VM进行快照。 单击左侧的 **Add actiion**，然后选择 **VM Snapshot**操作。

   .. figure:: images/ppro_30.png

#. 目标VM从Alert触发器自动填充源实体。 要完成此操作的详细信息填写，请在生存时间字段中输入值，例如**1**。

   .. figure:: images/ppro_32.png

#. 接下来，我们希望通过向VM添加更多内存来修复受约束的内存。 单击 **Add Action**以添加 **VM Add Memory**操作。

   .. figure:: images/ppro_33.png

#. 根据下面的屏幕设置空字段。

   .. figure:: images/ppro_34.png


#. 接下来，我们想通知某人已采取自动操作。 单击 **Add Action**以添加电子邮件操作

   .. figure:: images/ppro_35.png

#. 填写电子邮件操作中的字段。 以下是示例

**Recipient:** 填写您的电子邮件地址。

**Subject :**
``Playbook {{playbook.playbook_name}} addressed alert {{trigger[0].alert_entity_info.name}}``

**Message:**
``Prism Pro X-FIT detected  {{trigger[0].alert_entity_info.name}} in {{trigger[0].source_entity_info.name}}.  Prism Pro X-Play has run the playbook of "{{playbook.playbook_name}}". As a result, Prism Pro increased 1GB memory in {{trigger[0].source_entity_info.name}}.``

欢迎您撰写自己的主题信息。 以上只是一个例子。 您可以使用“参数”来丰富消息。

   .. figure:: images/ppro_36.png

#. 点击 **Add Action** 添加 **Acknowledge Alert** 动作。

   .. figure:: images/ppro_37.png

#. 点击 **Save & Close** 按钮并使用名称“*Initials* - Auto Increase Constrained VM Memory”. **务必启用‘Enabled’ 切换.**

   .. figure:: images/ppro_39.png

#. 你应该在“Playbooks”列表页面看到一个新的Playbook。

   .. figure:: images/ppro_40.png

#. 搜索您的VM并记录当前的内存容量。 您可以在属性小部件中向下滚动以查看配置的内存。

   .. figure:: images/ppro_41.png

#. **Switch tabs back to** the http://10.42.247.70 page and continue to the Story 1-3 Step.

   .. figure:: images/ppro_66.png

#. 现在我们将模拟“VM Memory Constrained”的警报，它将触发我们刚刚创建的Playbook。 单击“Simulate Alert”按钮以创建警报。

   .. figure:: images/ppro_64.png

#. 返回Prism页面并再次检查您的VM页面，您现在应该看到内存容量增加了1GB。 如果内存未显示更新，则可以刷新浏览器页面以加快进程。

#. 您还应该收到一封电子邮件。 查看电子邮件，查看其主题和电子邮件正文已填写您设置的参数的实际值。

#. 转到 **Playbook** 页面，单击刚刚创建的剧本。

   .. figure:: images/ppro_44.png

#. 单击**Plays**选项卡，您应该看到播放刚刚完成。

   .. figure:: images/ppro_45.png

#. 单击“Play”以检查详细信息。

   .. figure:: images/ppro_46.png


X-Play和3rd Party API结合使用
+++++++++++++++++++++++++++++++++++++++++++++

我们将使用Habitica展示如何在X-Play中使用3rd Party API。 Habitica是一款免费的习惯和生产力应用程序，可将您的现实生活像游戏一样对待。 我们将与Habitica共同创建任务。

#. 使用搜索栏导航到**Playbooks**页面。

   .. figure:: images/ppro_26.png

#. 我们将首先创建一个Playbook。 点击表格视图顶部的**Create Playbook**。
   .. figure:: images/ppro_27.png

#. 使用搜索栏导航到 **Action Gallery** 页面。

   .. figure:: images/ppro_47.png

#. 点击“ Rest API”项旁边的复选框，然后从操作菜单中选择“克隆”选项。

   .. figure:: images/ppro_48.png

#. 我们正在创建一个动作，以后可以在我们的剧本中使用它在Habitica中创建任务。 在 <YOUR NAME HERE>部分中填写以下值来替换您的名字。

**Name:** *Initials* - Create Habitica Task

**Method:** POST

**URL:** https://habitica.com/api/v3/tasks/user

**Request Body:** ``{"text":"*Initials* Check {{trigger[0].source_entity_info.name}}","type":"todo","notes":"VM has been detected as a bully VM and has been temporarily powered off.","priority":2}``

**Request Header:**

| x-api-user:fbc6077f-89a7-46e1-adf0-470ddafc43cf
| x-api-key:c5343abe-707a-4f7c-8f48-63b57f52257b
| Content-Type:application/json;charset=utf-8


   .. figure:: images/ppro_49.png

#. 单击 **copy**按钮以保存操作。

#. 使用搜索栏导航回到Playbooks页面。

#. 选择 **Alert trigger**并搜索并选择警报策略**VM Bully {vm_name}**。 这是我们希望在系统检测到Bully VM时采取的警报。

   .. figure:: images/ppro_50.png

#. 选择 **Specify VMs**单选按钮，然后选择您为实验室创建的虚拟机。 这样一来，只有在您的VM上发出的警报才会触发此Playbook。

   .. figure:: images/ppro_50b.png

#. 我们要做的第一件事是关闭虚拟机电源，因此我们可以确保它不会耗尽其他虚拟机的资源。 单击 **Add Action**按钮，然后选择 **Power Off VM**。

   .. figure:: images/ppro_51.png

#. 接下来，我们想创建一个任务，以便我们调查导致此VM成为欺负者的原因。 添加另一个动作。 这次选择您创建的名为“创建Habitica任务”的操作。

   .. figure:: images/ppro_53.png

#. 再添加一个动作，选择“确认警报”动作。 使用此操作的参数来填写“警告”参数。

   .. figure:: images/ppro_54.png

#. 保存并启用playbook。 您可以将其命名为“*Initials* - Power Off Bully VM for Investigation”。 **请确保启用‘Enabled’开关**，点击“保存”按钮。

   .. figure:: images/ppro_55.png

#. **切换回另一个tab** 运行 http://10.42.247.70 并模拟故事5的‘VM Bully Detected’警报。

   .. figure:: images/ppro_65.png

#. 成功模拟警报后，您可以检查Playbook是否已运行，并像以前一样查看详细信息。

   .. figure:: images/ppro_75.png

#. 您可以如下权限通过从以下位置的另一个选项卡登录来验证Rest API是否为Habitica所调用 https://habitica.com:

| Username : next19LabUser
| Password: Nutanix.123

And verify your task is created.

   .. figure:: images/ppro_57.png

概要总结
+++++++++

-X-Play 企业的IFTTT，是我们实现日常操作任务自动化的引擎。
-X-Play 使管理员可以在数分钟内自信地自动化其日常任务。
-X-Play 广泛，可以将客户现有的API和脚本用作其剧本的一部分。
