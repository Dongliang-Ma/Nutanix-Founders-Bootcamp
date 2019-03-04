.. _flow_assign_categories_in_calm:

---------------------------------------------
Flow: Assign Categories inside Calm Blueprint
---------------------------------------------

Overview
++++++++



Using Flow with Calm
++++++++++++++++++++

At the beginning of this lab, Calm was used to provide a multi-tier application as a basis for understanding how Flow policies can be created, applied, and monitored using existing workloads in an environment.

Flow also integrates natively with Calm to define Categories at the Service (VM) level within the Calm blueprint.

.. note::

  Flow policies for Calm provisioned VMs should ensure that port 22 (for Linux VMs) and port 5985 (for Windows VMs) are open. This was done earlier in the lab when initially creating the **AppTaskMan** policy.

As the Calm blueprint used in this exercise requires Internet access to install remote packages, first update the **AppTaskMan-**\ *Initials* security policy from **Whitelist Only** to **Allow All** for **Outbound** connections, as shown below.

.. figure:: images/46.png

In a production environment, VMs from Calm could leverage either a staging category during provisioning or additional Outbound rules to specify only the hosts with which it needed to communicate to complete provisioning.

In **Prism Central**, select :fa:`bars` **> Services > Calm**.

Click |blueprints| **Blueprints > **\ *Initials*\ **-TaskManager** to open your existing blueprint.

Select the **WebServer** service.

.. figure:: images/44.png

On the **VM** tab, scroll to **Categories** and select the **AppType: TaskMan-**\ *Initials* and **AppTier: TMWeb-**\ *Initials* categories.

.. figure:: images/45.png

Select the **HAProxy** service and add the **AppType: TaskMan-**\ *Initials* and **AppTier: TMLB-**\ *Initials* categories.

Select the **MySQL** service and add the **AppType: TaskMan-**\ *Initials* and **AppTier: TMDB-**\ *Initials* categories.

Select the **WinClient** service and add the **Environment: Dev** category.

Click **Save**.

Click **Launch**.

Fill out the following fields:

- **Name of the Application** - *Initials*-TaskManager2
- **User_initials** - *Initials*

Click **Create**.

You can monitor the status of your application deployment by clicking |applications| **Applications** and clicking your application's name.

Once application provisioning has completed, note the additional VMs detected as part of the policy.

Does the application behave as expected? From the new client VM, are you able to ping the load balancer but not the database? Are you able to access the application?

Integrating with Calm offers... <?>

The application of categories can also be performed programmatically via ... <?>

Takeaways
+++++++++

- Calm Blueprints can deploy applications that are automatically secured with Flow.

.. |blueprints| image:: ../images/blueprints.png
.. |applications| image:: ../images/blueprints.png
