Troubleshooting Labs
=============================================
There are four labs that cover this class:

.. toctree::
   :maxdepth: 1
   :glob:

   lab*


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

If you wish to replicate these labs in your office you will need to
perform these steps accordingly. Additional lab resources are provided
as illustrated in the diagram below:

LAB Environment Diagram

|image0|

Source: N/A

Timing for Labs
---------------

The time it takes to perform each lab varies and is mostly dependent on
accurately completing steps. This can never be accurately predicted but
we strived to derive an estimate among several people each having a
different level of experience. Below is an estimate of how long it will
take for each lab:

LAB Timing

+-----------------------------------------------+------------------+
| LAB Name (Description)                        | Time Allocated   |
+===============================================+==================+
| LAB I (APM Troubleshooting Lab Object Prep)   | 20 minutes       |
+-----------------------------------------------+------------------+
| LAB II (General Troubleshooting)              | 5 minutes        |
+-----------------------------------------------+------------------+
| LAB III                                       | 20 minutes       |
+-----------------------------------------------+------------------+
| LAB IV                                        | 25 minutes       |
+-----------------------------------------------+------------------+

General Notes
-------------

Provisioning Access Policy Manager (APM) is not required for basic
Access Policy uses cases although this has been provisioned for you
ahead of time. This was done to save time as provisioning often requires
services to restart which takes away valuable lecture/lab time.

Accessing the Lab Environment
-----------------------------

To access the lab environment, you will require a web browser and Remote
Desktop Protocol (RDP) client software. The web browser will be used to
access the Lab Training Portal. The RDP client will be used to connect
to the Jump Host, where you will be able to access the BIG-IP management
interfaces (HTTPS, SSH).

Your class instructor will provide additional lab access details.

How to use this Guide
---------------------

For each section, follow the instruction of the class moderator on when
to begin. Carefully read and implement each item step by step. Archives
have been provided for each completed section and can be loaded if
necessary at the beginning of each section for prior labs. You can
install the UCS archive by using the \ **tmsh no-license** option. For
the command syntax, refer to the following example:

    tmsh load sys ucs [ucs file name] no-license


.. |image0| image:: media/image2.tiff
   :width: 6.48475in
   :height: 4.17870in
