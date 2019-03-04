.. _calm_escript_blueprint:

------------------------------------------------------
Calm: EScript, Set Variable, and Task Library Tutorial
------------------------------------------------------

Overview
++++++++

Thus far we've utilized Bash and Powershell scripts within Calm blueprints to automate complex application deployments.  While shell scripts are powerful and versitile, there are times a more robust programming language, like Python, are useful.  Python is both extremely powerful, and friendly for beginners due to its easy readability.

You may have already noticed, but Nutanix Calm has a script type called EScript_.  Short for Epsilon Script (Epsilon is the orchestration engine that drives Calm), EScript is a sandboxed Python interpreter.  It contains many commonly used modules for scripting and automation.  In particular, it contains the **requests** module as **urlreq**, which allows Calm to interface with any Rest API.

.. _EScript: https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v250:nuc-supported-escript-modules-functions-c.html

.. note::
   It is sandboxed because the script runs **directly** within the Calm engine, which means it runs directly within Prism Central.  Allowing the user to import unknown and unvetted modules into Prism Central is a security concern.
   Compare this to shell or Powershell scripts, which are copied to and then run directly from the target machine with SFTP.

This lab will also cover two other items related to tasks, Set Variable, which allows for setting the results of a script to a Calm variable to be re-used later in the blueprint, and the Task Library, which allows for task re-use across blueprints.

Blueprint and Credentials Creation
..................................

From **Prism Central > Calm**, select **Blueprints** from the sidebar, and click **+ Create Blueprint > Multi VM/Pod Blueprint**.

.. figure:: images/create_blueprint.png


In the pop-up, fill in the following three fields, and click **Proceed**:

- **Name** - EScript<Initials>
- **Description** - My First EScript Blueprint
- **Project** - Calm

.. figure:: images/name_blueprint.png


Along the top of the blueprint, click on **Credentials**, and then click the blue **+** to create the following credential:

- **Credential Name** - PC_Creds
- **Username** - admin
- **Secret Type** - Password
- **Password** - Type in your Prism Central admin password

.. figure:: images/credentials.png

Click **Back** and then **Save**, and ensure no errors or warnings appear.

Existing Service Creation
........................

In the **Application Overview** pane on the left, click the **+** next to **Service** to create a new Service.  Under the **VM** column, fill in the following fields:

+------------------------------+------------------+
| **Service Name**             | PC               |
+------------------------------+------------------+
| **Name**                     | PrismCentral     |
+------------------------------+------------------+
| **Cloud**                    | Existing Machine |
+------------------------------+------------------+
| **Operating System**         | Linux            |
+------------------------------+------------------+
| **IP Address**               | localhost        |
+------------------------------+------------------+
| **Check log-in upon create** | **Unselect**     |
+------------------------------+------------------+
| **Credential**               | PC_Creds         |
+------------------------------+------------------+
| **Address**                  | Leave default    |
+------------------------------+------------------+
| **Connection Type**          | Leave default    |
+------------------------------+------------------+
| **Connection Port**          | Leave default    |
+------------------------------+------------------+
| **Delay**                    | Leave default    |
+------------------------------+------------------+

.. figure:: images/existing_machine.png


There are several new and interesting items in the above configuration:

- **Cloud** - Instead of creating a new VM on Nutanix or a public cloud provider, we're instead choosing to automate against an already existing machine.  All that's required for us to enter is an IP Address of the machine, which in our case is Prism Central.  Depending on your use case, you may specify something like Ansible Tower, or an Era Server, as an existing machine.
- **IP Address** - Since we're going to be making API Calls against Prism Central, and Calm runs directly in Prism Central, we're just entering localhost as an IP.  If you were going to be automating against something like Ansible Tower or Era, you would need to put your Ansible Tower or Era Server IP addresses in this field, rather than localhost.
- **Check log-in upon create** - This is the first time we've seen this box unselected.  Since EScript tasks run directly within Calm, there's no need to SSH in to the Service in question.  Instead we'll use credentials directly within the EScript code to authenticate our Rest API call.

Click **Save**, and ensure no errors or warnings appear.

RestList Custom Action
......................

In this section, we're going to be creating a custom action for our application.  When this action is later run, it will make a Rest API call against Prism Central.  Specifically, it will be a POST /list call, of a *kind* specified at runtime.  The results of this call will then be outputted.

In the **Application Overview** pane on the left, under the **Default Application Profile**, select the blue **+** next to **Actions**.

In the Action **Configuration Pane** that appears on the right, name the action **RestList**, and add a single variable:

- **Name** - kind
- **Value** - apps
- **Secret** - Unselected
- **Runtime** - Selected

.. figure:: images/restlist.png


When this custom action is later, a pop-up will appear, prompting the user for input.  While **apps** will already be filled in for the user, the field can be easily changed.

In the **Blueprint Canvas**, click the **+ Task** button to add a task to our custom action.  Fill in the following fields:

- **Task Name** - RuntimePost
- **Type** - Execute
- **Script Type** - EScript
- **Script** - Paste in the following script

.. code-block:: python

   # Set the credentials
   pc_user = '@@{PC_Creds.username}@@'
   pc_pass = '@@{PC_Creds.secret}@@'
   
   # Set the headers, url, and payload
   headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
   url     = "https://@@{address}@@:9440/api/nutanix/v3/@@{kind}@@/list"
   payload = {}
   
   # Make the request
   resp = urlreq(url, verb='POST', auth='BASIC', user=pc_user, passwd=pc_pass, params=json.dumps(payload), headers=headers)
   
   # If the request went through correctly, print it out.  Otherwise error out, and print the response.
   if resp.ok:
       print json.dumps(json.loads(resp.content), indent=4)
       exit(0)
   else:
       print "Post request failed", resp.content
       exit(1)

Again, there are some new and interesting features of this task.  Note how there is not a Credential dropdown within the Calm UI, and instead we're setting Python variables equal to our PC_Creds that we specified earlier.  We also see the **urlreq** module being used, which is the exact line that our API call is made.  Depending on how the request went through, we'll print an appropriate message and exit accordingly.

.. figure:: images/runtime_post.png


Click **Save**, and ensure no errors or warnings appear.

GetDefaultSubnet Custom Action
..............................

In this section, we're again going to be creating a custom action.  This time we'll make another Rest API call to get the list of **Projects** on this Prism Central instance.  We'll then parse the output of that API call to get the UUID of the default subnet that's set for the project that the running application belongs to.  This UUID will be set as a Calm variable, allowing for re-use elsewhere in the blueprint.  We'll then do another Rest API call, a GET on the default subnet (utilizing this newly set variable).

Select the **Prism Central** service within the **Blueprint Canvas**, and then in the **Configuration Pane** navigate to the **Service** column.  Add a variable called **SUBNET**, leaving all the other fields blank.

.. figure:: images/subnet_variable.png


In the **Application Overview** pane on the left, under the **Default Application Profile**, select the blue **+** next to **Actions**.

In the Action **Configuration Pane** that appears on the right, name the action **GetDefaultSubnet**.

.. figure:: images/get_default_subnet.png


In the **Blueprint Canvas**, click the **+ Task** button to add a task to our custom action.  Fill in the following fields:

- **Task Name** - GetSubnetUUID
- **Type** - Set Variable
- **Script Type** - EScript
- **Script** - Paste in the script below
- **Output** - SUBNET

.. code-block:: python

   # Get the JWT
   jwt = '@@{calm_jwt}@@'
   
   # Set the headers, url, and payload
   headers = {'Content-Type': 'application/json', 'Accept': 'application/json', 'Authorization': 'Bearer {}'.format(jwt)}
   url     = "https://@@{address}@@:9440/api/nutanix/v3/projects/list"
   payload = {}
   
   # Make the request
   resp = urlreq(url, verb='POST', params=json.dumps(payload), headers=headers, verify=False)
   
   # If the request went through correctly
   if resp.ok:
     
     # Cycle through the project "entities", and check if its name matches the current project
     for project in json.loads(resp.content)['entities']:
       if project['spec']['name'] == '@@{calm_project_name}@@':
   
         # If there's a default subnet reference, print UUID to set variable and exit success, otherwise error out
         if 'uuid' in project['status']['resources']['default_subnet_reference']:
           print "SUBNET={0}".format(project['status']['resources']['default_subnet_reference']['uuid'])
           exit (0)
         else:
           print "The '@@{calm_project_name}@@' project does not have a default subnet set."
           exit(1)
   
     # If we've reached this point in the code, none of our projects matched the calm_project_name macro
     print "The '@@{calm_project_name}@@' project does not match any of our /projects/list api call."
     print json.dumps(json.loads(resp.content), indent=4)
     exit(0)
   
   # In case the request returns an error
   else:
     print "Post clusters/list request failed", resp.content
     exit(1)

There are two main differences between this task and the previous.  The first is that instead of the script type being **Execute**, it is **Set Variable**.  Take note of the **print "SUBNET={0}"** line: Calm will parse output of the format **variable=value**, and set the variable equal to the value.  In our case, we're printing the variable called **SUBNET** is equal to the UUID of our chosen "default_subnet_reference" field in our API call response.  In the **Output** field below the Script body, we must paste in the variable name for Calm to set the variable appropriately.  Also, this variable must be defined somewhere else in the blueprint, in our case we defined it under the Service at the beginning of this section.

The second main difference is that we're no longer using our PC_Creds credentials.  Instead, we're using the **calm_jwt** macro to take care of Prism authentication.  If you're not familiar with JWT, read more about them here_.

.. _here: https://en.wikipedia.org/wiki/JSON_Web_Token

.. figure:: images/get_subnet_uuid.png


Back in the **Blueprint Canvas**, click the **+ Task** button again to add a second task to our custom action.  Fill in the following fields:

- **Task Name** - GetSubnetInfo
- **Type** - Execute
- **Script Type** - EScript
- **Script** - Paste in the following script

.. code-block:: python

   # Get the JWT
   jwt = '@@{calm_jwt}@@'
   
   # Set the headers, url, and payload
   headers = {'Content-Type': 'application/json', 'Accept': 'application/json', 'Authorization': 'Bearer {}'.format(jwt)}
   url     = "https://@@{address}@@:9440/api/nutanix/v3/subnets/@@{SUBNET}@@"
   payload = {}
   
   # Make the request
   resp = urlreq(url, verb='GET', params=json.dumps(payload), headers=headers, verify=False)
   
   # If the request went through correctly, print it out.  Otherwise error out, and print the response.
   if resp.ok:
       print json.dumps(json.loads(resp.content), indent=4)
       exit(0)
   else:
       print "Get request failed", resp.content
       exit(1)

There's nothing too groundbreaking in this task.  As with the very first task in this exercise, we're doing an **Execute** of type **EScript**.  Similar to the previous task, we're using the JWT macro instead of using blueprint credentials.  Lastly, the API call is a GET instead of a POST, and we're utilizing the **SUBNET** variable we set in the previous task.

.. figure:: images/get_subnet_info.png
   

Click **Save**, and ensure no errors or warnings appear.

Launching the Blueprint and Running the Custom Actions
......................................................

Click the **Launch** button in the upper right corner of our blueprint.  Name the application **RestCalls<Initials>**, and then click **Create**.

Navigate to the **Manage** page of the application, and view the **Create** task that is currently running.  It should complete quickly, as no VMs are getting created, and we do not have any tasks or scripts associated with our Create or Package Install.

.. figure:: images/app_create.png
   

Next, run the **RestList** action by clicking the play button next to it.  You should see a pop-up appear with our **kind** variable, leave **apps** in the field, and then click **Run**.

.. figure:: images/apps_run.png
   

In the output on the right pane, maximize the **RuntimePost** task, and view the API output.  Toggle between the output and the Script, by clicking the **View Script** link below the output.  Maximize the output/script window to make viewing easier.

.. figure:: images/apps_run2.png
   

Next, run the **RestList** task again, this time changing the value of the runtime variable to something of your choice, like **images**, **clusters**, **hosts**, or **vms**.  View the output like before.

Lastly, run the **GetDefaultSubnet** action by clicking the play button next to it, and clicking **Run** in the pop-up.  Expand both the **GetSubnetUUID** and **GetSubnetInfo** tasks, and view the output and the scripts, as before.

.. figure:: images/GetDefaultSubnet.png
   

.. figure:: images/GetDefaultSubnet2.png
   
Publishing to the Task Library
..............................

Imagine you have a task that you repeatedly use, like an upgrade task, or a common API call.  You may want to use this task across multiple blueprints, without having to copy and paste them, or keeping them in some third party tool.  The Task Library feature allows publishing of these commonly used tasks into a central repository.

Navigate back to the **Blueprints** section, and select your same **eScript<Initials>** blueprint.  In the **Application Overview** pane, select the **RestList**, and in the **Blueprint Canvas**, select the **RuntimePost** task.

In the **Configuration Pane** on the right, click the **Publish to Library** button.  In the pop-up that appears, change the following fields:

- **Name** - Prism Central Runtime List <Initials>
- Replace **address** with **Prism_Central_IP**

Then click **Apply**.  You should note that the original **address** macro was replaced with **Prism_Central_IP**.  This feature allows you to make your macro names more generic to increase task portability.

.. figure:: images/publish_task.png

Click **Publish**.  Then on the left menu, select the **Task Library** icon.  Select and view the task that you just published.  Optionally share it with additional Projects, and then click **Save**.

Takeaways
+++++++++

* In addition to being able to use Bash and Powershell scripts, Nutanix Calm can use EScript, which is a sandboxed Python interpreter, to provide application lifecycle management.
* EScript tasks are run directly within the Calm engine, rather than being executed on the remote machine.
* Shell, Powershell, and EScript tasks can all be utilized to set a variable based on script output.  That variable can then be used in other portions of the blueprint.
* The Task Library allows for publishing of commonly used tasks into a central repository, giving the ability to share these scripts across Projects and Blueprints.

