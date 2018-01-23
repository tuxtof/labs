Requirements
------------

If this workshop is being completed as part of a Nutanix or Nutanix partner sponsored event, all necessary software, templates, credentials and networking information will be provided to you.

If this workshop is being completed outside of the scenario described above, you'll need access to the following resources to complete all modules within the workshop:

- A Nutanix cluster running AOS 5.1+ and AHV 20160925.90+
- Windows Server 2012 R2 & Windows 10 templates

.. note:: Windows installation media can be downloaded for trial installs through `MSDN Subscriber Downloads <https://msdn.microsoft.com/subscriptions/downloads>`_ or `Microsoft Evaluation Center <https://www.microsoft.com/en-us/evalcenter/>`_
  See :ref:`windows-ahv-templates` for complete instructions on creating your own templates for the Nutanix Image Service.

- Citrix XenDesktop 7.15 LTSR installation .iso

.. note:: Citrix software can be downloaded for free evaluation with creation of a Citrix account `here <https://www.citrix.com/welcome/create-account.html>`_.

- XenDesktop MCS Plugin for Nutanix AHV

.. note:: Nutanix Citrix plugins are available for download on the `Nutanix Portal Download page <https://portal.nutanix.com/#/page/static/supportTools>`_. If you are not a current customer or partner with an active support subscription, contact sales@nutanix.com to be put in contact with your friendly, neighborhood Systems Engineer for assistance.

- An available VLAN **without** DHCP (AHV IP Address Management will be configured as part of the workshop)

- Static IP addresses in the aforementioned VLAN per the table below:

  ============== ============ ===============
  **VM Name**    **# of IPs** **Description**
  DC             1            Domain Controller
  XD             1            XenDesktop Delivery Controller, StoreFront, Citrix Licensing
  NSVPX          3            Optional NetScaler Secure Gateway
  ============== ============ ===============
