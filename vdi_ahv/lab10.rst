.. _ssflr_lab:
*Optional Lab* - Self-Service File Level Restore
------------------------------------------------

Overview
++++++++

In this exercise you will configure a snapshot policy for your persistent virtual desktops and test restoring deleted files as an end user.

Protecting the VMS
++++++++++++++++++

In **Prism > Data Protection**, click **+ Protection Domain > Async DR**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/1.png

Specify a **Name** for the Protection Domain (e.g. Persistent-Desktops) and click **Create**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/2.png

Select all **W10P-###** VMs and click **Protect Selected Entities > Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/3.png

Click **New Schedule** and fill out the following fields:

- **Repeat every X hour(s)** - *4*
- **Keep the last X snapshots** - *12*

Click **Create Schedule > Close**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/4.png

Staging the Test
++++++++++++++++

Log in to Citrix StoreFront again as **USER2** and launch your **Personal** desktop. Create some new directories and files (e.g. downloading images, creating text documents, etc.) as shown below.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/5.png

In **Prism > Data Protection > Table**, select the **Persistent-Desktops** Protection Domain.

Select the **Local Snapshots** tab and onserve that there is already 1 snapshot present that was taken when the Protection Domain was created. Rather than waiting 4 hours for another snapshot to be taken automatically, we will force a snapshot by clicking **Take Snapshot**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/6.png

As the snapshot isn't being replicated to a remote site we don't need to alter **Replication Start Time**. Select a **Retention Time** of **1 day** and click **Save**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/7.png

There is now an additional local snapshot available.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/8.png

In **Prism > VM > Table**, confirm that both Nutanix Guest Tools and Self Service Restore (SSR) are enabled for your Persistent desktops. This may require a restart of your VM if NGT has not yet been enabled.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/9.png

Performing Self Service Restore
+++++++++++++++++++++++++++++++

Log in to Citrix StoreFront again as **USER2** and launch your **Personal** desktop. Delete the files you had previously created.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/10.png

Launch **Nutanix SSR** from the desktop.

Log in using your Windows credentials.

.. note:: In order to access Nutanix SSR, the user needs to be part of the local Administrators group. The following section covers how to create a Group Policy Object to add domain users to the local Administrators group of all Computers in an OU. Alternatively you can complete this exercise using the local Administrator credentials (e.g. **LocalUser**). The credentials used for Nutanix SSR do not need to be that of the currently logged in user.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/11.png

Select the most recent snapshot **prior** to deleting your files.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/12.png

Select **Disk 0** and select **Mount** from the **Disk Action** drop down menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/13.png

Note the drive letter used for the mounted snapshot.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/14.png

In **File Explorer**, browse to the location of your files on the mounted snapshot drive.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/15.png

Copy and paste the files back to their original location on the C: drive. Return to the Nutanix SSR console and select **Mounted Snapshots** from the navigation bar. Select **Disk 0** and click **Unmount**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/16.png

Takeaways
+++++++++

- Nutanix provides native snapshot and replication capabilities that can be configured at the individual VM level. Cluster to cluster replication is a key component in enabling DR for VDI components such as gold images and persistent virtual desktops.

- Self-Service File Level Restore allows an end user to restore deleted or previous versions of files without storage or backup admin engagement. This may be applicable for persistent desktop users who may inadvertently store files in local directories.

- Self-Service File Level Restore mounts the native disk directly to the guest VM, preserving the original filesystem and security ACLs.

*Optional* - Making Domain Users Local Admins
+++++++++++++++++++++++++++++++++++++++++++++

In the **DC** VM console, launch **Control Panel > Administrative Tools > Group Policy Management**.

Right-click the **PersistentDesktops** OU and select **Create a GPO in this domain, and Link it here…**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/17.png

Specify a **Name** (e.g. Local Admin GPO) and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/18.png

Right-click on the GPO and select **Edit…**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/19.png

Open **Computer Configuration > Policies > Windows Settings > Security Settings**. Right-click **Restricted Groups > Add Group**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/20.png

Specify **Administrators** and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/21.png

Click **Add…** to the right of **Members of this group**.

Specify **<DOMAIN>\USER1; <DOMAIN>\USER2** and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/22.png

Click **Add…** to the right of **This group is a member of**.

Specify **Administrators** Security Group and click **OK > OK**.

This additional step maintains all current members of the Local Administrators group, rather than replacing them with only the PersistentUsers Security Group.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/23.png

Close **Group Policy Management Editor**.

Right-click the GPO and select **Enforced**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/24.png

After enforcing the GPO, log off or restart your Persistent desktops.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab10/25.png
