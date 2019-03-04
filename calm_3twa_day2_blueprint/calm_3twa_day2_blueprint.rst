.. _calm_3twa_day2_blueprint:

--------------------------------------
Calm: 3-Tier WebApp - Day 2 Operations
--------------------------------------

Overview
++++++++

Thus far we've seen how Calm can model complex applicaions in an easy to consume blueprint, and orchestrate their deployment.  However, Calm can manage applications throughout their *entire* lifecycle, not just creation.  In this lab, we'll create several custom actions which cover typical day 2 application operations.


Scale Out
.........

Imagine you're the administrator of the Task Manager Application that we've been building, and you're currently unsure of the amount of demand for this application by your end users.  Or imagine you expect the demand to ebb and flow due to the time of the year.  How can we easily scale to meet this changing demand?

If you recall in a previous step, we set the minimum number of **WebServer** replicas to 2, and our maximum to 4.  In current versions of Calm, the minimum number is always the starting point.  In the event our default 2 replicas of our **WebServer** web server is not enough to handle the load of your end users, we can perform a **Scale Out** Action.

In the **Application Overview > Application Profile** section, expand the **Default** Application Profile.  Then, select :fa:`plus-circle` next to the **Actions** section.  On the **Configuration Panel** to the right, rename the new Action to be **Scale Out**.

.. figure:: images/510scaleout1.png

In the box **below** the **WebServer** service tile, click the **+ Task** button to add a scaling task, and fill out the following fields:

- **Task Name** - web_scale_out
- **Scaling Type** - Scale Out
- **Scaling Count** - 1

.. figure:: images/510scaleout2.png

When a user later runs the **Scale Out** task, a new **WebServer** VM will get created, and the **Package Install** tasks for that service will be executed.  However, we do need to modify the **HAProxy** configuration in order to start taking advantage of this new web server.

**Within** the **HAProxy** service tile, click the **+ Task** button, then fill out the following fields:

- **Task Name** - add_webserver
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

Copy and paste the following script into the **Script** field:

.. code-block:: bash

  #!/bin/bash
  set -ex

  host=$(echo "@@{WebServer.address}@@" | awk -F "," '{print $NF}')
  port=80
  echo " server host-${host} ${host}:${port} weight 1 maxconn 100 check" | sudo tee -a /etc/haproxy/haproxy.cfg

  sudo systemctl daemon-reload
  sudo systemctl restart haproxy

That script will grab the last address in the WebServer address array, and add it to the haproxy.cfg file.  However, we want to be sure that this doesn't happen until **after** the new WebServer is fully up, otherwise the HAProxy server may send requests to a non-functioning WebServer.

To solve this issue, on the **Workspace**, click on the **web_scale_out** task, then the **Create Edge** arrow icon, and finally click on the **add_webserver** task to draw the edge.  Afterwards your **Workspace** should look like this:

.. figure:: images/510scaleout3.png

Scale In
........

Again imagine you're the administrator of this Task Manager Application we're building.  It's the end of your busy season, and you'd like to scale the Web Server back in to save on resource utilization.  To accomplish this, navigate to the **Application Overview > Application Profile** section, expand the **Default** Application Profile.  Then, select :fa:`plus-circle` next to the **Actions** section.  On the **Configuration Panel** to the right, rename the new Action to be **Scale In**.

.. figure:: images/510scalein1.png

**Below** the **WebServer** service tile, click the **+ Task** button to add a scaling task, and fill out the following fields:

- **Task Name** - web_scale_in
- **Scaling Type** - Scale In
- **Scaling Count** - 1

.. figure:: images/510scalein2.png

When a user later runs the **Scale In** task, the last **WebServer** replica will have its **Package Uninstall** task run, the VM will be shut down, and then deleted, which will reclaim resources.  However, we do need to modify the **HAProxy** configuration to ensure that we're no longer sending traffic to the to-be-deleted Web Server.

**Within** the **HAProxy** service tile, click the **+ Task** button, then fill out the following fields:

- **Task Name** - del_webserver
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

Copy and paste the following script into the **Script** field:

.. code-block:: bash

  #!/bin/bash
  set -ex

  host=$(echo "@@{WebServer.address}@@" | awk -F "," '{print $NF}')
  sudo sed -i "/$host/d" /etc/haproxy/haproxy.cfg

  sudo systemctl daemon-reload
  sudo systemctl restart haproxy

That script will grab the last address in the WebServer address array, and remove it from the haproxy.cfg file.  Similar to the last step, we want to be sure that this happens **before** the new WebServer is destroyed, otherwise the HAProxy server may send requests to a non-functioning WebServer.

To solve this issue, on the **Workspace**, click on the **del_webserver** task, then the **Create Edge** arrow icon, and finally click on the **web_scale_in** task to draw the edge.  Afterwards your **Workspace** should look like this:

.. figure:: images/510scalein3.png

Click **Save** and ensure no errors or warnings pop-up.  If they do, resolve the issue, and **Save** again.

Upgrades
........

Again, let's imagine we're the administrator of this web application.  Your company has a mandate to keep all application code up to date, to help minimize security vulnerabilities.  Your company also has a strict change control process, meaning you can only update your application during the weekend.  You're tired of doing a straightforward, yet time consuming, upgrade procedure one Saturday of every month.  Let's get your Saturday back by modeling the application upgrade with Nutanix Calm.

In the **Application Overview > Application Profile** section, expand the **Default** Application Profile.  Then, select :fa:`plus-circle` next to the **Actions** section.  On the **Configuration Panel** to the right, rename the new Action to be **Upgrade**.

The first thing we're going to need to do is to stop the respective processes on each of our Services.  **Within each** of our 3 Services, click the **+ Task** button to add a new task, and fill in the following information:

+------------------+-----------+---------------+-------------+
| **Service Name** | MySQL     | WebServer     | HAProxy     |
+------------------+-----------+---------------+-------------+
| **Task Name**    | StopMySQL | StopWebServer | StopHAProxy |
+------------------+-----------+---------------+-------------+
| **Type**         | Execute   | Execute       | Execute     |
+------------------+-----------+---------------+-------------+
| **Script Type**  | Shell     | Shell         | Shell       |
+------------------+-----------+---------------+-------------+
| **Credential**   | CENTOS    | CENTOS        | CENTOS      |
+------------------+-----------+---------------+-------------+
| **Script**       | See Below | See Below     | See Below   |
+------------------+-----------+---------------+-------------+

**StopMySQL Script:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo systemctl stop mysqld

**StopWebServer Script:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo systemctl stop php-fpm
   sudo systemctl stop nginx

**StopHAProxy Script:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo systemctl stop haproxy

Once complete, our blueprint canvas should look like this:

.. figure:: images/upgrade1.png

However, as we saw during the scaling section, we do not want to get into a situation where the WebServer goes down before the HAProxy, nor do we want the MySQL database to go down before the WebServers.  So let's manually draw orchestration edges so our HAProxy stops first, then the WebServers, then the MySQL database:

.. figure:: images/upgrade2.png

Now that our critical services are stopped, we'll want to perform our updates.  Again, **within each** Service, add a new Task.  All of the 3 tasks are identical other than the name:

+------------------+--------------+------------------+----------------+
| **Service Name** | MySQL        | WebServer        | HAProxy        |
+------------------+--------------+------------------+----------------+
| **Task Name**    | UpgradeMySQL | UpgradeWebServer | UpgradeHAProxy |
+------------------+--------------+------------------+----------------+
| **Type**         | Execute      | Execute          | Execute        |
+------------------+--------------+------------------+----------------+
| **Script Type**  | Shell        | Shell            | Shell          |
+------------------+--------------+------------------+----------------+
| **Credential**   | CENTOS       | CENTOS           | CENTOS         |
+------------------+--------------+------------------+----------------+
| **Script**       | See Below    | See Below        | See Below      |
+------------------+--------------+------------------+----------------+

**Script for all 3 Upgrade Tasks:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo yum update -y

Afterwards, your blueprint canvas should look like this:

.. figure:: images/upgrade3.png

From an a task ordering perpective, do we need to draw any orchestration edges?  Notice in the screenshot above that Calm automatically draws and edge from the Stop task to the Upgrade task, which is good as that's required.  However, do we need any side to side dependencies?

If you said "no", you're correct.  The critical components are starting and stopping of the Services, there's no reason to have each Service upgrade one at a time.  **Unless** you specifically tell Calm **not** to, Calm will always run tasks in parallel to save time.

Now that our Services have been upgraded, it's time to start them back up.  Again, we'll add a Task **within each** Service, with the following values:

+------------------+--------------+------------------+----------------+
| **Service Name** | MySQL        | WebServer        | HAProxy        |
+------------------+--------------+------------------+----------------+
| **Task Name**    | StartMySQL   | StartWebServer   | StartHAProxy   |
+------------------+--------------+------------------+----------------+
| **Type**         | Execute      | Execute          | Execute        |
+------------------+--------------+------------------+----------------+
| **Script Type**  | Shell        | Shell            | Shell          |
+------------------+--------------+------------------+----------------+
| **Credential**   | CENTOS       | CENTOS           | CENTOS         |
+------------------+--------------+------------------+----------------+
| **Script**       | See Below    | See Below        | See Below      |
+------------------+--------------+------------------+----------------+

**StartMySQL Script:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo systemctl start mysqld

**StartWebServer Script:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo systemctl start php-fpm
   sudo systemctl start nginx

**StartHAProxy Script:**

.. code-block:: bash

   #!/bin/bash
   set -ex

   sudo systemctl start haproxy

Once complete, our blueprint canvas should look like this:

.. figure:: images/upgrade4.png

This time, we *do* need to draw orchestration edges.  As we talked about earlier, we would not want our HAProxy service up before our WebServers, or our WebServers up before our MySQL database.  So let's draw orchestration edges, starting MySQL, then the WebServers, and lastly the HAProxy:

.. figure:: images/upgrade5.png

Click **Save** and ensure no errors or warnings pop-up.  If they do, resolve the issue, and **Save** again.

Launching and Managing the Application
......................................

Within the blueprint editor, click Launch. Specify a unique Application Name (e.g. Calm3TWA*<INITIALS>*-3) and click Create. Monitor the application as it deploys.

Once the application changes into a RUNNING state, navigate to the **Manage** tab, and run the **Scale Out** action.  Monitor the Scale Out tasks performed.  Once complete, run the **Upgrade** actin, and monitor the Upgrade tasks performed.

Takeaways
+++++++++

* Not only can Calm orchestrate complex application deployments, it can manage applications throughout their entire lifecycle, by modeling complex Day 2 operations.
* Whether it's a built in task, like scaling, or a custom task, like upgrades, Calm can be directed to perform the operations in specific order, or if order doesn't matter, perform them in parallel to save on time.
* What operation are you currently doing on a regular basis?  It's likely that it can be modeled in Calm, saving you countless hours.  Take back your weekend!
