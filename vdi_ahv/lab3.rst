Lab 3 - XenDesktop Install
-------------------------------

Overview
++++++++

In this exercise you will use the CLI to deploy a Windows 2012 template image and install backend services for XenDesktop. For the purposes of simplifying the workshop, all key XenDesktop roles (Studio, Delivery Controller, StoreFront, Citrix Licensing) will be installed in a single VM.

As previously discussed, in a production environment these roles would be sepearated on multiple VMs to provide scalbility and high availability. Additionally, you will install the MCS Plugin for AHV on the Delivery Controller VM. This plugin leverages the Citrix Provisioning SDK to allow Citrix Machine Creation Service to deploy and configure VMs running on Nutanix AHV. In a production environment the MCS Plugin would be deployed on each Delivery Controller VM.

Creating the VM (via CLI)
+++++++++++++++++++++++++

Using an SSH client, execute the following:

.. code:: bash

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > acli
  <acropolis> vm.create XD num_vcpus=4 num_cores_per_vcpu=1 memory=8G
  <acropolis> vm.disk_create XD cdrom=true empty=true
  <acropolis> vm.disk_create XD clone_from_image=<Windows 2012 Disk Image Name>
  <acropolis> vm.nic_create XD network=<Network Name> ip=<XD IP Address>
  <acropolis> vm.on XD

.. note:: When using **acli**, you can use the Tab key to autocomplete fields. Pressing Tab twice lists available namespaces and values.

In **Prism > VM > Table**, select the **XD** VM and click **Launch Console**.

Complete the Microsoft Out of Box Experience (OOBE) wizard:

- Select Country, App language, and Keyboard layout, click **Next**.

- Review the licensing agreement, click **I Accept**.

- Enter a password for the local Administrator account (e.g. nutanix/4u), click **Next**.

Log in to the **XD** VM as **Administrator**.

Enable Nutanix Guest Tools (via CLI)
++++++++++++++++++++++++++++++++++++

Using an SSH client, execute the following:

.. code:: bash

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > ncli vm list | grep XD -B 1
  > ncli ngt mount vm-id=<VM-UUID-OUTPUT-FROM-PREVIOUS-COMMAND>

If not installed in the base image, install NGT within the guest console (See :ref:`windows-ngt-install`). Restart **XD** VM.

.. code:: bash

  > ncli ngt get vm-id=<VM UUID>

When NGT has been successfully enabled, **Communcation Link Active** will be set to true

.. code:: bash

  > ncli ngt unmount vm-id=<VM UUID>

Preparing XenDesktop VM
+++++++++++++++++++++++

In the **XD** VM console, open **PowerShell** and execute the following to update the hostname and join the NTNX domain:

.. code:: bash

  PS > Add-Computer -DomainName NTNX.local -NewName XD -Restart

When prompted, provide your NTNX\\Administrator credentials and click **OK**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/1.png

After the VM restarts, log in as **NTNX\\Administrator**.

.. note:: If not using a fully patched Windows 2012 R2 image, you will need to install Microsoft patch `KB2919355 <https://www.microsoft.com/en-us/download/details.aspx?id=42334>`_ before proceeding with XenDesktop installation (`CTX220340 <https://support.citrix.com/article/CTX220340>`_).

Installing XenDesktop
+++++++++++++++++++++

Using an SSH client, execute the following:

.. code:: bash

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > acli
  <acropolis> vm.disk_update XD ide.0 clone_from_image=<XD ISO Name>

In the **XD** VM console, open the XenDesktop Installer and click the **Start** button to the right of **XenDesktop**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/2.png

Click **Get Started**

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/3.png

Accept the licensing agreement and click **Next**.

Select all components (Delivery Controller, Studio, Director, License Server, and StoreFront) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/4.png

Select all features (Microsoft SWL Server 2014 Express, Windows Remote Assistance) and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/5.png

Select **Automatically** to allow the installer to configure the Windows Firewall service to allow traffic for selected XenDesktop components.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/6.png

Review selections and click **Install**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/7.png

During installation of prerequisites you will be prompted to restart the VM, click **Close** and allow the VM to restart. After restart, log in as **NTNX\\Administrator** and the installation will continue.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/8.png

After installation, select **I do not want to connect to Smart Tools or Call Home** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/9.png

Click **Finish** to complete the installation.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/10.png

Installing MCS Plugin for AHV
+++++++++++++++++++++++++++++

Copy the **NutanixAcropolix-MCS-XD7.9_or_later.msi** file to the **XD** VM.

In the **XD** VM console, open the installer and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/12.png

Select **I accept the terms in the License Agreement** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/13.png

Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/14.png

Click **Install**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/15.png

Click **Finish**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/16.png

Configuring XenDesktop Site
+++++++++++++++++++++++++++

In the **XD** VM console, open **Citrix Studio** from the Start Menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/28.png

After launching Citrix Studio for the first time, you'll be prompted with three different options to configure your newly installed Delivery Controller. Click **Deliver applications and desktops to your users**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/17.png

Select **A fully configured, production-ready Site**, specify a **Site name** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/18.png

Observe the three separate databases being created and their locations, utilizing the local SQL Express instance we installed. Click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/19.png

Select **Use the free 30-day trial** and click **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/20.png

Fill out the following fields to configure the connection to your AHV cluster and click **Next**:

- **Connection Type** - *Nutanix AHV*
- **Connection Address** - Refer to your Environment Details Worksheet
- **User Name** - *admin*
- **Password** - Refer to your Environment Details Worksheet
- **Connection Name** - Specify a friendly name for the connection, such as your Nutanix cluster name

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/21.png

Specify a name for the network and storage resources for the cluster (e.g. the Nutanix cluster name), and select the IPAM enabled network created during the **Cluster Configuration** lab. This will be the DHCP pool used by provisioned desktops and XenApp servers.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/22.png

The workshop will not explore Citrix's AppDNA or App-V Publishing features, so both can be left unselected before clicking **Next**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/23.png

Review selections and click **Finish** to start site creation.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/24.png

Once complete, Studio will indicate that configuration was successful.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/25.png

Selecting the **PowerShell** tab will detail all the steps taken by the Site Creation Wizard.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/26.png

Select **Citrix StoreFront > Stores** and review the configuration.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/27.png

Takeaways
+++++++++

- The Nutanix MCS Plugin provides an integrated and fully supported experience for adding AHV clusters in Citrix Studio.

- Both Citrix and Nutanix support multiple hypervisors, including AHV, vSphere, Hyper-V, and XenServer. Choice of hypervisor and hardware platform with Nutanix prevents a business from being locked in to a single solution.

- The **acli** and **ncli** tools provide a programmatic interface for infrastructure and VM operations.

  .. note:: Nutanix also provides SDKs and fully documented APIs. See `developer.nutanix.com <http://developer.nutanix.com>`_ for API reference, code samples, and more. You can also find an interactive API Explorer within Prism under the <Username> drop down menu.

    .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab3/29.PNG
