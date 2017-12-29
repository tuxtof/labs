Lab 5 - Delivering Persistent Desktops
--------------------------------------

Overview
++++++++

Creating the Machine Catalog
++++++++++++++++++++++++++++

In the **XD** VM console, open **Citrix Studio**.

Right-click **Machine Catalogs > Create Machine Catalog**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/1.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/2.png

Select **Desktop OS** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/3.png

Select **Machines that are power managed** and **Citrix Machine Creation Services**. Click **Next**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/4.png

Select **Static** and **Yes, create a dedicated virtual machine**. Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/5.png

Select your Nutanix storage container and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/6.png

Select your **W10-Gold** snapshot and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/7.png

Fill out the following fields and click **Next**:

- **How many virtual machines do you want to create** - *3*
- **Total memory (MB) on each machine** - *4096*
- **Virtual CPUs** - *4*
- **Cores per vCPU** - *1*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/8.png

Select **Create new Active Directory accounts**. Under the **NTNX.local** domain, select the **PersistentDesktops** OU. Specify *W10P-###* as the **Account naming scheme**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/9.png

Specify a friendly **Machine Catalog name** and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/10.png

MCS will now create a clone from the snapshot of **W10-Gold**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/11.png

Observe the clone booting in **Prism** briefly before shutting down and being removed automatically.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/12.png

MCS will now create the VMs for our Machine Catalog.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/13.png

Observe the clones exist in **Prism** but are not powered on.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/14.png

Upon completion, view the details of the Machine Catalog in **Citrix Studio**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/15.png

Creating the Delivery Group
+++++++++++++++++++++++++++

Right-click **Delivery Groups > Create Delivery Group**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/16.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/17.png

Select your **Persistent** Machine Catalog and specify the maximum number of VMs available for the Delivery Group.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/18.png

Select **Desktops** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/19.png

Select **Restrict** and click **Add**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/20.png

Specify *PersistentUsers* in the **Object names** field and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/21.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/22.png

Click **Add** and fill out the following fields:

- **Display name** - *Personal Windows 10 Desktop*
- **Description** - *Persistent 4vCPU/4GB RAM Windows 10 Virtual Desktop*
- Select **Allow everyone with access to this Delivery Group**
- **Maximum desktops per user** - *1*
- Select **Enable desktop assignment rule**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/23.png

Click **OK > Next**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/24.png

Specify a friendly name for the Delivery Group and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/25.png

Following creation of the pool, observe in **Prism** that 1 of the **W10P-###** VMs been has powered on.

In **Citrix Studio**, right-click your Delivery Group and click **View Machines**. Alternatively you can double-click on the name of the Delivery Group.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/26.png

Observe the powered on desktop now appears as **Registered** with the Delivery Controller, indicating the desktop is ready for user connection.

Connecting to the Desktop
+++++++++++++++++++++++++

Open **\http://<XD-VM-IP>/Citrix/StoreWeb** in a browser on the same L3 LAN as your XD VM.

If prompted, click **Detect Receiver**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/27.png

If Citrix Receiver is not installed, select **I Agree with the Citrix license agreement** and click **Download**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/28.png

Launch the **CitrixReceiverWeb.exe** installer and complete the installation wizard using default settings.

.. note:: Do not enable single sign-on during Citrix Receiver installation.

Refresh your browser or click the **Detect again** link.

If prompted, select **Always open these types of links in the associated app** and click **Open Citrix Receiver Launcher**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/29.png

.. note:: This may appear slightly different depending on your browser (Chrome shown). You want to allow your browser to open the Citrix Receiver application.

Refresh your browser and log in to StoreFront as **NTNX\\USER1**

.. note:: If you're still being prompted to detect Citrix Receiver, click **Already installed** to proceed to the login page.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/30.png

Select the **Desktops** tab and click your **Personal Windows 10 Desktop** to launch the session.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/31.png

.. note:: Depending on your browser you may have to click on the downloaded .ica file if Receiver does not open automatically. You may also be able to instruct the browser to always open .ica files.

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/32.png

After the virtual desktop has completed logging in, experiment by changing application settings, installing applications, restarting the VM, and logging in again. Try logging in as **USER2** and **USER3**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/33.png

In **Citrix Studio**, observe the changes to VM details. As a user logs in they are statically assigned a desktop and another desktop will power on and register with the Delivery Controller, waiting for the next user.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab5/34.png

Takeaways
+++++++++
