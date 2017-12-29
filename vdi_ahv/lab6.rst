Lab 6 - Delivering Non-Persistent Desktops
------------------------------------------

Overview
++++++++

Creating the Machine Catalog
++++++++++++++++++++++++++++

In the **XD** VM console, open **Citrix Studio**.

Right-click **Machine Catalogs > Create Machine Catalog**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/1.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/2.png

Select **Desktop OS** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/3.png

Select **Machines that are power managed** and **Citrix Machine Creation Services**. Click **Next**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/4.png

Select **Random** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/5.png

Select your Nutanix storage container and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/6.png

Select your **W10-Gold** snapshot and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/7.png

Fill out the following fields and click **Next**:

- **How many virtual machines do you want to create** - *4*
- **Total memory (MB) on each machine** - *4096*
- **Virtual CPUs** - *2*
- **Cores per vCPU** - *1*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/8.png

Select **Create new Active Directory accounts**. Under the **NTNX.local** domain, select the **Non-PersistentDesktops** OU. Specify *W10NP-###* as the **Account naming scheme**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/9.png

Specify a friendly **Machine Catalog name** and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/10.png

MCS will now create a clone from the snapshot of **W10-Gold** and follow a similar preparation process as the Persistent Machine Catalog.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/11.png

Upon completion, view the details of the Machine Catalog in **Citrix Studio**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/12.png

Creating the Delivery Group
+++++++++++++++++++++++++++

Right-click **Delivery Groups > Create Delivery Group**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/13.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/14.png

Select your **Non-Persistent** Machine Catalog and specify the maximum number of VMs available for the Delivery Group.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/15.png

Select **Restrict** and click **Add**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/16.png

Specify *NonPersistentUsers* in the **Object names** field and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/17.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/18.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/19.png

Click **Add** and fill out the following fields:

- **Display name** - *Pooled Windows 10 Desktop*
- **Description** - *Non-Persistent 2vCPU/4GB RAM Windows 10 Virtual Desktop*
- Select **Allow everyone with access to this Delivery Group**
- Select **Enable desktop assignment rule**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/20.png

Click **OK > Next**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/21.png

Specify a friendly name for the Delivery Group and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/22.png

Following creation of the pool, observe in **Prism** that 1 of the **W10P-###** VMs been has powered on.

In **Citrix Studio**, right-click your Delivery Group and click **Edit Delivery Group**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/23.png

Select **Power Management** from the left hand menu.

Click and drag the number of machines powered on during peak hours from 1 to 4. The peak hours period can optionally be modified by clicking and dragging to either the left or the right.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/24.png

.. note:: For more granular control of registered, powered on VMs you can click the Edit link and provide the number or percentage of VMs you want available for every hour of the day. You can also configure the disconnected VM policy to free up disconnected VMs after a configurable time out period, returning the desktop to the pool for another user.

After increasing the number of powered on virtual machines, validate the **W10NP-###** VMs are powered on in **Prism** and appear as Registered in **Citrix Studio**.

Connecting to the Desktop
+++++++++++++++++++++++++

Open **\http://<XD-VM-IP>/Citrix/StoreWeb** in a browser on the same L3 LAN as your XD VM.

Log in as **NTNX\USER2**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/25.png

Select the **Desktops** tab and observe both your **Personal Windows 10 Desktop** and a **Pooled Windows 10 Desktop** are available. Click the **Pooled** desktop to launch the session.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab6/26.png

After the virtual desktop has completed logging in, experiment by changing application settings, installing applications, restarting the VM, and logging in again. Try logging in as **USER2** and **USER3**.

Takeaways
+++++++++
