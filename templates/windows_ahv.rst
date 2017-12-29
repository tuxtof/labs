.. _windows-ahv-templates:

AHV Windows Templates
---------------------

Requirements
++++++++++++

-  A Nutanix cluster running AHV 20160925.91+ and AOS 5.1+
-  Windows installation .iso image, available via `MSDN Subscriber Downloads <https://msdn.microsoft.com/subscriptions/downloads>`_ or `Microsoft Evaluation Center <https://www.microsoft.com/en-us/evalcenter/>`_
-  Nutanix VirtIO for Windows .iso image available via `Nutanix Portal <https://portal.nutanix.com>`_
-  SSH client such as PuTTY

.. note:: As of AOS 5.1, Nutanix AHV supports the following Windows guest operating systems:

   - Windows Server 2008 R2, 2012, 2012 R2 and 2016
   - Windows 7, 8, 8.1, 10 (64-bit releases only)

Uploading Installation Media
++++++++++++++++++++++++++++

Open \https://<*NUTANIX-CLUSTER-IP*>:9440 in your browser to access **Prism**. Log in as a user with administrative priveleges.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/1.png

Click the Settings icon and select **Image Configuration** from the drop down menu.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/3.png

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/4.png

Click **Upload Image** and fill out the following fields:

- **Name** - *Nutanix VirtIO drivers* - This serves as the display name for the image being uploaded
- **Annotation** - This is an optional field that is helpful for designating additional information about an image, such as version or filename
- **Image Type** - *ISO* - This field differentiates a mountable CD-ROM image used for installing an OS from a cloneable disk image of a pre-installed OS
- **Storage Container** - *<Default Storage Container>* - This field specifies which Nutanix storage container to which the image will be uploaded
- **Image Source** - Images can either be uploaded from your local filesystem or can be obtained directly from a remote webserver

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/5.png

Click **Save** and wait for the image upload to Complete.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/6.png

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/7.png

Once prompted that the image was successfully created, click **Upload Image** to upload your desired OS .iso image.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/8.png

Once prompted that the image was successfully created, click **Close**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/9.png

Creating a Virtual Machine
+++++++++++++++++++++++++++

Click **Home** in the navigation bar and select **VM** from the drop down menu.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/10.png

Click **+ Create VM** and fill out the following fields:

- **Name** - The display name of the VM being created, often confgiured to match the VM hostname
- **Description** - An optional field that is helpful for designating additional information about a VM, such as purpose or installed applications
- **vCPUs** - The number of virtual sockets presented to the VM
- **Number of Cores per vCPU** - The number of cores per virtual socket
- **Memory** - The amount of RAM presented to the VM in GB

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/12.png

Under **Disks**, click the Pencil icon of the pre-populated CD-ROM drive.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/13.png

Fill out the following fields and click **Update**:

- **Operation** - *Clone From Image Services*
- **Bus Type** - *IDE*
- **Image** - Select the Windows installation .iso you uploaded to the Image Service

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/14.png

Under **Disks**, click **Add New Disk**. Fill out the following fields and click **Add**:

- **Type** - *CD-ROM*
- **Operation** - *Clone from Image Service*
- **Bus Type** - *IDE*
- **Image** - Select the Nutanix VirtIO drivers .iso you uploaded to the Image Service

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/15.png

Under **Disks**, click **Add New Disk**. Fill out the following fields and click **Add**:

- **Type** - *DISK*
- **Operation** - *Allocate on Storage Container*
- **Bus Type** - *SCSI*
- **Storage Container** - The storage container on which the virtual disk will be created
- **Size** - The size of the virtual disk in GiB

.. note:: All virtual disks created on AHV are thin provisioned.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/16.png

Select **Boot Device** for the CD-ROM device mounting your Windows installation media.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/17.png

Under **Network Adapters (NIC)**, click **Add New NIC**. Select a network from the **VLAN Name** drop down menu and click **Add**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/18.png

Click **Save**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/19.png

Installing Windows
++++++++++++++++++

In **Prism > VM > Table**, select the VM you've created and click **Power on** from the actions menu below the VM table.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/20.png

Once the VM is powered on, click **Launch Console** to access your VM.

.. note:: You may need to allow pop-ups in your browser for the VM console to appear.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/21.png

Proceed with Windows installation. When prompted for installation location, select **Load driver**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/22.png

Browse for the Nutanix VirtIO CD Drive and select the folder appropriate OS and architecture. Click **OK**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/23.png

Select all available drivers and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/24.png

Select your virtual disk (Drive 0) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/25.png

Complete the Windows installation. In **Prism > VM > Table**, select the VM you've created and click **Update**. Click the Eject icon next to each of the CD-ROM devices to unount your OS and VirtIO .iso images. Click **Save**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/26.png

.. _windows-ngt-install:

Installing Nutanix Guest Tools
++++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the VM you've created and click **Manage Guest Tools**. Select **Enable Nutanix Guest Tools** and **Mount Nutanix Guest Tools**. Click **Submit**.

.. note:: Nutanix Guest Tools can also be mounted programmatically with nCLI. Using nCLI or connecting to <NUTANIX-CLUSTER-IP> via SSH:

  .. code::

      > ncli vm list | grep <VM-Name> -B 2
      > ncli ngt mount vm-id=<VM-Id>

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/27.png

In your VM console, log in and launch setup.exe for the Nutanix Guest Tools installation.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/28.png

Accept the End User Licensing Agreement and click **Install**.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/29.png

Complete the installation, click **Close** and reboot the VM.

.. note:: Nutanix Guest Tools can also be installed silently via command line using the following syntax:

  .. code::

      DRIVE:\> setup.exe /quiet ACCEPTEULA=yes

.. note:: If you're prompted by the BIOS that the boot volume cannot be found, ensure your **scsi.0** disk is now configured as the **Boot Device**.

    .. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/30.png

Updates and Sysprep
+++++++++++++++++++

At this point you can patch your template image and install any other applications you'd like to include in your base disk image.

Following any patching or application installations, the next step in creating the template is to generalize the VM with Sysprep. In your VM console, open **Command Prompt** as **Administrator** and run the following command:

.. code::

  > sysprep.exe /generalize /oobe /shutdown

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/31.png

Copying Template to Image Service
+++++++++++++++++++++++++++++++++

Once the template VM has powered off following sysprep, we want to push the associated disk image to the Image Service for our cluster. Using an SSH client, execute the following:

.. code::

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > acli
  <acropolis> image.create <IMAGE-NAME> clone_from_vmdisk=vm:<VM-NAME>:scsi.0 image_type=kDiskImage annotation="<IMAGE-ANNOTATION>"

Verify in Prism that the image is available and in an Active state.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/32.png
