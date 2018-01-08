Lab 8 - Updating Machine Catalogs
---------------------------------

Overview
++++++++

In this exercise you will use perform "Day 2" operations for a XenDesktop environment, including expanding an existing Machine Catalog with more desktop VMs, and rolling out an updated gold image to your non-persistent Machine Catalog.

Adding More Desktops
++++++++++++++++++++

In **Citrix Studio > Machine Catalogs**, right-click your Non-Persistent Machine Catalog and select **Add Machines**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/1.png

Specify the **Number of machines to add** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/2.png

Confirm the existing OU and naming scheme and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/3.png

Review configuration and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/4.png

In **Citrix Studio > Delivery Groups**, right-click your Non-Persistent Delivery Group and select **Add Machines**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/5.png

Increment the **number of Machines for this Delivery Group** and click **Next > Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/6.png

Edit the Non-Persistent Delivery Group **Power Management** to power on your added desktop capacity.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/7.png

Observe the additional **W10NP-###** VMs are powered on in **Prism** and appear Registered in **Citrix Studio**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/8.png

Updating the Gold Image
+++++++++++++++++++++++

In **Prism > VM > Table**, select the **W10-Gold** VM and click **Power on**.

Once the VM has started, click **Launch Console**.

Install or update an application and then shut down the VM.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/10.png

Once **W10-Gold** is powered off, select the VM and click **Take Snapshot**

Specify a **Name** for the snapshot (e.g. "W10-Gold vYYYYMMDD-X - Installed Sublime") and click **Submit**.

In **Citrix Studio > Machine Catalogs**, right-click your Non-Persistent Machine Catalog and select **Update Machines**. Observe that Update Machines is not an option for your Persistent Machine Catalog, updates to persistent desktops are handled via traditional patch management tools.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/11.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/12.png

Select your updated **W10-Gold** VM snapshot and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/13.png

Fill out the following fields and click **Next**:

- **When do you want to update this image** - *Immediately*
- **Distribution time** - *Update all machines at the same time*
- **Notify users of the update** - *Do not send a notification*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/14.png

Review configuration and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/15.png

A new preparation VM will be cloned and booted to prepare the new snapshot.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/16.png

After the preparation VM is shut down and removed, **Prism > Tasks** will display the changing power state of VMs as they're powered off. After several minutes you'll see VM disk update tasks, which is MCS updating the cloned disk for the provisioned VMs to point to the new prepared snapshot.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/17.png

Log in to Citrix StoreFront as **USER3** and launch your **Pooled** desktop to verify the presented desktop reflects the updated image.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/18.png

In **Citrix Studio > Machine Catalogs**, note you now have an option to rollback your Non-Persistent Machine Catalog to the previous snapshot.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab8/19.png

Takeaways
+++++++++

- Adding capacity to an existing Machine Catalog can be done rapidly. Combined with Nutanix's ability to expand a physical cluster with a One Click operation, this enables an IT organization to be very responsive to changing business needs.

.. No vCenter.

- Nutanix creates separate block maps (metadata mapping a vDisk to its corresponding extents) for each new snapshot created, eliminating the additional overhead and read latency common with large snapshot chains traditionally seen in other hypervisors. Gold image management is simplified by not having to mitigate the performance impact of snapshot chains.

- Gold image versioning with MCS can be implemented easily through snapshot naming conventions.
