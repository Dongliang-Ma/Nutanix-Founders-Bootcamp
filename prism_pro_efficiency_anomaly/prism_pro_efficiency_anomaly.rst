.. _prism_pro_effeciency_anomaly:

--------------------------------------------
Prism Pro: VM Efficiency & Anomaly Detection
--------------------------------------------

Overview
++++++++



Lab Setup
+++++++++

This lab requires a VM to be provisioned and will be stressed later in the lab to produce CPU and memory metrics.

Applications provisioned as part of the  :ref:`linux_tools_vm` will be used to accomplish this.

#. Please follow the instructions to deploy the :ref:`linux_tools_vm` before moving on with this lab.


#. Right click the following URL to open a new tab and navigate to the webpage at http://10.42.247.70 and enter the details in the Setup portion of the form. Then click 'Begin Setup' once you have filled in all the fields. This will get your environment ready for this lab. **Keep this tab open during entire Prism Pro lab to return to as directed in later portions.**

   .. figure:: images/ppro_08.png

#. After hitting continue, it will take a bit of time for the setup to complete. In the meantime, switch back to Prism Central and go through the labs.

VM Efficiency
+++++++++++++++++++++++++++

Prism Pro uses X-Fit machine learning to detect the behaviors of VMs running within the managed clusters. Then applies a classification to VMs that are learned to be inefficient. The following are short descriptions of the different classifications:

* **Overprovisioned:** VMs identified as using minimal amounts of assigned resources.
* **Inactive:** VMs that have been powered off for a period of time or that are running VMs that do not consume any CPU, memory, or I/O resources.
* **Constrained:** VMs that could see improved performance with additional resources.
* **Bully:** VMs identified as using an abundance of resources and affecting other VMs.

#. In **Prism Central**, select :fa:`bars` **> Dashboard** (if not already there).

#. From the Dashboard, take a look at the VM Efficiency widget. This widget gives a summary of inefficient VMs that Prism Pro’s X-FIT machine learning has detected in your environment. Click on the ‘View All Inefficeint VMs’ link at the bottom of the widget to take a closer look.

   .. figure:: images/ppro_58.png

#. You are now viewing the Efficiency focus in the VMs list view with more details about why Prism Pro flagged these VMs. You can hover the text in the Efficiency detail column to view the full description.

   .. figure:: images/ppro_59.png

#. Once an admin has examined the list of VM on the efficiency list they can determine any that they wish to take action against. From VMs that have too many or too little resources they will require the individual VMs to be resized. This can be done in a number of ways with a few examples listed below:

* **Manually:** An admin edits the VM configuration via Prism or vCenter for ESXi VMs and changes the assigned resources.
* **X-Play:** Use X-Plays automated play books to resize VM(s) automatically via a trigger or admins direction. There will be a lab story example of this later in this lab.
* **Automation:** Use some other method of automation such as powershell or REST-API to resize a VM.


Anomaly Detection
+++++++++++++++++++++++++++++++

In this lab story you will take a look at VMs with an anomaly. An anomaly is a deviation from the normal learned behavior of a VM. The X-FIT alogrithms learn the normal behavior of VMs and represent that as a baseline range on the different charts for each VM.

#. Now let's take a take a look at a VM by searching for ‘bootcamp_good’ and selecting ‘bootcamp_good_1’.

   .. figure:: images/ppro_61.png

#. Go to Metrics > CPU Usage. Notice a dark blue line, and a lighter blue area around it. The dark blue line is the CPU Usage. The light blue area is the expected CPU Usage range for this VM. This range is calculated using Prism Pro’s X-FIT machine learning engine. In this case, an anomaly has been raised for this VM, because the Usage is far below the expected range. You can also reduce the time range “Last 24 hours” to examine the chart more closely.

   .. figure:: images/ppro_60.png

#. Click **“Alert Setting”** to set an alert policy for this kind of situation.

#. In the right hand side, you can change some of the configurations however you would like. In this example I have changed the Behavioral Anomaly threshold to ignore anomalies between 10% and 70%. All other anomalies will generate a Warning alert. I have also adjusted the Static threshold to Alert Critical if the CPU Usage on this VM exceeds 95%.

   .. figure:: images/ppro_25.png

#. Hit **Cancel** to exit the policy creation workflow.

Takeaways
+++++++++

- Prism Pro is our solution to make IT OPS smarter and automated. It covers the IT OPS process ranging from intelligent detection to automated remediation.
- X-FIT is our machine learning engine to support smart IT OPS, including forecast, anomaly detection, and inefficiency detection.
