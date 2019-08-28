.. _prism_pro_resource_planning:

--------------------------------
Prism Pro: Resource Planning
--------------------------------

Overview
++++++++

This lab will introduce Prism Central’s resource planning features that help you stay on top of cluster utilization and more accurately predict cluster expansions.

Lab Setup
+++++++++

This lab requires a VM to be provisioned and will be stressed later in the lab to produce CPU and memory metrics.

Applications provisioned as part of the  :ref:`linux_tools_vm` will be used to accomplish this.

#. Please follow the instructions to deploy the :ref:`linux_tools_vm` before moving on with this lab.


#. Right click the following URL to open a new tab and navigate to the webpage at http://10.42.247.70 and enter the details in the Setup portion of the form. Then click 'Begin Setup' once you have filled in all the fields. This will get your environment ready for this lab. **Keep this tab open during entire Prism Pro lab to return to as directed in later portions.**

   .. figure:: images/ppro_08.png

#. After hitting continue, it will take a bit of time for the setup to complete. In the meantime, switch back to Prism Central and go through the labs.

Prism Central Resource Planning
+++++++++++++++++++++++++++++++

Nutanix utilizes our X-Fit machine learning and data analytics as part of Prism Pro. We utilize that machine learning and data analytics to provide Cluster Runway and just in time forecasting (What If Planning).

Capacity Runway
...............

Use Prism Central’s Capacity Runway feature to learn about cluster resource planning and recommendations.

Lets view the Capacity Runway of your lab cluster.

#. In **Prism Central > Planning > Capacity Runway**.

- Note the runway summaries showing the days left for each cluster.
- How long does the current cluster has before it runs out of memory, CPU, and storage?

#. Click on the **Prism-Pro-Cluster** cluster.

. You can now take a look at the Runway for Storage, CPU, and Memory.

   .. figure:: images/ppro_12.png

#. When selecting the Memory tab, you can see a Red Exclamation mark, indicating where this cluster will run out of Memory. You can hover the chart at this point to see on which day this will occur.

   .. figure:: images/ppro_13.png

#. Click on the **‘Optimize Resources’** button on left. This is where you can see the inefficient VMs in the environment with suggestions on how you can optimize these resources to be as efficient as possible.

   .. figure:: images/ppro_14.png

#. Close the optimize resources popup.

What If Planning
................

#. Under the **‘Adjust Resources’** section in the left side of this page, click the **‘Get Started’** button. We can now use this to start planning for new workloads and see how runway will need to be extended in the future.

#. Click the **add/adjust** button in the left side underneath the ‘Workloads’ item.

   .. figure:: images/ppro_15.png

#. Add one for VDI and select 1000 Users. You can also set a date for when this workload should be added to the system. Save this workload when you are done.

   .. figure:: images/ppro_16.png

   .. figure:: images/ppro_17.png

#. Add another workload of your choice.

#. Now click the **‘Recommend’** button on the right side of the page.

   .. figure:: images/ppro_18.png

#. Once the Recommendation is available, toggle between list and chart view to get a better overview of your Scenario.

   .. figure:: images/ppro_19.png

#. Click the **Generate PDF** button in the upper right hand corner. This will open a new tab with a PDF report for the scenario/workloads you have created.

   .. figure:: images/ppro_19b.png

#. View your report.

   .. figure:: images/ppro_20.png

Takeaways
+++++++++

- The Capacity Runway view in the Planning dashboard allows you to view summary resource runway information for the registered clusters and access detailed runway information about each cluster.
- The Scenarios view in the Planning dashboard allows you to create "what if" scenarios to assess the future resource requirements for potential work loads that you specify.
- You must have a Prism Pro license to use the resource planning tools.
