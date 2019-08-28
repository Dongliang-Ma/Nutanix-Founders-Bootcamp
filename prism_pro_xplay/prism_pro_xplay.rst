.. _prism_pro_xplay:

--------------------------------------------
Prism Pro: X-Play
--------------------------------------------

Overview
++++++++



Increase Constrained VM Memory with X-Play
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

In this lab story we will now use X-Play to create a Playbook to automatically add memory to the lab VM that was created earlier, when a memory constraint is detected.

#. Use the search bar to navigate to the **Playbooks** page.

   .. figure:: images/ppro_26.png

#. We will start by creating a Playbook. Click **Create Playbook** at the top of the table view

   .. figure:: images/ppro_27.png

#. Select Alert as a trigger

   .. figure:: images/ppro_28.png

#. Search and select **VM {vm_name} Memory Constrained** as the alert policy, since this is the issue we are looking to take automated steps to remediate.

   .. figure:: images/ppro_29.png

#. Select the *Specify VMs* radio button and choose the VM you created for the lab. This will make it so only alerts raised on your VM will trigger this Playbook.

   .. figure:: images/ppro_29b.png

#. We will first need to snapshot the VM. Click **Add Action** on the left side and select the **VM Snapshot** action.

   .. figure:: images/ppro_30.png

#. The Target VM is auto filled with the source entity from the Alert trigger. To finish filling the details for this action, enter a value, such as **1**, in the Time to Live field.

   .. figure:: images/ppro_32.png

#. Next we would like to remediate the constrained memory by adding more memory to the VM. Click **Add Action** to add the **VM Add Memory** action

   .. figure:: images/ppro_33.png

#. Set the empty fields according to the screen below.

   .. figure:: images/ppro_34.png


#. Next we would like to notify someone that an automated action was taken. Click **Add Action** to add the email action

   .. figure:: images/ppro_35.png

#. Fill in the field in the email action. Here are the examples

**Recipient:** Fill in your email address.

**Subject :**
``Playbook {{playbook.playbook_name}} addressed alert {{trigger[0].alert_entity_info.name}}``

**Message:**
``Prism Pro X-FIT detected  {{trigger[0].alert_entity_info.name}} in {{trigger[0].source_entity_info.name}}.  Prism Pro X-Play has run the playbook of "{{playbook.playbook_name}}". As a result, Prism Pro increased 1GB memory in {{trigger[0].source_entity_info.name}}.``

You are welcome to compose your own subject message. The above is just an example. You could use the “parameters” to enrich the message.

   .. figure:: images/ppro_36.png

#. Click **Add Action** to add the **Acknowledge Alert** action

   .. figure:: images/ppro_37.png

#. Click **Save & Close** button and save it with a name “*Initials* - Auto Increase Constrained VM Memory”. **Be sure to enable the ‘Enabled’ toggle.**

   .. figure:: images/ppro_39.png

#. You should see a new playbook in the “Playbooks” list page.

   .. figure:: images/ppro_40.png

#. Search for your VM and record the current memory capacity. You can scroll down in the properties widget to see the configured memory.

   .. figure:: images/ppro_41.png

#. **Switch tabs back to** the http://10.42.247.70 page and continue to the Story 1-3 Step.

   .. figure:: images/ppro_66.png

#. Now we will simulate an alert for ‘VM Memory Constrained’ which will trigger the Playbook we just created. Click the ‘Simulate Alert’ button to create the alert.

   .. figure:: images/ppro_64.png

#. Go back to Prism page and check your VMs page again, you should now see the memory capacity is increased by 1GB. If the memory does not show updated you can refresh the browser page to speedup the process.

#. You should also receive an email. Check the email to see that its subject and email body have filled the real value for the parameters you set up.

#. Go to the **Playbook** page, click the playbook you just created.

   .. figure:: images/ppro_44.png

#. Click the **Plays** tab, you should see that a play has just completed.

   .. figure:: images/ppro_45.png

#. Click the “Play” to examine the details

   .. figure:: images/ppro_46.png


Using X-Play with 3rd Party API
+++++++++++++++++++++++++++++++++++++++++++++

For this story we will be using Habitica to show how we can use 3rd Party APIs with X-Play. Habitica is a free habit and productivity app that treats your real life like a game. We will be creating a task with Habitica.


#. Use the search bar to navigate to the **Playbooks** page.

   .. figure:: images/ppro_26.png

#. We will start by creating a Playbook. Click **Create Playbook** at the top of the table view

   .. figure:: images/ppro_27.png

#. Use the search bar to navigate to the **Action Gallery** page.

   .. figure:: images/ppro_47.png

#. Click the checkbox next to the item for ‘Rest API’ and then from the actions menu select the ‘Clone’ option.

   .. figure:: images/ppro_48.png

#. We are creating an Action that we can later use in our playbook to create a Task in Habitica. Fill in the following values replacing your name in the <YOUR NAME HERE> part.

**Name:** *Initials* - Create Habitica Task

**Method:** POST

**URL:** https://habitica.com/api/v3/tasks/user

**Request Body:** ``{"text":"*Initials* Check {{trigger[0].source_entity_info.name}}","type":"todo","notes":"VM has been detected as a bully VM and has been temporarily powered off.","priority":2}``

**Request Header:**

| x-api-user:fbc6077f-89a7-46e1-adf0-470ddafc43cf
| x-api-key:c5343abe-707a-4f7c-8f48-63b57f52257b
| Content-Type:application/json;charset=utf-8


   .. figure:: images/ppro_49.png

#. Click the **copy** button to save the action.

#. Navigate back to the Playbooks page using the search bar.

#. Select the **Alert trigger** and search for and select the alert policy **VM Bully {vm_name}**. This is the alert that we would like to act on to handle when the system detects a Bully VM.

   .. figure:: images/ppro_50.png

#. Select the **Specify VMs** radio button and choose the VM you created for the lab. This will make it so only alerts raised on your VM will trigger this Playbook.

   .. figure:: images/ppro_50b.png

#. The first thing we would like to do is Power off the VM, so we can make sure it is not starving other VMs of resources. Click the **Add Action** button and select **Power Off VM**.

   .. figure:: images/ppro_51.png

#. Next we would like to create a task so that we can look into what is causing this VM to be a Bully. Add another Action. This time select the action you created called, Create Habitica Task.

   .. figure:: images/ppro_53.png

#. Add one more action, select the Acknowledge Alert action. Use the parameters for this action to fill in the ‘Alert’ parameter.

   .. figure:: images/ppro_54.png

#. Save & Enable the playbook. You can name it  “*Initials* - Power Off Bully VM for Investigation”. **Be sure to enable the ‘Enabled’ toggle.** Click the Save button.

   .. figure:: images/ppro_55.png

#. **Switch back to the other tab** running http://10.42.247.70 and Simulate the ‘VM Bully Detected’ alert for Story 5.

   .. figure:: images/ppro_65.png

#. Once the alert is successfully simulated, you can check that your Playbook ran, and view the details as before.

   .. figure:: images/ppro_75.png

#. You can verify the Rest API was called for Habitica by logging in from another tab at https://habitica.com using the credentials:

| Username : next19LabUser
| Password: Nutanix.123

And verify your task is created.

   .. figure:: images/ppro_57.png

Takeaways
+++++++++

- X-Play, the IFTTT for the enterprise, is our engine to enable the automation of daily operations tasks.
- X-Play enables admins to confidently automate their daily tasks within minutes.
- X-Play is extensive that can use customer’s existing APIs and scripts as part of its playbooks.
