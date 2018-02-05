.. _example-lab1:

-------------------
Lab 1 - Style Guide
-------------------

Overview
++++++++

Here is where we provide a high level description of what the user will be doing during this module. In this module we'll look at basic

Text and Figures
++++++++++++++++

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

Notes
+++++

Notes provide easily noticable interjections to lab instructions. Reasons to use a note include calling attention to a step that requires additional context or referencing external resources. Make sure to include a return before and after the note directive for it to render properly. **Please don't abuse notes.**

.. note:: Check out `this <http://openalea.gforge.inria.fr/doc/openalea/doc/_build/html/source/sphinx/rest_syntax.html>`_ cheat sheet for helpful documentation on formatting text, links, tables, etc. in Restructured Text.

Takeaways
+++++++++

- Here is where we summarize any key takeaways from the module
- Such as how a Nutanix feature used in the lab delivers value
- Or highlighting a differentiator
