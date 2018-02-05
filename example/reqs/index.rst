Requirements
------------

.. This section is a required part of any workshop and should list requirements such as node configuration, hypervisor and AOS version, templates, software, network configuration, and IP addresses

Nutanix Hosted Cluster
++++++++++++++++++++++

To complete this workshop you will be provided with the following resources:

- A Nutanix cluster running AOS 5.1+ and AHV 20160925.90+
- Environment Details Worksheet - *Contains environment IP addresses and credentials*

  .. note:: The subnet IP scheme provided in your Environment Details Worksheet will be used for the static VM IPs detailed below.

- Windows Server 2012 R2 & Windows 10 templates - *Available on \\\\AFC-POCFS\\Workshops\\Templates*
- Required software - *Available on \\\\AFC-POCFS\\Workshops\\Example\\Software*
  - Matt's Cool Tool 1.0
  - Some Other Thing 2.2
  - Radical Plugin 0.1
- IP addresses

  ============== ================ ===============
  **VM Name**    **IP**           **Description**
  DC             10.XXX.YYY.2     Domain Controller
  PC             10.XXX.YYY.3     Prism Central
  VM1            10.XXX.YYY.4     Something
  ============== ================ ===============

Environment Setup
.................

.. The setup section should direct the user to any generic guides such as AD or PC deployment used across multiple workshops. Any additional custom setup instructions should be created in the reqs/ subdirectory and referenced here.

If not already completed, follow the exercises below prior to beginning :ref:`example-lab1`:

  - :ref:`prism-element-setup`
  - :ref:`prism-central-setup`
  - :ref:`active-directory-setup`
  - :ref:`custom-staging`

Other Nutanix Cluster
+++++++++++++++++++++

To complete this workshop using a non-Nutanix Hosted environment, you'll need access to the following resources:

- A Nutanix cluster running AOS 5.1+ and AHV 20160925.90+
- Windows Server 2012 R2 & Windows 10 templates

.. note:: Windows installation media can be downloaded for trial installs through `MSDN Subscriber Downloads <https://msdn.microsoft.com/subscriptions/downloads>`_ or `Microsoft Evaluation Center <https://www.microsoft.com/en-us/evalcenter/>`_
  See :ref:`windows-ahv-templates` for complete instructions on creating your own templates for the Nutanix Image Service.

- Some third party software

.. note:: Required third party software can be downloaded for free evaluation with creation of a XYZ account `here <https://www.google.com/>`_.

- Some Nutanix software

.. note:: Nutanix plugins are available for download on the `Nutanix Portal Download page <https://portal.nutanix.com/#/page/static/supportTools>`_. If you are not a current customer or partner with an active support subscription, contact sales@nutanix.com to be put in contact with a Systems Engineer for assistance.

- An available VLAN **without** DHCP (AHV IP Address Management will be configured as part of the workshop)

- Static IP addresses in the aforementioned VLAN:

  ============== ===============
  **VM Name**    **Description**
  DC             Domain Controller
  PC             Prism Central
  VM1            Something
  ============== ===============

Environment Setup
.................

If not already completed, follow the exercises below prior to beginning :ref:`example-lab1`:

  - :ref:`prism-element-setup`
  - :ref:`prism-central-setup`
  - :ref:`active-directory-setup` - *Existing AD environment can be supplemented*
  - :ref:`custom-staging`
