-------------------
Lab 1 - Example Lab
-------------------

Overview
++++++++

Here is where we provide a high level description of what the user will be doing during this module.

Configuring Something
+++++++++++++++++++++

Open \https://<*NUTANIX-CLUSTER-IP*>:9440 in your browser to access **Prism**. Log in as a user with administrative priveleges.

.. figure:: http://s3.nutanixworkshops.com/templates/ahv_windows/1.png

Click **Network Config > User VM Interfaces**, click **+ Create Network**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/1.png

Select **Enable IP Address Management** and fill out the following fields:

- **Name** - VM VLAN
- **VLAN ID** - *Refer to your Environment Details Worksheet*
- **Network IP Address/Prefix Length** - *Refer to your Environment Details Worksheet*
- **Gateway IP Address** - *Refer to your Environment Details Worksheet*

  .. note:: Here is a note inserted in a list

- **Domain Name Servers** - *Refer to your Environment Details Worksheet*

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/2.png

Click **Submit > Save**.

.. note:: Here is a note that contains an image

  .. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/2.png

Doing Something Else
++++++++++++++++++++

In **Prism > Storage**, click **+ Storage Container**. Specify **Default** as the **Name** of the storage container.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/4.png

Click **Advanced Settings**. Select **Compression** and specify **0** as the **Delay**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab1/5.png

Click **Save**.

Here's a code snippet:

.. code:: python

  if hasattr(obj, 'attr_name'):
    # obj.attr_name exists.

Takeaways
+++++++++

- Here is where we summarize any key takeaways from the module
- Such as how a Nutanix feature used in the lab delivers value
- Or highlighting a differentiator
