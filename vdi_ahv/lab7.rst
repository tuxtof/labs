Lab 7 - User Profiles With AFS
------------------------------

Overview
++++++++

In this exercise you will use Prism to deploy Acropolis File Services (AFS), a native, distributed file server solution for Nutanix clusters. The file server will be used to store user profiles and user data.

You will use Citrix Studio to configure and enforce policy for Citrix User Profile Management (UPM), a built-in profile management solution for XenDesktop. Citrix UPM can also be configured via Group Policy or .ini files within the desktop VM.

Deploy Acropolis File Services
++++++++++++++++++++++++++++++

In **Prism > File Server**, click **+ File Server**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/1.png

Click **Download or Upload AFS Software**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/2.png

Click **Download** to the right of the most recent release

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/3.png

After the download completes, click **Continue**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/4.png

If unconfigured, click **Add Data Services IP**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/5.png

Specify the **iSCSI Data Services IP** from your Environment Details Worksheet and click **Save**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/6.png

Click **Continue**.

.. note:: You may need to click **+ File Server** again to launch the AFS wizard if you had to configure a Data Services IP.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/7.png

Fill out the following fields and click **Next**:

- **Name** - *AFS*
- **File Server Size** *1 TiB*

.. note:: Clicking **Custom Configuration** will allow you to alter the scale up and scale out sizing of the AFS VMs based on User and Throughput targets.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/8.png

Select the **VM VLAN** you configured during the Cluster Configuration lab for the Client Network. Observe that IPs will be chosen for each of the File Server VMs from the DHCP pool. Selecting an Unmanaged VLAN requires you to manually provide static IP addresses for each of the File Server VMs. Ensure the **DNS IP** is configured as the IP of your **DC** VM. Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/9.png

Select the **VM VLAN** you configured during the Cluster Configuration lab for the Storage Network. Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/10.png

Specify the fully qualified domain and domain Adminisrator credentials. Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/11.png

Review the configuration and click **Create**. Observe that as part of the AFS deployment, a **home** share will be created by default for user profiles.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/12.png

Monitor deployment progress in **Prism > Tasks**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/13.png

After the AFS cluster has been deployed, validate you can access the **home** share from your **XD** or **DC** VMs.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/14.png

In **Prism > File Server > File Server**, select the **AFS** server and click **Protect**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/15.png

Note the default Self Service Restore schedules, this feature controls the snapshot schedule for Windows' Previous Versions functionality. Supporting Previous Versions allows end users to roll back changes to files without engaging storage or backup administrators. Note these local snapshots do not protect the file server cluster from local failures and that replication of the entire file server cluster can be performed to remote Nutanix clusters.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/16.png

In **Prism > File Server > Share**, select the **home** share and click **Update**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/17.png

Select **Enable Access Based Enumeration** and **Self Service Restore** and click **Save**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/18.png

.. note:: To learn more about AFS architecture, see the `File Services <http://nutanixbible.com/#anchor-file-services-85>`_ section of the Nutanix Bible.

Configuring Share Permissions
+++++++++++++++++++++++++++++

In the **DC** VM console, open **\\\\AFS** in **File Explorer**.

Right-click **Home > Properties**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/19.png

Select the **Security** tab and click **Advanced**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/20.png

Select the **Permissions** tab and click **Add**.

Click **Select a principal** and specify **Everyone** in the **Object Name** field. Click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/21.png

Fill out the following fields and click **OK**:

- **Type** - *Allow*
- **Applies to** - *This folder only*
- Select **Read & execute**
- Select **List folder contents**
- Select **Read**
- Select **Write**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/22.png

Click **OK > OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/23.png

Configuring Citrix User Profile Management
++++++++++++++++++++++++++++++++++++++++++

In **Citrix Studio > Policies**, right-click **Policies > Create Policy**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/24.png

Select **Profile Management** from the **All Settings** drop down menu. Optionally you can filter for only policies supported on **7.15 Desktop OS** from the **All Versions** drop down menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/25.png

 Search for **Enable Profile management** and click **Select**. Select **Enabled** and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/26.png

Search for **Path to user store** and click **Select**. Select **Enabled** and specify **\\\\AFS\home\\%USERNAME%\\!CTX_OSNAME!!CTX_OSBITNESS!** as the path. Click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/27.png

Click **Next**.

Click **Assign** to the right of **Organizational Unit (OU)**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/28.png

Click **Browse** and select the **Non-PersistentDesktops** OU. Click **OK > OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/29.png

Click **Next**.

Studio offers many different means of applying policies. Across a more diverse environment it may make sense to configure UPM settings based on tags. If our computers weren't optimally organized in Active Directory, we could have also elected to assign this policy based on the Non-Persistent desktop Delivery Group.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/30.png

Provide a friendly **Policy name** and select **Enable**. Review your configuration and click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/31.png

In order for the policy to take effect you could remotely execute a **gpupdate /force** command on the VMs in the **Non-PersistentDesktops** OU, or leverage the power management integration between XenDesktop and AHV to restart the VMs through **CItrix Studio**.

.. note:: Use acli for a quick (ungraceful) way to power off your non-persistent VMs. Studio will power on VMs again accoding to the Delivery Group's Power Management configuration.

  .. code::

      > acli vm.off W10NP*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/32.png

Testing Profiles and Folder Redirection
+++++++++++++++++++++++++++++++++++++++

Log in to Citrix StoreFront as **USER2** and connect to a **Pooled Windows 10 Desktop** with Citrix Receiver.

Make some simple changes such as adding files to your Documents folder and changing the desktop background. Note the hostname of the desktop to which you are connected.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab7/33.png

Sign out of the **Pooled** desktop. Do not just close the Citrix Receiver session as the desktop will not be re-provisioned.

Again, log in to Citrix StoreFront as **USER2** and connect to a **Pooled Windows 10 Desktop** with Citrix Receiver. Note that your files and settings persist across sessions, despite the underlying desktop being freshly provisioned every time you log in.

Open **\\\\\\AFS\\home\\user2\\** in **File Explorer**. Drill down into the directory structure to find the data associated with your user profile.

Log in to Citrix StoreFront as **USER3** and connect to a **Pooled Windows 10 Desktop** with Citrix Receiver. Open **\\\\\\AFS\\home\\** in **File Explorer**. Note that you don't see or have access to USER2's profile directory. Disable **Access Based Enumeration (ABE)** in **Prism > File Server > Share > home > Update** and try again.

Takeaways
+++++++++

- Nutanix provides native file services suitable for storing user profiles and data.

- AFS can be deployed on the same Nutanix cluster as your virtual desktops, resulting in better utilization of storage capacity and the elimination of an additional storage silo.

- Supporting mixed workloads (e.g. virtual desktops and file services) is further enhanced by Nutanix's ability to mix different node configurations within a single cluster, such as:

  - Mixing storage heavy and compute heavy nodes
  - Expanding a cluster with Storage Only nodes to increase storage capacity without incurring additional virtualization licensing costs
  - Mixing different generations of hardware (e.g. NX-3460-G6 + NX-6235-G5)
  - Mixing all flash nodes with hybrid nodes
  - Mixing NVIDIA GPU nodes with non-GPU nodes
