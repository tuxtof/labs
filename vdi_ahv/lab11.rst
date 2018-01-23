*Optional Lab* - Delivering Applications And Server Desktops
-------------------------------------------------------------

Overview
++++++++

In this exercise you will deploy and configure a Windows 2012 R2 XenApp gold image. Once the image has been created, you will create both the Machine Catalog and Delivery Group.

Creating the XenApp VM (via CLI)
++++++++++++++++++++++++++++++++

Using an SSH client, execute the following:

.. code:: bash

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > acli
  <acropolis> vm.create XA-Gold num_vcpus=4 num_cores_per_vcpu=1 memory=8G
  <acropolis> vm.disk_create XA-Gold cdrom=true empty=true
  <acropolis> vm.disk_create XA-Gold clone_from_image=<Windows 2012 Disk Image Name>
  <acropolis> vm.nic_create XA-Gold network=<Network Name>
  <acropolis> vm.on XA-Gold

.. note:: When using **acli**, you can use the Tab key to autocomplete fields. Pressing Tab twice lists available namespaces and values.

In **Prism > VM > Table**, select the **XA-Gold** VM and click **Launch Console**.

Complete the Microsoft Out of Box Experience (OOBE) wizard:

- Select Country, App language, and Keyboard layout, click **Next**.

- Review the licensing agreement, click **I Accept**.

- Enter a password for the local Administrator account (e.g. nutanix/4u), click **Next**.

Log in to the **XA-Gold** VM as **Administrator**.

Enabling Nutanix Guest Tools
++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **XA-Gold** VM and click **Manage Guest Tools**.

Select **Enable Nutanix Guest Tools** and **Mount Nutanix Guest Tools**, click **Submit**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/1.png

.. note:: If the Nutanix Guest Tools have already been installed into the base image, the CD-ROM image will be automatically unmounted once the Nutanix Guest Agent service is able to communicate with the Nutanix CVM. A reboot of the VM or restart of the Nutanix Guest Tools Agent may be required. If the Nutanix Guest Tools have not yet been installed into the base image, see :ref:`windows-ngt-install` for complete instructions.

Installing Applications
+++++++++++++++++++++++

In the **XA-Gold** VM console, install a few applications such as OpenOffice, Google Chrome, etc.

.. note:: If not using a fully patched Windows 2012 R2 image, you will need to install Microsoft patch `KB2919355 <https://www.microsoft.com/en-us/download/details.aspx?id=42334>`_ before proceeding with XenDesktop installation (`CTX220340 <https://support.citrix.com/article/CTX220340>`_).

Disable Windows Updates for your gold image by opening **Control Panel > Administrative Tools > Services**.

Right-click **Windows Update > Properties**.

Select **Disabled** from the **Startup type** down down menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/2.png

Click **Stop**.

Click **OK**.

Installing Virtual Delivery Agent
+++++++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **XA-Gold** VM and click **Update**.

Under **Disks**, eject any images currently attached to the CD-ROM device and select the **Pencil** icon to change the configuration.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/3.png

Fill out the following fields and click **Update**:

- **Operation** - *Clone from Image Service*
- **Image** - Select your XenDesktop installation .iso

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/4.png

Click **Save**.

In the **XA-Gold** VM console, open the XenDesktop Installer and click the **Start** button to the right of **XenDesktop**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/5.png

Click **Virtual Delivery Agent for Windows Server OS**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/6.png

Select **Create a Master Image** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/7.png

De-select **Citrix Receiver** and click **Next**.

Select all **Additional Components** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/8.png

Select **Let Machine Creation Services do it automatically** from the drop down menu and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/9.png

Select **Optimize performance** (`CTX125874 <https://support.citrix.com/article/CTX125874>`_) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/10.png

Select **Automatically** to allow the installer to configure the Windows Firewall service to allow traffic for selected XenDesktop components.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/11.png

Review selections and click **Install**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/12.png

Select **I do not want to participate in Call Home** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/13.png

Click **Finish** and wait for the VM to restart.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/14.png

In **Prism > VM > Table**, select the **XA-Gold** VM and click **Update**.

Under **Disks**, eject the XenDesktop installation .iso currently attached to the CD-ROM device.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/15.png

Click **Save**.

Creating Gold Image Snapshot
++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **XA-Gold** VM and click **Power Off Actions**.

Select **Guest shutdown** and click **submit** to gracefully shut down the VM.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/16.png

Once **XA-Gold** is powered off, select the VM and click **Take Snapshot**

Specify a **Name** for the snapshot (e.g. "XA-Gold vYYYYMMDD-X - Post-VDA 7.15 Install") and click **Submit**.

Available snapshots and associated actions can be found by selecting the **VM Snapshots** tab under the VM table.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/17.png

Creating the Machine Catalog
++++++++++++++++++++++++++++

In the **XD** VM console, open **Citrix Studio**.

Right-click **Machine Catalogs > Create Machine Catalog**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/18.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/19.png

Select **Server OS** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/20.png

Select **Machines that are power managed** and **Citrix Machine Creation Services**. Click **Next**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/21.png

Select your Nutanix storage container and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/22.png

Select your **XA-Gold** snapshot and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/23.png

Fill out the following fields and click **Next**:

- **How many virtual machines do you want to create** - *2*
- **Total memory (MB) on each machine** - *8192*
- **Virtual CPUs** - *4*
- **Cores per vCPU** - *1*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/24.png

Select **Create new Active Directory accounts**. Under the **NTNX.local** domain, select the **XenAppServers** OU. Specify *WS12XA-###* as the **Account naming scheme**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/25.png

Specify a friendly **Machine Catalog name** and a description of the image (e.g. what types of applications or teams it will be used for). Click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/26.png

MCS will now create a clone from the snapshot of **XA-Gold**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/27.png

Upon completion, view the details of the Machine Catalog in **Citrix Studio**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/28.png

Creating the Delivery Group
+++++++++++++++++++++++++++

Right-click **Delivery Groups > Create Delivery Group**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/29.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/30.png

Select your **Persistent** Machine Catalog and specify the maximum number of VMs available for the Delivery Group.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/31.png

Select **Restrict** and click **Add**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/32.png

Specify *XenAppUsers* in the **Object names** field and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/33.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/34.png

Select **From start menu…** from the **Add** drop down menu. This will boot on of the provisioned **WS12XA-###** VMs and register with the Delivery Controller in order to provide an introspective look at the available applications within that image's Start Menu. You can also enter applications manually provided you know the path to the desired executable.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/35.png

Select the desired applications and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/36.png

Select an application and click **Properties**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/37.png

Note the pre-populated information such as executable path on the **Location** tab. Explore the additional configuration options, such as the ability to add shortcuts to a user's desktop, limit visibility or simultaneous instances (which may be necessary for licensing compliance).

Click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/38.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/39.png

Click **Add** and fill out the following fields:

- **Display name** - *Shared Windows Server Desktop*
- **Description** - *Windows Server 2012 R2 RDS Desktop*
- Select **Allow everyone with access to this Delivery Group**
- Select **Enable desktop assignment rule**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/40.png

Click **OK > Next**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/41.png

Specify a friendly name for the Delivery Group and click **Finish**.

Following creation of the pool, observe in **Prism** that all of the **WS12XA-###** VMs been have powered on.

In **Citrix Studio**, right-click your Delivery Group and click **Edit Delivery Group**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/42.png

Select the different tabs in the **Edit Delivery Group** wizard and note the additional configuration options you have available, such as the ability to prelaunch application sessions to speed up application launch time and scheduled XA VM reboots to restore the VM to a freshly provisioned state.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/43.png

In **Citrix Studio > Citrix StoreFront > Stores**, right-click the **Store Service** and click **Configure Unified Experience**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/51.png

Select **Set the unified Receiver experience as the default for this store** and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/52.png

Connecting to the Applications/Desktop
++++++++++++++++++++++++++++++++++++++

Open **\http://<XD-VM-IP>/Citrix/StoreWeb** in a browser on the same L3 LAN as your XD VM.

Log in to Citrix StoreFront as **NTNX\\USER2**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/44.png

Select the **Apps** tab and click an application to launch the session.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/45.png

Note that the application launches and appears as though it were running as part of your local operating system.

Return to the **Desktops** tab and launch your **Personal Windows 10 Desktop**.

We'll now configure the Citrix Receiver client to run applications published by our XenApp server farm from inside of our virtual desktop.

.. note:: Newer versions of Citrix Receiver do not allow connections to non-secured (HTTP) StoreFront servers by default. Follow the instructions below (`CTX134341 <https://support.citrix.com/article/CTX134341>`_) on your **Personal Windows 10 Desktop** to enable HTTP stores in Citrix Receiver:

  - Open **regedit**
  - Open **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Citrix\\Dazzle\\**
  - Set **AllowAddStore** to **A** to allow users to add non-secure Stores
  - Open **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Citrix\\AuthManager\\**
  - Right-click **AuthManager > New > String Value**
  - Specify *ConnectionSecurityMode* as the **Name** and leave **Value** blank
  - Exit Citrix Receiver (within your **Personal Windows 10 Desktop**)

From within the virtual desktop, open **Citrix Receiver**.

Specify **\http://XD.NTNX.local/Citrix/Store** as the address for StoreFront and click **Add**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/46.png

Specify your credentials and click **Log On**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/47.png

Launch an application from the **Apps** tab of Receiver.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/48.png

Close your **Personal Windows 10 Desktop** and launch your **Shared Windows Server Desktop**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/49.png

Without disconnecting any current sessions, log into StoreFront as **USER1** or **USER3** and launch applications or a Shared Windows Server Desktop.

In **Citrix Studio > Delivery Groups > XenApp DG > View Machines**, observe multiple sessions from multiple users leveraging the same shared compute assets.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/50.png

Takeaways
+++++++++

- MCS offers a unified workflow for deploying desktops, desktop applications, server desktops and server applications.
