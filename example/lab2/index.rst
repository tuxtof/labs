.. _example-lab2:

------------------------
Lab 2 - More Style Guide
------------------------

Overview
++++++++

In this module you'll learn how Font Awesome icons can be used inline to help keep directions clear and consistent. Additionally you will insert command line instructions inline and include code from external files.

Using Icons
+++++++++++

The Prism UI includes several unlabeled icons. When directing a user to one of these actions or menus, use the example inline markup below:

In **Prism**, click :fa:`cog` **> Manage VM High Availability**.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab9/6.png

Under **Disks**, select :fa:`pencil` to change the **CD-ROM** device configuration.

.. figure:: http://s3.nutanixworkshops.com/vdi_ahv/lab11/3.png

Click :fa:`eject` to unmount the disk image from the **CD-ROM** device.

It is **not** necessary to include inline icons when the are accompanied by a label/text in the UI, such as :fa:`times` **Delete**

Codeblocks
++++++++++

You can insert code into your lab guides both inline and via external files. Inline is a great option for providing command line instructions or display shorter code snippets.

.. note:: `Here <http://www.sphinx-doc.org/en/stable/markup/code.html>`_ is a reference for even more code options available in Sphinx, including emphasizing individual lines. A list of languages available for markup can be found `here <http://pygments.org/docs/lexers/>`_.

Inline
......

Using an SSH client, execute the following:

.. code-block:: none
  :name: inline-code-example

  > ssh nutanix@<NUTANIX-CLUSTER-IP>
  > acli
  <acropolis> vm.create XD num_vcpus=4 num_cores_per_vcpu=1 memory=8G
  <acropolis> vm.disk_create XD cdrom=true empty=true
  <acropolis> vm.disk_create XD clone_from_image=<Windows 2012 Disk Image Name>
  <acropolis> vm.nic_create XD network=<Network Name> ip=<XD IP Address>
  <acropolis> vm.on XD

.. note:: When using **acli**, you can use the Tab key to autocomplete fields. Pressing Tab twice lists available namespaces and values.

The name option can be used to reference code blocks like this - :ref:`inline-code-example`

External File
.............

Use the literalinclude directive to create a code block from an external file. The best option when dealing with longer scripts or the content of the script is maintained separate from the lab guide.

.. literalinclude:: example.py
   :language: python
   :emphasize-lines: 2,7-8
   :linenos:
   :caption: EXAMPLE.PY
   :name: literal-include-example

Takeaways
+++++++++

- Here is where we summarize any key takeaways from the module
- Such as how a Nutanix feature used in the lab delivers value
- Or highlighting a differentiator
