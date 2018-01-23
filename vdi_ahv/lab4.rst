Lab 4 - Windows 10 Gold Image
-------------------------------

Overview
++++++++

In this exercise you will use the Prism to deploy a Windows 10 template image to be used as a Master (often referred to as "Gold") image for XenDesktop. Once the image has been created, you will snapshot the VM in Prism.

Creating the VM
+++++++++++++++

In **Prism > VM**, click **+ Create VM** and fill out the following fields:

- **Name** - *W10-Gold*
- **Description** - *Windows 10 x64 XenDesktop Gold Image*
- **vCPU** - *2*
- **Number of Cores per vCPU** - *1*
- **Memory** - *4*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/1.png

Click **+ Add New Disk** and fill out the following fields:

- **Type** - *DISK*
- **Operation** - *Clone from Image Service*
- **Bus Type** - *SCSI*
- **Image** - Select your Windows 10 Disk Image

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/2.png

Click **Add**. Click **Add New NIC**.

Select **VM VLAN** from the **VLAN Name** drop down menu and click **Add**. Click **Save**.

In **Prism > VM > Table**, select the **DC** VM and click **Power on**.

Once the VM has started, click **Launch Console**.

.. note:: You may need to allow pop-ups in your browser for the VM console to appear.

Complete the Microsoft Out of Box Experience (OOBE) wizard:

- Select Country, App language, and Keyboard layout, click **Next**.

- Review the licensing agreement, click **I Accept**.

- Click **Customize**.

- Set all features to **Off** and click **Next**.

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/3.png

- Set all features to **Off** and click **Next**.

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/4.png

- Set all features to **Off** and click **Next**.

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/5.png

- Select **Join a local Active Directory domain** and click **Next**.

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/6.png

- Create a *LocalUser* accout and click **Next**.

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/7.png

- Contemplate why this workshop wasn't built with a Windows 7 image and click **Not Now** to disable Cortana

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/8.png

- Click **Next**.

Log in to **W10-Gold** as **LocalUser**.

Enabling Nutanix Guest Tools
++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **W10-Gold** VM and click **Manage Guest Tools**.

Select **Enable Nutanix Guest Tools** and **Mount Nutanix Guest Tools**, click **Submit**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/4.png

.. note:: If the Nutanix Guest Tools have already been installed into the base image, the CD-ROM image will be automatically unmounted once the Nutanix Guest Agent service is able to communicate with the Nutanix CVM. A reboot of the VM or restart of the Nutanix Guest Tools Agent may be required. If the Nutanix Guest Tools have not yet been installed into the base image, see :ref:`windows-ngt-install` for complete instructions.

Installing Applications
+++++++++++++++++++++++

In the **W10-Gold** VM console, install a few applications such as Google Chrome, text editors, etc.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/10.png

Disable Windows Updates for your gold image by opening **Control Panel > Administrative Tools > Services**.

Right-click **Windows Update > Properties**.

Select **Disabled** from the **Startup type** down down menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/11.png

Click **Stop**.

Click **OK**.

Installing Virtual Delivery Agent
+++++++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **W10-Gold** VM and click **Update**.

Under **Disks**, eject any images currently attached to the CD-ROM device and select the **Pencil** icon to change the configuration.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/12.png

Fill out the following fields and click **Update**:

- **Operation** - *Clone from Image Service*
- **Image** - Select your XenDesktop installation .iso

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/13.png

Click **Save**.

In the **W10-Gold** VM console, open the XenDesktop Installer and click the **Start** button to the right of **XenDesktop**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/14.png

Click **Virtual Delivery Agent for Windows Desktop OS**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/15.png

Select **Create a Master Image** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/16.png

Select **No, install VDA in standard mode** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/17.png

Select **Citrix Receiver** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/18.png

Select all **Additional Components** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/19.png

Select **Let Machine Creation Services do it automatically** from the drop down menu and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/20.png

Select **Optimize performance** (`CTX125874 <https://support.citrix.com/article/CTX125874>`_) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/21.png

Select **Automatically** to allow the installer to configure the Windows Firewall service to allow traffic for selected XenDesktop components.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/22.png

Review selections and click **Install**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/23.png

Select **I do not want to participate in Call Home** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/24.png

Click **Finish** and wait for the VM to restart.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/25.png

In **Prism > VM > Table**, select the **W10-Gold** VM and click **Update**.

Under **Disks**, eject the XenDesktop installation .iso currently attached to the CD-ROM device.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/26.png

Click **Save**.

Creating Gold Image Snapshot
++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **W10-Gold** VM and click **Power Off Actions**.

Select **Guest shutdown** and click **submit** to gracefully shut down the VM.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/27.png

Once **W10-Gold** is powered off, select the VM and click **Take Snapshot**

Specify a **Name** for the snapshot (e.g. "W10-Gold vYYYYMMDD-X - Post-VDA 7.15 Install") and click **Submit**.

Available snapshots and associated actions can be found by selecting the **VM Snapshots** tab under the VM table.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab4/28.png

Takeaways
+++++++++

- The gold VM does not require Sysprep or being domain joined.

- Using MCS helps simplify the gold image by not having to manually specify (or depend on Active Directory to specify) what XenDesktop Delivery Controller(s) with which the image should attempt to register. This allows more flexibility in having a single gold image support multiple environments without external dependencies.
