Getting Started
===============

Lab Network Setup
-----------------

	 In the interest of focusing as much time as possible configuring and
	 troubleshooting APM, we have provided some resources and basic setup
	 ahead of time. These are:

	 -  Cloud-based lab environment complete with Jump Host, Virtual BIG-IP
	    (VE) and Lab Server

	 -  Duplicate Lab environments for each student for improved
	    collaboration

	 -  Virtual BIG-IP has been pre-licensed and provisioned for Access
	    Policy Manager (APM)

.. NOTE::
   All work for this lab will be performed exclusively from the Windows
   jumphost. No installation or interaction with your local system is
   required.

If you wish to replicate these labs in your office you will need to
perform these steps accordingly. Additional lab resources are provided
as illustrated in the diagram below:

LAB Environment Diagram
-----------------------

|image0|

Source: N/A

The following components have been included in your lab environment:
..  TODO:: VERIFY that this table is corect

- 2 x F5 BIG-IP VE (v12.1)
- 1 x F5 iWorkflow VE (v2.1)
- 1 x Linux LAMP Webserver (xubuntu 14.04)
- 1 x Windows Jumphost

Lab Components
--------------
.. TODO:: Complete lab components table

The following table lists VLANS, IP Addresses and Credentials for all components:

.. list-table::
    :widths: 20 40 40
    :header-rows: 1
    :stub-columns: 1

    * - **Component**
      - **VLAN/IP Address(es)**
      - **Credentials**
    * - Sample Host
      - - **Management:** 10.1.1.250
        - **Internal:** 10.1.10.250
        - **External:** 10.1.20.250
      - ``admin``/``admin``

.. |image0| image:: /_static/lab_image.png
	 :width: 13.10in
	 :height: 8.44in
