*Optional Lab* - Testing Environment Resiliency
-----------------------------------------------

Overview
++++++++

Updating CVM Firewall Rules
+++++++++++++++++++++++++++

In order to access the internal Curator diagnostic page from outside of the Controller VM subnet, we'll need to open TCP port 2010 in the CVM firewall.

Using an SSH client, execute the following:

.. code:: bash

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > allssh "sudo iptables -t filter -A WORLDLIST -p tcp -m tcp --dport 2010  -j ACCEPT"

Configuring VM Affinity
+++++++++++++++++++++++

In **Prism > VM > Table**, select **DC**. In the **Host** column, observe the host the **DC** VM is currently running on. Click **Update**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/1.png

Under **VM Host Affinity**, click **Set Affinity**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/2.png

Select Node 1 and Node 2 and click **Save**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/3.png

Click **Save**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/4.png

If your VM wasn't already running on Node 1 or 2, observe that it was automatically live migrated after updating the affinity policy.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/5.png

Repeat these steps for **XD** and any other applicable non-AFS or non-desktop VMs (**NSVPX**, etc.).

Enabling HA Memory Reservation
++++++++++++++++++++++++++++++

In **Prism**, click the **Settings** icon and select **Manage VM High Availability**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/6.png

Select **Enable HA** and click **Save > OK**.

.. note:: Increased failures to tolerate, such as in the case of an RF3 cluster wanting to tolerate 2 node failures, can be defined via acli.

  .. code::

    > acli  ha.update num_host_failures_to_tolerate=2

  Restart priority for individual VMs can be defined via acli. A negative value disables HA restart for that VM.

  .. code::

    > acli vm.update <VM NAME> ha_priority=1000

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/7.png

Staging Connections
+++++++++++++++++++

Log in to Citrix StoreFront as **USER2** and launch both your **Pooled** and **Personal** desktops.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/9.png

In **Citrix Studio > Search > Sessions**, note the VM Names of **USER2**'s sessions

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/10.png

In **Prism > VM > Table**, search for each of **USER2**'s VMs. Validate whether or not the VM is currently running on Node 3, if not, select the VM and click **Migrate**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/11.png

Select Node 3 from the **Host** drop down menu and click **Migrate**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/12.png

Repeat these steps to ensure the second VM is also on Node 3.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/13.png

Executing Order 66
++++++++++++++++++

.. note:: If you're using a non-NX Nutanix platform you will need to consult manufacturer documentation for your hardware platform for instruction on accessing the out-of-band management and powering off the node.

In **Prism > Hardware > Table**, select Node 3. In the **Host Details** table, click the **IPMI IP** link.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/14.png

Log in with the default credentials (*ADMIN*/*ADMIN*)

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/15.png

Select **Power Control** from the **Remote Control** drop down menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/16.png

Select **Power Off Server - Immediate** and click **Perform Action**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/17.png

Immediately you'll observe that both of your Citrix Receiver sessions have been interrupted. Close both of them.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/19.png

Log in to Citrix StoreFront again as **USER2** and launch a **Pooled** desktop. You will be able to connect to another desktop immediately, complete with your profile and user data if configured.

In **Citrix Studio**, verify that the desktop to which you're now connected is not the same VM to which you were previously connected.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/20.png

In **Prism > Tasks**, the node failure has been detected and VMs have already begun to power on on the remaining nodes in the cluster. In the screenshot below we can see our **Personal Windows 10 Desktop** has already been powered on on Node 2.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/21.png

In **Citrix Studio > Search > Desktop OS Machines**, verify your **Personal Windows 10 Desktop VM** now appears as Registered with the Delivery Controller.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/22.png

Return to Citrix StoreFront and launch your **Personal Windows 10 Desktop**. Verify that the desktop logs in successfully.

In **Prism > Home**, verify that the cluster is in Critical Status and that a rebuild is in progress.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/23.png

Open \https://<*NUTANIX-CLUSTER-IP*>:2010 in your browser and click the **Curator Master** link.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/24.png

Verify that Node 3 is down and that a **Partial Scan** due to a **Node Failure** has generated many background tasks. Click the **Execution ID** link associated with this job for more details.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/25.png

The majority of the jobs associated with the scan are to replicate missing extents.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/26.png

Restoring Balance to the Force
++++++++++++++++++++++++++++++

In your browser, return to the out-of-band management (IPMI) console **> Remote Control > Power Control**. Select **Power On Server** and click **Perform Action**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/27.png

After several minutes, giving time for the host and CVM to boot, verify in **Prism > Home** that **Data Resiliency Status** has returned to **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/28.png

**Health** still appears as critical, this is normal following a CVM reboot as an unexpected CVM reboot could be indicative of an issue with the cluster. After a short period of time the Health will update itself.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/29.png

In **Prism > VM > Table**, filter by the Node 3 hostname and note that the majority of VMs that had previously been running on Node 3 have returned to running on this node.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/30.png

Restore CVM firewall to default configuration:

.. code:: bash

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > allssh "sudo service iptables start"; done

Verify you're no longer able to access the Curator page from your browser.

Takeaways
+++++++++
