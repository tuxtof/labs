Lab 1 - Cluster Configuration
-----------------------------

Overview
++++++++

In this brief exercise you will use Prism to configure the storage and virtual network that will be used throughout the workshop.

Configuring Storage Container
+++++++++++++++++++++++++++++

In **Prism > Storage**, click **+ Storage Container**. Specify **Default** as the **Name** of the storage container.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/4.png

Click **Advanced Settings**. Select **Compression** and specify **0** as the **Delay**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/5.png

Click **Save**.

Configuring VM Network
++++++++++++++++++++++

Open **\https://<NUTANIX-CLUSTER-IP>:9440** in your browser to access **Prism**. Log in as a user with administrative priveleges.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/1.png

In Prism, select the **VM** page from the **Home** drop down menu.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab2/1.png

Click **Network Config > User VM Interfaces**, click **+ Create Network**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/1.png

Select **Enable IP Address Management** and fill out the following fields:

- **Name** - VM VLAN
- **VLAN ID** - *Refer to your Environment Details Worksheet*
- **Network IP Address/Prefix Length** - *Refer to your Environment Details Worksheet*
- **Gateway IP Address** - *Refer to your Environment Details Worksheet*
- **Domain Name Servers** - *Refer to your Environment Details Worksheet*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/2.png

Click **+ Create Pool** and fill out the following fields:

- **Start Address** - *Refer to your Environment Details Worksheet*
- **End Address** - *Refer to your Environment Details Worksheet*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/3.png

Click **Submit > Save**.

Takeaways
+++++++++

- Deploying storage in a Nutanix environment is a fast and simple operation. Large environments with mixed workloads can be deployed on a single storage container with no additional SAN administration overhead.

- Virtual networking in AHV provides distributed virtual switching out of the box, complete with integrated IP Address Management services.
