Creating VMs in AHV from Disk Images
------------------------------------

Click **Home** in the navigation bar and select **VM** from the drop down menu.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/33.png

Click **+ Create VM** and fill out the following fields:

- **Name** - The display name of the VM being created, often confgiured to match the VM hostname
- **Description** - An optional field that is helpful for designating additional information about a VM, such as purpose or installed applications
- **vCPUs** - The number of virtual sockets presented to the VM
- **Number of Cores per vCPU** - The number of cores per virtual socket
- **Memory** - The amount of RAM presented to the VM in GB

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/34.png

Under **Disks**, click **Add New Disk**. Fill out the following fields and click **Add**:

- **Type** - *DISK*
- **Operation** - *Clone from Image Service*
- **Bus Type** - *SCSI*
- **Image** - The desired disk image from which to clone
- **Size** - Pre-populated based on the provisioned size of the selected image

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/35.png

Click **Save**.

In **Prism > VM > Table**, select the VM you've created and click **Power on** from the actions menu below the VM table.

Once the VM is powered on, click **Launch Console** to access your VM.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/36.png
