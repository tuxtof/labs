Lab 2 - Domain Controller Install
---------------------------------

Overview
++++++++

Creating the VM
+++++++++++++++

In **Prism > VM**, click **+ Create VM** and fill out the following fields:

- **Name** - *DC*
- **Description** - *Domain Controller*
- **vCPU** - *2*
- **Number of Cores per vCPU** - *1*
- **Memory** - *4*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/2.png

Click **+ Add New Disk** and fill out the following fields:

- **Type** - *DISK*
- **Operation** - *Clone from Image Service*
- **Bus Type** - *SCSI*
- **Image** - Select your Windows 2012 R2 Disk Image

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/3.png

Click **Add**. Click **Add New NIC**.

Select **VM VLAN** from the **VLAN Name** drop down menu and click **Add**. Click **Save**.

In **Prism > VM > Table**, select the **DC** VM and click **Power on**.

Once the VM has started, click **Launch Console**.

.. note:: You may need to allow pop-ups in your browser for the VM console to appear.

Complete the Microsoft Out of Box Experience (OOBE) wizard:

- Select Country, App language, and Keyboard layout, click **Next**.

- Review the licensing agreement, click **I Accept**.

- Enter a password for the local Administrator account (e.g. nutanix/4u), click **Next**.

Log in to the **DC** VM as **Administrator**.

Enabling Nutanix Guest Tools
++++++++++++++++++++++++++++

In **Prism > VM > Table**, select the **DC** VM and click **Manage Guest Tools**.

Select **Enable Nutanix Guest Tools** and **Mount Nutanix Guest Tools**, click **Submit**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/4.png

.. note:: If the Nutanix Guest Tools have already been installed into the base image, the CD-ROM image will be automatically unmounted once the Nutanix Guest Agent service is able to communicate with the Nutanix CVM. A reboot of the VM or restart of the Nutanix Guest Tools Agent may be required. If the Nutanix Guest Tools have not yet been installed into the base image, see :ref:`windows-ngt-install` for complete instructions.

Configuring IP and Hostname
+++++++++++++++++++++++++++

In the **DC** VM console, open **Control Panel > Network & Internet > Network & Sharing Center > Change adapter settings**.

Right-click **Ethernet > Properties**.

Select **Internet Protocol Version 4 (TCP/IPv4) > Properties**.

Select **Use the following IP Address** and fill out the following fields:

- **IP Address** - Refer to your Environment Details Worksheet
- **Subnet Mask** - Refer to your Environment Details Worksheet
- **Default Gateway** - Refer to your Environment Details Worksheet
- **Preferred DNS Server** - Refer to your Environment Details Worksheet

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/5.png

Click **OK**.

Open **PowerShell** and execute the following commands to change the hostname and restart the VM:

.. code::

  PS > Rename-Computer -NewName DC -ComputerName .
  PS > shutdown /r /t 0

Installing Active Directory
+++++++++++++++++++++++++++

In the **DC** VM console, open **Server Manager > Add roles and features > Next**.

Select **Role-based or feature-based installation > Next**.

Select **Select a server from the server pool > DC > Next**.

From the Roles list, select **Active Directory Domain Services > Add Features**.

From the Roles list, select **DNS Server > Add Features > Next  > Next > Next > Install**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/6.png

Following a successful installation, click **Promote this server to a domain controller**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/7.png

In the Active Directory Domain Services Configuration Wizard, select **Add a new forest** and specify *NTNX.local* as the **Root domain name**. Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/8.png

Specify a Directory Services Restore Mode password (e.g. nutanix/4u) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/9.png

Ignore the DNS delegation warning, as we're using AD integrated DNS, and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/10.png

Accept default NetBIOS domain name (NTNX) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/11.png

Accept default AD database, log files, and SYSVOL folders and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/12.png

Review your configuration and click **Next > Install**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/14.png

After the **DC** VM restarts, log in as **NTNX\\Administrator**.

Creating User Accounts
++++++++++++++++++++++

In the **DC** VM console, open **Control Panel > Administrative Tools > Active Directory Users & Computers**.

Right-click **Users > New > User**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/15.png

Fill out the following fields and click **Next**.

- **First Name** - *USER1*
- **User logon name** - *USER1*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/16.png

Specify a user password, de-select **User must change password at next logon**, and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/17.png

Repeat these steps to create accounts for *USER2* and *USER3*.

Creating Organizational Units
+++++++++++++++++++++++++++++

Right-click **Domain Name > New > Organizational Unit**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/18.png

Specify *Persistent Desktops* as the OU **Name** and click **OK**.

Repeat these steps to create an OU for *Non-Persistent Desktops* and *XenAppServers*.

Creating Security Groups
++++++++++++++++++++++++

Right-click **Users > New > Group**.

Specify *PersistentUsers* as the **Group name** and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/19.png

Right-click **PersistentUsers > Properties**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/20.png

Select the **Members** tab and click **Add**. Specify *USER1;USER2;* in the **Object names** field and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/21.png

Repeat these steps to create a *NonPersistentUsers* group containing *USER2* and *USER3*

Repeat these steps to create a *XenAppUsers* group containing *USER1*, *USER2*, and *USER3*

Updating IPAM Settings
++++++++++++++++++++++

In **Prism > VM > Network Config > User VM Interfaces**, click the **Pencil** icon for **VM VLAN**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/22.png

Update **Domain Name Servers** to reflect the IP address of the **DC** VM.

Update **Domain Search** with your fully qualified root domain, *NTNX.local*.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/23.png

Click **Save**.
