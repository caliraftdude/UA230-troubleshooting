Agility 2017 Hands-on Lab Guide

UA 230 Troubleshooting Universal Access

Presented by: Buu Lam, Dan Holland, Jason Chiu, Jeff Nalbone

What’s inside

`Welcome 1 <#welcome>`__

`Lab Network Setup 1 <#lab-network-setup>`__

`Timing for Labs 2 <#timing-for-labs>`__

`General Notes 3 <#general-notes>`__

`Accessing the Lab Environment 3 <#accessing-the-lab-environment>`__

`How to use this Guide 3 <#how-to-use-this-guide>`__

`LAB I – APM Troubleshooting Lab Object Preparation
4 <#lab-i-apm-troubleshooting-lab-object-preparation>`__

`Connect to the Lab 5 <#connect-to-the-lab>`__

`DNS Resolver for System Configuration
6 <#dns-resolver-for-system-configuration>`__

`NTP Server for System Configuration 8 <#_Toc482272862>`__

`Access Policy (APM) AAA Server – Active Directory Object Creation
10 <#access-policy-apm-aaa-server-active-directory-object-creation>`__

`Access Policy (APM) SSO Configuration – NTLMv1
12 <#access-policy-apm-sso-configuration-ntlmv1>`__

`Access Policy (APM) Access Profile Creation
14 <#access-policy-apm-access-profile-creation>`__

`Local Traffic (LTM) Pool and Member Creation
16 <#local-traffic-ltm-pool-and-member-creation>`__

`Local Traffic (LTM) Virtual Server Creation
18 <#local-traffic-ltm-virtual-server-creation>`__

`LAB II – General Troubleshooting
21 <#lab-ii-general-troubleshooting>`__

`Questions to ask yourself 21 <#questions-to-ask-yourself>`__

`Verify DNS is setup from the CLI of the BIG-IP
22 <#verify-dns-is-setup-from-the-cli-of-the-big-ip>`__

`Verify NTP is setup from the CLI of the BIGIP 25 <#_Toc482272871>`__

`Manage Sessions within the Access Policy Manager menu
26 <#manage-sessions-within-the-access-policy-manager-menu>`__

`LAB III Visual Policy Editor (VPE) & Session Variables
39 <#lab-iii-visual-policy-editor-vpe-session-variables>`__

`Questions to ask yourself 39 <#questions-to-ask-yourself-1>`__

`Visual Policy Editor (VPE) Workflow, Actions, Branches, Endings
40 <#visual-policy-editor-vpe-workflow-actions-branches-endings>`__

`Pausing the APM Policy Execution for Troubleshooting – The Message Box
42 <#pausing-the-apm-policy-execution-for-troubleshooting-the-message-box>`__

`Session Variables – Setting and Retrieving (Some Quick Information)
54 <#session-variables-setting-and-retrieving-some-quick-information>`__

`LAB IV – Command Line Tools 73 <#lab-iv-command-line-tools>`__

`Questions to ask yourself 73 <#questions-to-ask-yourself-2>`__

`What’s Not Covered but we will discuss
73 <#whats-not-covered-but-we-will-discuss>`__

`Checking APM Logs 73 <#checking-apm-logs>`__

`Turning up the heat on Logging 83 <#turning-up-the-heat-on-logging>`__

`SessionDump Command 85 <#sessiondump-command>`__

`ADTest Tool 97 <#adtest-tool>`__

`iRules Logging Assistance 103 <#irules-logging-assistance>`__

`TCPDump Troubleshooting Assistance
106 <#tcpdump-troubleshooting-assistance>`__

`Learn More 107 <#learn-more>`__

Welcome
=======

Welcome to the UA 230 Troubleshooting Universal Access Lab. These lab
exercises will instruct you on how to configure and troubleshoot common
Access Policy Manager (APM) issues as experienced by field engineers,
support engineers and customers. This guide is intended to complement
lecture material provided during the UA 230 course as well as a
reference guide that can be referred to after the class as a basis for
troubleshooting APM in your own environment.

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

LAB I – APM Troubleshooting Lab Object Preparation
==================================================

The purpose of this lab is to preconfigure some objects that will be
used throughout the other labs. These objects are as follows:

-  Domain Name Services (DNS) Resolver

-  Network Time Protocol (NTP) Server

-  Access Policy (APM) AAA Server – Active Directory

-  Access Policy (APM) SSO Configuration – NTLMv1

-  Access Policy (APM) Access Profile

-  Local Traffic (LTM) Pool and Member

-  Local Traffic (LTM) Virtual Server

Connect to the Lab
------------------

|image1|

1. Establish an RDP connection to your Jump Host and double-click on the
   **BIG-IP** Chrome shortcut on the Windows desktop.

   -  User: agility

   -  Password: Agility1

2. Ignore the certificate warning.

3. Login into the BIG-IP Configuration Utility with the desktop icon (or
   Favorite link in Chrome) with the following credentials:

-  User: **admin**

-  Password: **admin**

DNS Resolver for System Configuration
-------------------------------------

|image2|

1. Create a DNS entry by selecting: System  Configuration  Device 
   DNS

|image3|

1. In the Properties Section for DNS Lookup Server List, enter
   **10.128.20.100** in the Address field and click the **ADD** button.

2. Scroll down to the DNS Search Domain List section and enter
   **agilitylab.com** in the Address field and click the **ADD** button.

3. Click the **UPDATE** button at the bottom of the page to save the
   changes you just made.

NTP Server for System Configuration
-----------------------------------

|image4|

1. Create a NTP entry by selecting: System  Configuration  Device 
   NTP

|image5|

1. In the Properties Section for Time Server List, enter
   **10.128.20.100** in the Address field and click the **ADD** button.

2. Click the **UPDATE** button at the bottom of the page to save the
   changes you just made.

Access Policy (APM) AAA Server – Active Directory Object Creation
-----------------------------------------------------------------

|image6|

1. Create a new AAA Server Object of type Active Directory by selecting:
   Access  Authentication  Active Directory

|image7|

1. Click the **CREATE** button on right side of page.

|image8|

1. Under General Properties type **LAB\_AD\_AAA** in the name field.

2. In the Configuration Section, Click the radio button option next to
   **Direct** in the Server Connection row.

3. In the Domain Name field enter **agilitylab.com**

4. Leave the Domain Controller, Admin Name and Admin Password fields
   blank for now.

5. Click the **FINISHED** button at the bottom of the page to save your
   changes.

Access Policy (APM) SSO Configuration – NTLMv1
----------------------------------------------

|image9|

1. Create a new SSO Configuration Object of type NTLM by selecting:
   Access  Single Sign-On  NTLMV1

|image10|

1. Click the **CREATE** button on the right side of the page.

|image11|

1. In the Name field enter **Agility\_Lab\_SSO\_NTLM**

2. Click the **FINISHED** button at the bottom.

Access Policy (APM) Access Profile Creation
-------------------------------------------

|image12|

1. Create a new APM Profile Object of type ALL by selecting: Access 
   Profiles/Policies  Access Profiles (Per-Session Policies)

|image13|

1. Click the **CREATE** button on the right side of the page.

|image14|

1. In the Name field enter, **Agility-Lab-Access-Profile**

2. In the Profile Type drop down list select **All**

3. **In the Profile Scope drop down list select Profile**

|image15|

1. In the Settings section click the checkbox to the right of Access
   Policy Timeout and change the value from 300, to **30**, seconds.

|image16|

1. Scroll the bottom of the page and in the Language Settings section,
   click to highlight **English** in the Factory Builtin Languages box,
   then click the left **<<** arrows to move it to the left box labeled
   Accepted Languages.

2. Click the **FINISHED** button at the bottom of the page to save your
   changes.

Local Traffic (LTM) Pool and Member Creation
--------------------------------------------

|image17|

1. Create a new LTM Pool and Member by selecting Local Traffic  Pools
   Pools List

|image18|

1. Click the **CREATE** button on the right side of the page.

|image19|

1. In the Name field enter **Agility-Lab-Pool**

2. In the Resources section, in the New Members area, enter
   **10.128.20.100** in the Address field.

3. In the Service Port field, enter **80**, or select **HTTP** from the
   drop-down menu.

4. Click the **ADD** button

5. Click the **FINISHED** button at the bottom to save your changes.

Local Traffic (LTM) Virtual Server Creation
-------------------------------------------

This lab will walk you through creating the Virtual Server we will use
during the course of the lab. This Virtual Server will be used to
associate Access Policies which will be evaluated when authenticating
users.

|image20|

1. Create an new Virtual Server by selecting Local Traffic  Virtual
   Servers  Virtual Server List

|image21|

1. Click the **CREATE** button on the right side of the page.

|image22|

1. Under the General Properties section, in the Name field enter
   **Agility-LTM-VIP**

2. In the Destination Address field enter **10.128.10.100**

3. In the Service Port fields enter **443**, or select **HTTPS** from
   the drop-down menu

|image23|

1. Under the Configuration section, in the HTTP Profile field use the
   drop-down menu to select **http**

2. In the SSL Profile (Client) field select **clientssl** from the
   Available profiles then use the **<<** left arrows to move it to the
   Selected box.

3. Ensure VLAN and Tunnel Traffic is set to **All VLANs and Tunnels**

4. In the Source Address Translation field select **Auto Map** from the
   drop-down menu.

|image24|

1. Scroll down to the Access Profile section, select
   **Agility-Lab-Access-Profile** from the drop-down menu.

|image25|

1. Click the **FINISHED** button to save your changes.

LAB II – General Troubleshooting
================================

In this lab exercise, you will learn where to look and what to look at
when an Access Policy is not successfully allowing access or not
performing as intended.

Questions to ask yourself
-------------------------

1. Do we have proper Network Connectivity?

2. Are there any Upstream/Downstream Firewall Rules preventing APM to be
   reachable or to reach destination targets it requires to access?

3. Do we have DNS setup properly?

4. Do we have NTP setup properly?

5. Are we receiving any Warnings or Error messages when we logon?

6. Are there any missing dependencies?

7. Time to check on our Sessions under Manage Session Menu

   a. What can we see from the Manage Session Menu?

   b. If we click the Session ID link what more information is
      available?

   c. Is Authentication Successful or is it Failing?

   d. Is the user receiving the proper ENDING ALLOW from the Policy?

8. Time to Review the Reports information for the Session in question

   a. What information is available from the ALL SESSIONS REPORT?

   b. Can we review the Session Variables for the user’s session from
      the ALL SESSION REPORT? If YES then Why however If NO then WHY?

9. Can the BIG-IP TMOS Resolve the AAA server by Hostname and by
   Hostname.Domain?

   a. Is the AAA reachable over the network, no Firewalls blocking
      communication from BIGIP Self-IP?

Verify DNS is setup from the CLI of the BIG-IP
----------------------------------------------

Perform the following steps to verify DNS is correctly configured:

|image26|

1. Click on the PuTTY (SSH client) to access the BIG-IP CLI

|image27|

1. Click on the **agilitylab** Saved Session and click Load

2. The click on **OPEN**

Alternatively, you can simply double-click on the **agilitylab** Saved
Session to open the session

|image28|

1. Logon as **root** with password **default** if necessary (you should
   logon automatically)

|image29|

1. From the CLI type **dig agilitylab.com** and then press enter

2. The following results should be reviewed and verified.

3. If DNS is properly configured you should receive the returned IP
   address of **10.128.20.100**

|image30|

1. From the CLI type **nslookup** and then press enter.

2. Type **agilitylab.com** and then press enter

3. The following results should be reviewed and verified.

4. If DNS is properly configured you should receive the returned IP
   address of **10.128.20.100**

5. Exit nslookup by typing **exit**

\ **Verify NTP is setup from the CLI of the BIGIP**

Perform the following steps to verify NTP is correctly configured:

|image31|

1. From the CLI (via PuTTy –SSH Client) …. type **ntpq –pn** and then
   press enter.

2. The following results should be reviewed.

|image32|

1. | If time is out of sync by too much of an offset you can update the
     local time using the following command:
   | **date MMDDhhmmYYYY**

Manage Sessions within the Access Policy Manager menu
-----------------------------------------------------

We use the Manage Sessions menu to view general status of currently
logged in sessions, view their progress through a policy, and to kill
sessions when needed.

STEP 1

|image33|

1. Open a USER session to APM through a new browser window by navigating
   to your first Virtual Server IP Address created in LAB I
   (**10.128.10.100**)

|image34|

1. Did you receive an error message? If so, take note of the Session
   Reference Number

TEST 1

|image35|

1. In the browser window, you are using to manage the BIG-IP, navigate
   to Access  Active Sessions menu.

2. Review the Manage Sessions screen, is there an Active Session? If not
   then why?

STEP 2

|image36|

1. Now open the APM Visual Policy Editor (VPE) for the policy
   created/loaded in LAB I by navigating to Access  Profiles/Policies
   -> Access Profiles (Per-Session Policies) menu.

|image37|

1. Then click the Edit link in the row that has the name of your Access
   Profile you are working with currently.
   (**Agility-Lab-Access-Profile**)

|image38|

1. This will either launch a new browser or new tab depending on your
   browsers settings to display the APM Visual Policy Editor (VPE). The
   first policy we created was never edited to add any additional tasks
   that would instruct APM on what Actions it would need to take/enforce
   throughout a Policy Execution for the user’s Session. So we will now
   adjust the policy and retest to see if we receive some new results.

|image39|

1. Click on the **+** symbol between the Start and ending Deny objects.

|image40|

1. This will pop up the Actions window where we can select from several
   Actions we wish to associate with our policy. On the Logon tab select
   the **Logon Page** radio button and then click the **ADD ITEM**
   button at the bottom of the page.

|image41|

1. Click the **SAVE** button on the Logon Page properties window.

|image42|

1. Then click the **Apply Access Policy** link on the top left of the
   page.

TEST 2

|image43|

1. Restart your session to APM. (**https://10.128.10.100**)

|image44|

1. Did you receive and error this time? Or did you receive a Logon Page?

|image45|

1. Open your browser or tab for managing APM and open the Active
   Sessions menu again.

2. Is there now an Active Session displayed on the page? If you were
   already on this page you may need to click the Refresh Session Table
   button.

3. What does the Status Icon look like? Is it a Green Circle or a Blue
   Square?

4. Is your username displayed in the Logon column?

5. Click on the Session ID for your session, this will open up a Session
   Details window.

|image46|

1. In the Session Details window, we can see some information about the
   session up to the point that the policy has executed so far.

|image47|

1. Further down there is a reports section titled **Built-In Reports**,
   click that to open the list of built in reports.

|image48|

1. Scroll down to see the list of **Session Reports** and click the
   **Current Sessions** line and select **Run Report** from the pop up
   window.

|image49|

1. Do you see your Session ID displayed in the list of current sessions?
   If not then why?

TEST 3

|image50|

1. Return to the browser or tab you are using for access to
   **https://10.128.10.100**. Restart a new session if necessary.

2. Next logon to the APM Logon page with:

   -  Username: **student**

   -  Password: **password**

|image51|

1. Did you receive and error after logging on? If so note the Session
   Reference Number.

|image52|

1. Review the Manage Sessions menu, is your session listed?

|image53|

1. Navigate to Access -> Overview  Access Reports. When prompted Click
   Run Report.

|image54|

1. Do you see your Session ID listed in the list of All Sessions? Is the
   username listed in the Logon column?

|image55|

1. Click the Session ID to open the Session Details window.

2. Do you now see more information in this Sessions Details compared to
   the previous one we reviewed?

3. Is the username listed in the details?

4. In the Session Details screen we can see some important
   troubleshooting information, for example just below the username row
   we see a line that states that the Policy followed a path or branch
   called Fallback out of the Logon Page object to an Ending “Deny” thus
   the Access Policy Result was Logon\_Deny.

|image56|

1. Now click back on the All Sessions tab at the top.

2. In the row for this session look to the right of the Logon column.
   You will see the next column states that the session is not Active.
   Now click the View Session Variables link in the next column.

|image57|

1. Do you see a lot of information recorded for Session Variables for
   this session? If not, then why?

LAB III Visual Policy Editor (VPE) & Session Variables
======================================================

This lab will go a little deeper into understanding the Visual Policy
Editor and Session Variables.

Questions to ask yourself
-------------------------

-  Does the VPE Flow look correct?

-  Does the VPE have the proper ENDING assigned to the appropriate
   BRANCH?

-  Are your connection attempts following the intended VPE BRANCH/PATH
   during your test?

   -  How could you alter the VPE to allow for better trouble shooting
      or pausing of a policy execution and termination?

-  How can I pause the Policy Execution or Termination to review the
   session variable in Reports?

-  What are VPE Actions?

-  Are the Correct Session Variables being sent to the AAA Object?

-  How can we GET or SET Session Variables in the VPE?

-  How could I preserve the originally requested URI from the Client to
   pass to the internal server after APM authentication has complete?

Visual Policy Editor (VPE) Workflow, Actions, Branches, Endings
---------------------------------------------------------------

The Visual Policy Editor (VPE) is a screen on which to configure an
access policy using visual elements. We have used it a few times already
throughout our previous labs. This is meant to both review and explain
in a bit more detail what the available Visual Policy Editor conventions
are.

This table provides a visual dictionary for the Visual Policy Editor
(VPE).

Visual Policy Editor (VPE) Visual Dictionary

+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Element type                               | Description                                                                                                                                                                                                                                                           | Visual element   |
+============================================+=======================================================================================================================================================================================================================================================================+==================+
| Initial Access Policy                      | When an access profile is created, usually an initial access policy is also created.                                                                                                                                                                                  | |image58|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Start                                      | Every access profile contains a start.                                                                                                                                                                                                                                | |image59|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Branch                                     | A branch connects an action to another action or to an ending.                                                                                                                                                                                                        | |image60|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Add an action                              | Clicking this icon causes a screen to open with available actions for selection.                                                                                                                                                                                      | |image61|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Action                                     | Clicking the name of an action, such as Logon Page, opens a screen with properties and rules for the action. Clicking the x deletes the action from the access policy.                                                                                                | |image62|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Action that requires some configuration    | The red asterisk indicates that some properties must be configured. Clicking the name opens a screen with properties for the action.                                                                                                                                  | |image63|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Ending                                     | Each branch has an ending: Allow or Deny.                                                                                                                                                                                                                             | |image64|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Configure ending                           | Clicking the name of an ending opens a popup screen.                                                                                                                                                                                                                  | |image65|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Add a macro for use in the access policy   | Opens a screen for macro template selection. After addition, the macro is available for configuration and for use as an action item.                                                                                                                                  | |image66|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Macro added for use                        | Added macros display under the access policy. Clicking the plus (+) sign expands the macro for configuration of the actions in it.                                                                                                                                    | |image67|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Macrocall in an Access Policy              | Clicking the *Macrocall* name expands the *Macro* in the area below the *Access Policy.*                                                                                                                                                                              | |image68|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Apply Access Policy                        | Clicking it commits changes. The visual policy editor displays this link when any changes remain uncommitted.                                                                                                                                                         | |image69|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+
| Add Items Actions                          | The actions that are available on any given tab of the *Add Item* screen depend on the access profile type, such as LTM-APM (for web access) or SSL-VPN (for remote access), and so on. Only actions that are appropriate for the access profile type will display.   | |image70|        |
+--------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------+

Pausing the APM Policy Execution for Troubleshooting – The Message Box
----------------------------------------------------------------------

Now that we have reviewed/refreshed our memory on VPE conventions lets
edit our policy we were previously working on to add some more actions.
This section we show a great tool for troubleshooting a policy that may
have been reaching an ENDING DENY and closing the APM session too
rapidly for proper inspection during the troubleshooting phase.

STEP 1

|image71|

1. Navigate to Access  Access Profiles  Profiles/Policies -> Access
   Profiles (Per-Sessions Policies). Click Edit next to
   **Agility-Lab-Access-Profile** to open the Visual Policy Editor
   (VPE).

|image72|

1. After the Logon Page object, on the fallback branch, click the **+**
   symbol to open the Actions window.

|image73|

1. Click on the **General** **Purpose** tab and then click the radio
   button next to **Message Box** and click the **ADD ITEM** button at
   the bottom of the page.

|image74|

1. Click the **SAVE** button on the next window

|image75|

1. Now client the ending Deny.

|image76|

1. In the pop-up window change it to Allow and click the **SAVE**
   button.

|image77|

1. Then click the Apply Access Policy link at the top left.

TEST 1

|image78|

1. Return to the browser or tab you are using for access to
   **https://10.128.10.100**. Restart a new session if necessary.

-  Username: **student**

-  Password: **password**

|image79|

1. Did we receive an error this time after the logon page?

2. Did the Message Box display?

|image80|

1. Keep the message box display there and move to the other browser to
   review the Manage Sessions menu.

2. Does the Manage Sessions menu show the Username this time?

3. Is the Status showing a Blue Square or Green Circle? Why?

|image81|

1. Click the session ID to review the details for any new messages.

2. If things worked correctly you should see a message in the details
   stating, “Session deleted due to user inactivity or errors”

|image82|

1. If you look back at the other browser window you should notice a
   Session Expired/Timeout message is being displayed.

STEP 2

|image83|

1. Navigate back to Access  Profiles/Policies  Access Profiles
   (Per-Session Policies). Click on **Agility-Lab-Access-Profile**

|image84|

1. Access Policy Timeout from 30 seconds back to **300** seconds by
   removing the check from the custom column.

2. Click the **UPDATE** button at the bottom of the page.

|image85|

1. Click Apply Access Policy link at the top left of the page.

|image86|

1. Finalize the update by confirming the box is checked next to the
   profile and clicking **APPLY ACESS POLICY**

TEST 2

|image87|

1. Now go back and restart the user session and logon.

|image88|

1. **Do NOT** click the message box link “Click here to continue”

2. Leave the message box message displayed for the time.

|image89|

1. Go to the other browser/tab and open the Manage Sessions menu.

2. Your session should be there but the Status icon should still be a
   Blue Square.

3. Click on your Session ID

|image90|

1. Click Built-in Reports

|image91|

1. Click on All Sessions report, then choose Run Report on the pop-up
   menu.

|image92|

1. Click the Session Variables for your current session.

|image93|

1. Do you now have Session Variables being displayed for this session?
   If so why?

|image94|

1. Click the All Sessions tab and look at the column labeled Active.
   Does it show a Y or N in the column?

Note that session variables will only be displayed for Active sessions.
Since you placed a message box in the VPE to pause policy execution the
session is seen as active. This provides you the ability to now review
Session Variables that APM has collected up to this point in the
policies execution.

|image95|

1. Now in the user browser click the link in the Message Box.

If it timed out then restart and this time click through the message box
link.

|image96|

1. Now review the Active Sessions menu and note what icon is shown in
   the status column. Green Circle finally? Success!!

|image97|

1. If you now click the Session ID you will see that the Policy has
   reached an ending Allow thus the Access Policy Result is now showing
   we have been granted LTM+APM\_Mode access.

|image98|

1. Now open the All Sessions report once more to review the Session
   Variables collected.

|image99|

1. Click the logon folder in the Session Variables page that opens for
   your session.

|image100|

1. Click the folder icon named *last* to expand its contents.

Notice on the left column labeled Variable Name above and to the right
the next column is Variable Value and the third column is Variable ID.
If you look at the Variable Name of username you will see to the right
its value is recorded as student as you entered it in the logon page.
The next column displays APM’s matching session Variable ID for this
information. You will see that the naming convention follows the session
hierarchy starting with session. then the first folder logon. then the
next folder last. then finally the Variable Name of Username.

We will use some session variables in the next lab to GET and SET
information for the users session.

Session Variables – Setting and Retrieving (Some Quick Information)
-------------------------------------------------------------------

This section will provide some guidance on how to both retrieve and set
session variables within a policy for a user’s session. Session
Variables are very useful in many areas of policy execution. They can be
used to assist in areas like authentication or single sign-on or
assigning resource items for users based on information APM can collect
from the backend AAA server and its associated directory.

Currently cached session Variables are available in APM Reports for
review by an administrator. Additional available variables can always be
found in the APM Configuration Guides. What is really nice is that APM
is not limited to only having awareness of Session Variable it collects
from the user session establishment or from the AAA server,
administrators can actually create or set their own custom session
variables for use within a policy. This means that an administrator
could create new session variables via the VPE’s Variable Assign action
or session variables could even be set from an iRule attached to a
virtual server. This means that information that the LTM VIP can see or
be gathered via an iRule could then be set as a session variable that
could then be retrieved and used within the VPE.

About Session Variable Names
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The name of a session variable consists of multiple hierarchical nodes
that are separated by periods (.):

|image101|

Session Variable Reference
~~~~~~~~~~~~~~~~~~~~~~~~~~

APM Session Variable references are provided in APM documentation.
Current release information can be found at the following link:
https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-visual-policy-editor-13-0-0/5.html

Partial Session Variable list

|image102|

Session Variable Categorization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While these are not formal categories, Session Variables fall under
three general categories:

+----------------------------------------------------+---------------------------------------------+
| Category                                           | Examples                                    |
+====================================================+=============================================+
| Variables returned by Access Policy actions        | • Active Directory query results            |
|                                                    |                                             |
|                                                    | • Antivirus Check results                   |
|                                                    |                                             |
|                                                    | • Windows Info and Registry check results   |
+----------------------------------------------------+---------------------------------------------+
| Special purpose user variables                     | • Lease Pools                               |
|                                                    |                                             |
|                                                    | • Client IP assigned to a client session    |
|                                                    |                                             |
|                                                    | • Username and Password                     |
+----------------------------------------------------+---------------------------------------------+
| Network access resource variables and attributes   | • Split tunneling                           |
|                                                    |                                             |
|                                                    | • DNS Settings                              |
|                                                    |                                             |
|                                                    | • Compression, etc.                         |
+----------------------------------------------------+---------------------------------------------+

Active Session Variables
~~~~~~~~~~~~~~~~~~~~~~~~

Below is a short breakdown of information gathered and cached during an
Active session. Additional information can be gathered from the results
of End Point checks when they are put into a policy. These would display
as folders like check\_av or check\_fw if the actions were added to the
policy

|image103|

Session Variable Manipulation via TCL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Variables can be parsed, modified, manipulated, etc using TCL. Although
the tables below are not an exhaustive reference for writing and using
TCL expressions, it includes some common operators and syntax rules.

Standard Operators

You can use TCL standard operators with most BIG-IP® Access Policy
Manager® rules. You can find a full list of these operators in the TCL
online manual, at http://www.tcl.tk/man/tcl8.5/TclCmd/expr.htm. Standard
operators include:

+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Operator        | Description                                                                                                                                                                                                                                                                                                               |
+=================+===========================================================================================================================================================================================================================================================================================================================+
| **- + ~ !**     | Unary minus, unary plus, bit-wise NOT, logical NOT. None of these operators may be applied to string operands, and bit-wise NOT may be applied only to integers.                                                                                                                                                          |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **\*\***        | Exponentiation. Valid for any numeric operands.                                                                                                                                                                                                                                                                           |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **\* / %**      | Multiply, divide, remainder. None of these operators may be applied to string operands, and remainder may be applied only to integers. The remainder will always have the same sign as the divisor and an absolute value smaller than the divisor.                                                                        |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **+ -**         | Add and subtract. Valid for any numeric operands.                                                                                                                                                                                                                                                                         |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **<< >>**       | Left and right shift. Valid for integer operands only. A right shift always propagates the sign bit.                                                                                                                                                                                                                      |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **< > <= >=**   |                                                                                                                                                                                                                                                                                                                           |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|                 | Boolean less than, greater than, less than or equal to, and greater than or equal to. Each operator produces 1 if the condition is true, 0 otherwise. These operators may be applied to strings as well as numeric operands, in which case string comparison is used.                                                     |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **== !=**       | Boolean equal to and not equal to. Each operator produces a zero/one result. Valid for all operand types.                                                                                                                                                                                                                 |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **eq ne**       | Boolean string equal to and string not equal to. Each operator produces a zero/one result. The operand types are interpreted only as strings.                                                                                                                                                                             |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **in ni**       | List containment and negated list containment. Each operator produces a zero/one result and treats its first argument as a string and its second argument as a Tcl list. The in operator indicates whether the first argument is a member of the second argument list; the ni operator inverts the sense of the result.   |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **&**           | Bit-wise AND. Valid for integer operands only.                                                                                                                                                                                                                                                                            |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **^**           | Bit-wise exclusive OR. Valid for integer operands only.                                                                                                                                                                                                                                                                   |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **\|**          | Bit-wise OR. Valid for integer operands only.                                                                                                                                                                                                                                                                             |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **&&**          | Logical AND. Produces a 1 result if both operands are non-zero, 0 otherwise. Valid for boolean and numeric (integers or floating-point) operands only.                                                                                                                                                                    |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **\|\|**        | Logical OR. Produces a 0 result if both operands are zero, 1 otherwise. Valid for boolean and numeric (integers or floating-point) operands only.                                                                                                                                                                         |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **x?y:z**       | If-then-else, as in C. If x evaluates to non-zero, then the result is the value of y. Otherwise the result is the value of z. The x operand must have a boolean or numeric value.                                                                                                                                         |
+-----------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Standard Operators

A rule operator compares two operands in an expression. In addition to
using the TCL standard operators, you can use the operators listed
below.

+----------------------+----------------------------------------------------------------+
| Operator             | Description                                                    |
+======================+================================================================+
| **contains**         | Tests if one string contains another string.                   |
+----------------------+----------------------------------------------------------------+
| **ends\_with**       | Tests if one string ends with another string                   |
+----------------------+----------------------------------------------------------------+
| **equals**           | Tests if one string equals another string                      |
+----------------------+----------------------------------------------------------------+
| **matches**          | Tests if one string matches another string                     |
+----------------------+----------------------------------------------------------------+
| **matches\_regex**   | Tests if one string matches a regular expression               |
+----------------------+----------------------------------------------------------------+
| **starts\_with**     | Tests if one string starts\_with another string                |
+----------------------+----------------------------------------------------------------+
| **switch**           | Evaluates one of several scripts, depending on a given value   |
+----------------------+----------------------------------------------------------------+

Logical Operators

Logical operators are used to compare two values.

+------------+--------------------------------------------------------+
| Operator   | Description                                            |
+============+========================================================+
| **and**    | Performs a logical and comparison between two values   |
+------------+--------------------------------------------------------+
| **not**    | Performs a logical not action on a value               |
+------------+--------------------------------------------------------+
| **or**     | Performs a logical or comparison between two values    |
+------------+--------------------------------------------------------+

Getting/Setting Session Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

During the pre-logon sequence, using the Visual Policy Editor (VPE) you
can get and set Session Variables. The following are some quick
examples.

-  To **set/modify** a variable: Variable Assign action

-  | To **get** a value the last username entered by a user, use expr or
     return:
   | expr { [mcget {session.logon.last.username}]}

**expr** evaluates an expression, whereas **return** simply returns the
result. For example, we have a two custom variables:

-  session.custom.value1 = 3

-  session.custom.value2 = 4

Using **expr** we can construct the following expression, this would
return a value of 7 (i.e. the evaluation of 3+4):

    expr { “[mcget session.custom.value1] + [mcget
    session.custom.value2]” }.

Using **return** we can construct the following expression, this would
return simply “3+4” as shown.

    return { “[mcget session.custom.value1] + [mcget
    session.custom.value2]” }

Using iRules
~~~~~~~~~~~~

In all the “Access” events

ACCESS::session data get/set “variable\_name” [“value”]

Set Secure Variables
~~~~~~~~~~~~~~~~~~~~

You can also set Secure Variables. The value of a secure session
variable is stored as encrypted in the session db. The value is not
displayed as part of session report in UI, nor is it logged as part of
logging agent. Secure variables require the -secure flag, both for mcget
and access::session data get/set.

|image104|

Review these two examples below. The first is a Variable Assign action
that is SETTING the Session Variable ID of “session.logon.last.upn” with
the information extracted from an x509 Client Certificate that was
presented by the user’s computer/browser upon connection to the VIP.

|image105|

The second example show a message box displaying a Session Variable
value by calling out the Session Variable ID in the Message Box for the
user to see.

|image106|

Session Variable Exercise
~~~~~~~~~~~~~~~~~~~~~~~~~

The following are some exercises to demonstrate how session variables
can be utilized.

STEP 1

|image107|

1. Open the APM VPE for the **Agility-Lab-Access-Profile** Access Policy
   we have been working with.

|image108|

1. Edit the Message Box in the VPE.

|image109|

1. In the Message text box enter: **My username is:
   %{session.logon.last.username}** Then click the **Save** button

|image110|

1. Then click Apply Access Policy

TEST 1

|image111|

1. Now logon with the “student” username to the test site.

|image112|

1. When the message box appears, you should see a message stating,
   “\ **My username is: student**\ ”. Was it successful?

STEP 2

|image113|

1. Go back into the VPE

|image114|

|image115|

1. Add a Variable Assign action from the Assignment action tab and place
   it before the Message Box action.

|image116|

1. When the properties screen opens, click the **Add New Entry** button.

|image117|

1. Then click the “Change” link.

|image118|

1. A window will pop up with *Custom Variable* on the left and *Custom
   Expression* on the right.

You will notice both boxes are currently empty.

|image119|

1. Often you may forget how to start off with the variable name or the
   expression so a trick you can use to get you started is first select
   a pre-defined variable on the left side and a AAA attribute on the
   right side and then reselect custom variable and custom expression.
   This will populate each box with example data that you can now edit.

***This is not a required step, just a tip!***

|image120|

1. On the Custom Variable side type: **session.custom.mynewvar** (Be
   sure to make it lowercase). On the Custom Expression side type:
   **mcget {session.user.clientip}** (There is a space between mcget and
   the { bracket)

2. Click the **Finished** button.

|image121|

1. Click the **Save** button.

|image122|

1. Click on the Message Box.

|image123|

1. After the closing **}** bracket in the first line of the message
   section add a space and then type **<br>**

2. Then on the next line type, **My Client IP is:
   %{session.custom.mynewvar} **

3. Then click the **Save** button.

|image124|

1. Then click Apply Access Policy.

TEST 2

|image125|

1. Now logon to the test site as a user again and review the message box
   text.

|image126|

1. Does it display your client IP address?

|image127|

1. Now run the All Sessions Report and review the View Session Variables
   for the active SessionID. (Access Overview Access Reports)

2. Notice the folder icon named custom and the corresponding Variable ID
   of session.custom. This was generated automatically during the
   Variable Assign action that you added to the policy. When you set the
   Custom Variable to session.custom.mynewvar APM used the next word
   after the session as the new container (custom) for variable
   (mynewvar).

|image128|

1. If you expand custom folder you will notice a new Variable named
   mynewvar and in the next column you will see your client ip address
   and in the third column the variable id of session.custom.mynewvar

As you can see this could be expanded upon to be very useful. For
example, maybe you are enabling two-factor authentication for both
Active Directory and RSA Secure ID. Well the AAA server authentication
Action objects expect to see a specific session variable name sent to
them for so that they can correctly parse that data and verify against
the AAA server. As an example both the AD Auth and the RSA Auth expect
to see session.logon.last.password as the variable used to hold the
password value. However, if you create a logon page with three input
fields, one for username, a second for AD password and the third for the
RSA Token/PIN then they must each have their own unique post and session
variable name as they are configured in the Logon Page object.

This means that as the third variable for the RSA toke/pin is passed to
APM no longer as session.logon.last.password because the AD Password
field was already set to use that variable on the logon page. What do we
do now?

Variable Assign to the rescue, take a look at this below example to fix
this problem as it mimics what we just accomplished with the
session.custom.mynewvar exercise. Consider the following screen shots.

|image129|

LAB IV – Command Line Tools
===========================

This lab will show you how to make use of some of the Command Line
Utilities for troubleshooting Access Policy Manager when dealing with
Authentication issues that you could experience.

Questions to ask yourself
-------------------------

-  What should I expect in the Logs with Default Settings?

-  Can I review the APM configuration from TMSH?

-  Can I review Session Data from the CLI?

-  How can I test if the AAA server responds to Authentication Tests
   using CLI Tools?

-  How can I test if the AAA server respond to Query Tests using CLI
   Tools?

-  How can I change the Logging Level for more Verbose details?

-  How can I use iRules for Troubleshooting Assistance?

-  How can I use TCPDump for Troubleshooting Assistance?

What’s Not Covered but we will discuss
--------------------------------------

-  VDI Troubleshooting/Debug Logging

-  SAML Troubleshooting Tools – SAML Tracer (Not CLI based)

Checking APM Logs
-----------------

APM Logs by default show the same information you can get from the
Manage Sessions menu, as well as APM module-specific information.

Access Policy Manager uses syslog-ng to log events. The syslog-ng
utility is an enhanced version of the standard logging utility syslog.

The type of event messages available on the APM are:

+------------------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Event Messages         | File Location    | Description                                                                                                                                                                                                 |
+========================+==================+=============================================================================================================================================================================================================+
| Access Policy Events   | /var/log/apm     | Access Policy event messages include logs pertinent to access policy, SSO, network access, and web applications. To view access policy events, on the navigation pane, expand System menu and click Logs.   |
+------------------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Audit Logging          | /var/log/audit   | Audit event messages are those that the APM system logs as a result of changes made to its configuration.                                                                                                   |
+------------------------+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

When setting up logging you can customize the logs by designating the
minimum severity level or log level, that you want the system to report
when a type of event occurs. The minimum log level indicates the minimum
severity level at which the system logs that type of event.

***Note: Files are rotated daily if their file size exceeds 10MB.
Additionally, weekly rotations are enforced if the rotated log file is a
week old, regardless whether or not the file exceeds the 10MB
threshold.***

The **default** log level for the BIG-IP APM access policy log is
**Notice**, which does ***not*** log Session Variables. Setting the
access policy log level to **Informational** or **Debug** will cause the
BIG-IP APM system to log Session Variables, but it will also add
additional system overhead. If you need to log Session Variables on a
production system, F5 recommends setting the access policy log level to
Informational temporarily while performing troubleshooting or debugging.

We need to add some more actions to the APM Profile in the VPE we have
been working with to go along with the next few lab tests.

STEP 1

|image130|

1. Open the VPE and add a new AD Query action after the first Message
   Box action by selecting the **+** sign that follows.

|image131|

1. Navigate to the Authentication tab and select the AD Query radial and
   click **Add Item**.

|image132|

1. In the AD Query, use the drop-down dialog box on Server to select the
   **/Common/LAB\_AD\_AAA** server. Click the **Save** button.

|image133|

1. On the top branch following the AD Query action, add another Message
   Box.

Hint: A Message Box can be added by clicking the **+** sign, navigating
to the General Purpose tab and selecting Message Box

|image134|

1. After the second Message Box add the AD Auth action from the
   Authentication tab

Hint: An AD Auth action can be added by clicking the **+** sign,
navigating to the Authentication tab and selecting AD Auth

|image135|

1. In the AD Auth properties window use the server drop-down menu to
   select **/Common/LAB\_AD\_AAA** server.

2. Click the **Save** button.

|image136|

1. Your policy should now look like this

Notice that one the top branch to the AD Query object the line reads
User Primary Group ID is 100 (See graphic in Step 8 above, just after AD
Query). Maybe you do not want to query for that information and would
prefer to delete that branch. You must be ***careful*** in what you
select or do when deleting that branch when you have other actions
following it in the policy or they could be deleted when you do not want
them to be deleted. Here is a trick you can use to preserve the actions
that follow the ad query when you need to delete a branch.

STEP 1 Continued

|image137|

1. Just before the second Message Box after the “User Primary Group ID
   is 100” and after the **+** symbol there is a double arrow symbol.
   This will allow us to swap portions of the policy that come after
   that **->>-** double arrow to another location in the VPE policy.

|image138|

1. Click the **->>-** double arrow.

|image139|

1. You will now notice a **vertical arrow** pointing to other locations
   in the VPE where this section highlighted in green can be swapped.

2. Click on the **Vertical Arrow**

|image140|

1. Now click the **AD Query** action in your policy and go to **Branch
   Rules** tab

2. Click the **X** to the right in the gray box for the Branch Rule

3. Click **Save** to save your settings

|image141|

1. Your policy should now look like this. Now you can see how the Swap
   function can help with moving action objects throughout the VPE

|image142|

1. Click **Apply Access Policy** to save and implement or work

Now let’s see what can be seen in the logs when set at the default
logging level of Notice.

TEST 1

|image143|\ |image144|\ |image145|

1. Review the current Access Policy Logging (Access  Overview  Event
   Logs -> Settings)

2. Select **default-log-setting**, then Click Edit to view settings.

3. Select **Access System Logs**

|image146|

1. Logon to the BIGIP APM console using an SSH client (PuTTY from your
   desktop). Select **agilitylab**  **Load**  **Open**

|image147|

1. Maximize your SSH window to reduce line wrapping when reviewing the
   logs from the CLI.

2. From the CLI prompt, type **tail –f /var/log/apm** and hit **Enter**
   so you can start see the logs being displayed

|image148|

With the SSH console logging, open a browser and access the APM as the
user **student**.

|image149|

1. Notice the logs being produced at the different stages of the users
   session as it first reaches the VIP, then when the user
   authenticates, receives message boxes or other policy actions, and
   then when the user reaches the policy result.

With the ***default logging*** level, there are no session variables
being logged.

In the Next test we will turn up logging to Informational and restart
the user session and then in the last test change logging level to Debug
and notice the differences from Informational and Notice logging levels.

Turning up the heat on Logging
------------------------------

Now let’s test more verbose logging. You can step up from Notice to
Informational and then to Debug if you want to see the differences
yourself. For the purpose of this test though I will jump straight to
Debug. You can use the GUI to make the log level changes to Debug or you
could use the Traffic Management Shell (TMSH) command from the CLI to
adjust the logging.

STEP 1

|image150|

1. Change Access Policy log setting to Debug (Access -> Overview  Event
   Logs  Settings, select default-log-setting, then click Edit)

TIP: Make sure you change setting back to Notice when not
troubleshooting. High levels of logging not only consume more disk
space, but also consume other resources, such as CPU, when enabled.

TEST 2

|image151|

1. Once you have the logging level increased restart you user session
   with the browser to the APM VIP and walk through the policy message
   boxes and other actions taking note of the additional verbosity in
   the logs you see in the SSH terminal window.

For sake of saving space in this document we will not include the screen
shots showing the Informational and Debug logging messages and allow you
to experience that yourself during your tests.

SessionDump Command
-------------------

SessionDump is a command line utility that shows sessions and their
associated session variables (like GUI Reports)

The sessiondump command has sever switches that can be used and you can
further enhance your troubleshooting by additionally using other CLI
utilities like grep to help filter the results to certain information.
As you can see from the examples below, the first command simple
provides all keys to be dumped for any/all user sessions while the
second using grep allows you to filter the output to those associated
with a given username. Refer to the screen shots below if you need
additional detail.

|image152|

This first example uses just the –allkeys switch.

**sessiondump –allkeys**

|image153|

This second example also uses the –allkeys switch. However, it also adds
the \|grep command to search for the “username”

**sessiondump -allkeys \| grep ‘student’**

STEP 1

|image154|

1. On the command line, if you still had the tail command showing
   logging then stop that now by typing **CTRL-C**

|image155|

Remember back in previous labs we learned that Session Variables cannot
be displayed in the Reports screens if the User Session is not in an
***Active*** state. Well that is the same with the CLI sessiondump
utility. There must be active sessions through APM in order to dump
details.

1. Once you are at the command prompt again try using the **sessiondump
   –allkeys** command first. Did you receive any data after running the
   command? If not, then why?

|image156|

1. If all your previous sessions have expired then startup and new
   session as a user and logon to APM and click through the message
   boxes.

|image157|

1. Now on the console type: **sessiondump –allkeys.** You should see a
   long list of information.

|image158|

Compare that with running: sessiondump –allkeys \| grep student You
should then only see the lines that had the username you specified in
the command to be returned

Now let us have some fun with using this utility to help with SSO
troubleshooting/validation.

STEP 2

|image159|

1. Edit the VPE for the **Agility-Lab-Access-Profile** policy we have
   been working with.

|image160|

1. Add two new actions to the policy after the AD Auth on the successful
   branch.

|image161|

1. First after AD Auth add the SSO Credential Mapping action from the
   Assignment Tab. Click **Add Item**

|image162|

1. Keep the default settings and click **Save**.

|image163|

1. Next add after the SSO Credential Mapping action add a Pool Assign
   action from the Assignment tab.

|image164|

1. In the next window click the **Add\\Delete** link.

|image165|

1. Then select the radio button for **/Common/Agility-Lab-Pool**. Now
   click the **Save** button.

|image166|

1. Then click Apply Access Policy link on top left of page.

TEST 2

|image167|

1. Restart a new APM user session. Logon and follow through all the
   policy actions

|image168|

1. This time instead of seeing a browser error you should be getting
   prompted for authentication for a website which is the site being
   hosted on the pool member that we assigned to the policy. Why are we
   getting prompted for authentication though? Did we not add the SSO
   Credential Mapping to the policy as well?

|image169|

1. Let’s use the following command at the console to check if we are
   getting credentials mapped to token variables properly: **sessiondump
   –allkeys \| grep ‘sso**\ ’ You should see two lines that show
   something like this following picture.

If you see the two lines with session.sso.token.last, then we know the
credential mapping is happening and the username should be displayed
accordingly. So what’s missing?

STEPS

|image170|

1. Next go to the Access Policy menu, click on Access ->
   Profiles/Policies -> Access Profiles (Per-Session Policies) .

|image171|

1. In the list of access profiles, click the NAME of your access
   profile, **Agility-LAB-Access-Profile**

|image172|

1. When this page opens, look at the top, there are four tabs, click the
   **SSO / Auth Domains** tab

|image173|

1. On this page, use the drop down menu on the SSO Configuration row to
   select **Agility\_Lab\_SSO\_NTLM**. Then click Update

|image174|

1. Then click **Apply Access Policy** on the top left of the page and
   apply the policy on the next page.

TEST 3

|image175|

1. Restart your user session again to the VIP and logon and click
   through the actions.

If necessary, you can kill your existing session by navigating to Access
Policy  Manage Sessions, then select the user/session and Click Kill
Selected Sessions

|image176|

1. Now what do you see when the policy has completed? Are you seeing the
   web application without being prompted for an additional logon prompt
   from the application? If so, then you were successful.

ADTest Tool
-----------

In this section we will get familiar with anther CLI utility to assist
in verifying proper authentication and query capabilities to an Active
Directory domain. We need to prepare for this lab by making a quick
change to the BIGIP’s configuration.

STEP 1

|image177|

1. Navigate to System  Configuration  Device  DNS

2. Highlight **10.128.10.100** in the DNS Lookup Server List and click
   **Delete**.

3. Also highlight and **Delete** the DNS Search Domain List of
   **agilitylab.com**

4. Click the **Update** button.

The **/usr/local/bin/adtest** utility is a test tool for APM's Active
Directory Module

+---------------------------------------------------------------------+--------------+
| tYPICAL USAGE                                                       |              |
+=====================================================================+==============+
| Auth Test with Administrative username & password (not necessary)   | |image178|   |
+---------------------------------------------------------------------+--------------+
| Auth Test without just username and password                        | |image179|   |
+---------------------------------------------------------------------+--------------+
| Query Test With Administrative username and password                | |image180|   |
+---------------------------------------------------------------------+--------------+

The ADTest tool can help point out potential issues with a BIG-IP’s
configuration or interoperability issues on the server’s side.

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
| cOMMON ERRORS                                                                                                                                                                                                                                                  |                                                                                          |
+================================================================================================================================================================================================================================================================+==========================================================================================+
| ERROR: query with '(sAMAccountName=student)' failed in krb5\_get\_init\_creds\_password(): Preauthentication failed, principal name: administrator@agilitylab.com (-1765328360)                                                                                | The cause of this is simply failed administrative credentials while attempting a query   |
|                                                                                                                                                                                                                                                                |                                                                                          |
| **Test done: total tests: 1, success=0, failure=1**                                                                                                                                                                                                            |                                                                                          |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
| ERROR: query with '(sAMAccountName=student)' failed in ldap\_sasl\_interactive\_bind\_s(): Local error, SASL(-1): generic failure: GSSAPI Error: Unspecified GSS failure. Minor code may provide more information (Cannot find KDC for requested realm) (-2)   | The cause of this is typically failed DNS resolution                                     |
|                                                                                                                                                                                                                                                                |                                                                                          |
| **Test done: total tests: 1, success=0, failure=1**                                                                                                                                                                                                            |                                                                                          |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+

Refer to the screen shots below if you need additional information
regarding the options of ADTest.

|image181|

Test 1

|image182|

1. Try logging on to the VIP as a user again after removing the DNS
   entries. You will notice that your logon will likely fail and you
   will receive the following screen.

|image183|

1. Review the session details for this logon session in reports or
   manage sessions. As we can see from the session details the AD Query
   is failing as well as AD Auth

|image184|

1. Now we can test from the console. Open a console/ssh session. Using
   the following command let us first test authentication using the
   ADtest utility. **adtest -t auth -r "agilitylab.com" -u student -w
   password**. What result did you get with that test?

|image185|

1. Now let’s try a query test. **adtest -t query -r "agilitylab.com" -A
   Administrator -W adminpass -u student -w password**. What result was
   returned?

|image186|

1. Go back to the DNS Settings section and re-add the DNS server IP and
   domain. Then re-test the Auth and Query using the ADtest utility.

iRules Logging Assistance
-------------------------

As many know one of the most useful features of F5 BIGIP TMOS is the
flexibility provided by iRules.

With APM and iRules you can accomplish many things, in fact you can now
use iRules to create APM sessions. We are not going to go over that here
however for the purpose of how iRules can be used for troubleshooting we
will provide some highlights.

Often you can run into problems wherein an application single sign-on is
not being processed and completing as it should. What happens as a
result of the initial setup not working immediately is that many people
start second guessing what is happening as traffic passes from the
clients browser, to the front client side of the BIGIP VIP, then what F5
VIP is actually able to SEE, next What does LTM see, APM see, what is
being passed along the way at each stage of the transaction through the
BIGIP, and of course what does the BIGIP APM then forward to the Backend
Server Application and How does that Backend Server Application respond?
Fortunately, iRules can be very beneficial in this process to collect
and subsequently log specific data at each stage which greatly enhances
the troubleshooting capabilities.

We all know that TCPDump can be your friend in capturing data to analyze
however at times the application workflows between client f5 and server
and encryption along the way can hamper what TCPDump could capture for
analysis. Another issue with TCPDump is that is captures a lot of data
that then needs to be analyzed. Granted TCPDump provides a filtering
capability to weed through that extra data however when you compare it
to using some targeted iRules to collect APM session variables and data
to be output to logs it makes it easier to review the application flow
more specific to the steps you are trying to validate.

By default, APM in the current code release automatically secures that
variables that are entered into the logon page on APM. Furthermore, the
password is hidden from the reports screen session variable view and
hidden from the database. Yet there are times when the Admin of the APM
may need to have access to the decrypted password to either verify that
the correct information is being keyed by user, received by APM and sent
from APM to servers. Fortunately, there is a way using an iRule to do
just this for our troubleshooting purpose.

TEST 1

1.  First open a console session to the BIGIP.

2.  From the command prompt type: **tail –f /var/log/ltm**

3.  Hit the enter key several times to move the text on the screen up to
    the top so you have a clear screen to start reviewing log data
    during this test.

4.  Now open a browser and access the APM VIP and logon as a user.

5.  When you reach the end of your APM policy take a look at the console
    session and note whether or not the logs provide any details about
    the username or password you just used to logon to APM.

6.  Now in another browser open the APM Admin GUI.

7.  Go to the reports screen and run the All Sessions Report.

8.  Open the Session Variables link for the current session you have
    just started as the user.

9.  Navigate down to the SSO folder and expand it.

10. Review the SSO Token Username and verify it displays the username
    you entered.

11. Review the SSO Token Password and verify it displays the password
    you entered. Or can you?

12. No, you cannot because it is obscured by default.

Next, we will implement an iRule to assist the Admin in verifying what
password is being entered by the user.

An iRule has been created already and supplied for you so you won’t need
to create it yourself you only need to apply it to the Virtual Server
under the Resources Tab.

STEP 2

1. Open the properties for the Virtual Server.

2. Click the resources Tab.

3. In the iRules section, click the Manage button.

4. In the right-side box scroll down to find the iRule named
   **Agility-201-Troubleshooting**

5. Highlight the iRule and click the arrow button to move it to the left
   box.

6. Click the finished button.

TEST 2

1. Navigate to Manage Sessions and Kill all existing sessions.

2. In the console screen, hit the enter key several times to move any
   existing output up to the top of the window, then enter the following
   command **tail –F /var/log/ltm**

3. In the browser for user session testing, restart the session back to
   the APM VIP and logon with your username and password.

4. Click through to the end of the policy.

5. Now go back to the console session and review the log messages.

6. Do you see the username you entered in the logon page?

7. Do you see the password you entered in the logon page? If you
   answered yes then you were successful. Congratulations!

TCPDump Troubleshooting Assistance
----------------------------------

Beginning in BIG-IP 11.2.0, you can use the “\ **p**\ ” interface
modifier with the “\ **p**\ ” modifier to capture traffic with TMM
information for a specific flow, and its related peer flow. The
“\ **p**\ ” modifier allows you to capture a specific traffic flow
through the BIG-IP system from end to end, even when the configuration
uses a Secure Network Address Translation (SNAT) or OneConnect. For
example, the following command searches for traffic to or from client
**10.128.10.100** on interface **0.0**:

**tcpdump -ni 0.0:nnnp -s0 -c 100000 -w /var/tmp/capture.dmp host
10.128.10.100**

Once **tcpdump** identifies a related flow, the flow is marked in TMM,
and every subsequent packet in the flow (on both sides of the BIG-IP
system) is written to the capture file.

Learn More
==========

???Add Learn more points????

Notes:

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| F5 Networks, Inc. \| f5.com                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
+======================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| US Headquarters: 401 Elliott Ave W, Seattle, WA 98119 \| 888-882-4447 // Americas: info@f5.com // Asia-Pacific: apacinfo@f5.com // Europe/Middle East/Africa: emeainfo@f5.com // Japan: f5j-info@f5.com                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ©2017 F5 Networks, Inc. All rights reserved. F5, F5 Networks, and the F5 logo are trademarks of F5 Networks, Inc. in the U.S. and in certain other countries. Other F5 trademarks are identified at f5.com. Any other products, services, or company names referenced herein may be trademarks of their respective owners with no endorsement or affiliation, express or implied, claimed by F5. These training materials and documentation are F5 Confidential Information and are subject to the F5 Networks Reseller Agreement. You may not share these training materials and documentation with any third party without the express written permission of F5.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. |image0| image:: media/image2.tiff
   :width: 6.48475in
   :height: 4.17870in
.. |image1| image:: media/image3.png
   :width: 5.30000in
   :height: 3.34687in
.. |image2| image:: media/image4.png
   :width: 5.30000in
   :height: 1.87749in
.. |image3| image:: media/image5.png
   :width: 5.28125in
   :height: 7.47544in
.. |image4| image:: media/image6.png
   :width: 5.30000in
   :height: 1.48855in
.. |image5| image:: media/image7.png
   :width: 5.28125in
   :height: 3.99637in
.. |image6| image:: media/image9.png
   :width: 5.30972in
   :height: 2.05069in
.. |image7| image:: media/image10.png
   :width: 5.21875in
   :height: 0.71782in
.. |image8| image:: media/image11.png
   :width: 5.21875in
   :height: 6.02240in
.. |image9| image:: media/image13.png
   :width: 5.30972in
   :height: 2.66111in
.. |image10| image:: media/image14.png
   :width: 5.30000in
   :height: 0.79642in
.. |image11| image:: media/image16.png
   :width: 5.30972in
   :height: 6.01667in
.. |image12| image:: media/image18.png
   :width: 5.30972in
   :height: 1.95069in
.. |image13| image:: media/image19.png
   :width: 5.30000in
   :height: 0.42589in
.. |image14| image:: media/image21.png
   :width: 5.30972in
   :height: 2.25208in
.. |image15| image:: media/image22.png
   :width: 5.23333in
   :height: 2.07270in
.. |image16| image:: media/image23.png
   :width: 5.18567in
   :height: 2.05208in
.. |image17| image:: media/image24.png
   :width: 5.25792in
   :height: 2.94792in
.. |image18| image:: media/image25.png
   :width: 5.30000in
   :height: 0.88333in
.. |image19| image:: media/image26.png
   :width: 5.23958in
   :height: 5.90988in
.. |image20| image:: media/image27.png
   :width: 5.28571in
   :height: 2.00000in
.. |image21| image:: media/image28.png
   :width: 5.30000in
   :height: 0.47834in
.. |image22| image:: media/image29.png
   :width: 5.27083in
   :height: 3.12743in
.. |image23| image:: media/image30.png
   :width: 5.19792in
   :height: 5.54507in
.. |image24| image:: media/image31.png
   :width: 5.30913in
   :height: 2.26042in
.. |image25| image:: media/image32.png
   :width: 5.30000in
   :height: 1.04073in
.. |image26| image:: media/image33.png
   :width: 1.33004in
   :height: 0.80208in
.. |image27| image:: media/image34.png
   :width: 5.25000in
   :height: 5.15331in
.. |image28| image:: media/image36.png
   :width: 5.02778in
   :height: 1.68056in
.. |image29| image:: media/image38.png
   :width: 5.30972in
   :height: 2.99931in
.. |image30| image:: media/image39.png
   :width: 5.30000in
   :height: 0.98470in
.. |image31| image:: media/image40.png
   :width: 5.30000in
   :height: 0.57609in
.. |image32| image:: media/image42.png
   :width: 5.09722in
   :height: 0.65278in
.. |image33| image:: media/image43.png
   :width: 5.30000in
   :height: 0.74486in
.. |image34| image:: media/image44.png
   :width: 5.31250in
   :height: 5.79805in
.. |image35| image:: media/image45.png
   :width: 5.24680in
   :height: 2.65625in
.. |image36| image:: media/image47.png
   :width: 5.30972in
   :height: 1.95069in
.. |image37| image:: media/image48.png
   :width: 5.30000in
   :height: 0.85074in
.. |image38| image:: media/image49.png
   :width: 5.30000in
   :height: 1.51016in
.. |image39| image:: media/image49.png
   :width: 5.30000in
   :height: 1.51016in
.. |image40| image:: media/image51.png
   :width: 5.30972in
   :height: 4.78750in
.. |image41| image:: media/image52.png
   :width: 5.27083in
   :height: 5.47535in
.. |image42| image:: media/image53.png
   :width: 5.30000in
   :height: 1.47274in
.. |image43| image:: media/image43.png
   :width: 5.30000in
   :height: 0.74486in
.. |image44| image:: media/image54.png
   :width: 5.30000in
   :height: 4.27509in
.. |image45| image:: media/image56.png
   :width: 5.30972in
   :height: 2.79931in
.. |image46| image:: media/image58.png
   :width: 5.30972in
   :height: 0.71806in
.. |image47| image:: media/image59.png
   :width: 5.30000in
   :height: 1.05629in
.. |image48| image:: media/image60.png
   :width: 5.30000in
   :height: 1.88883in
.. |image49| image:: media/image61.png
   :width: 5.30000in
   :height: 1.13638in
.. |image50| image:: media/image62.png
   :width: 5.30000in
   :height: 3.50845in
.. |image51| image:: media/image63.png
   :width: 5.31250in
   :height: 3.55414in
.. |image52| image:: media/image64.png
   :width: 5.27045in
   :height: 3.28125in
.. |image53| image:: media/image66.png
   :width: 5.30972in
   :height: 1.71875in
.. |image54| image:: media/image67.png
   :width: 5.30000in
   :height: 0.95176in
.. |image55| image:: media/image68.png
   :width: 5.28361in
   :height: 2.26042in
.. |image56| image:: media/image69.png
   :width: 5.30000in
   :height: 0.95176in
.. |image57| image:: media/image70.png
   :width: 5.30000in
   :height: 1.16637in
.. |image58| image:: media/image71.png
   :width: 1.53743in
   :height: 0.80214in
.. |image59| image:: media/image72.png
   :width: 0.48663in
   :height: 0.31660in
.. |image60| image:: media/image73.png
   :width: 0.58333in
   :height: 0.28125in
.. |image61| image:: media/image74.png
   :width: 0.19792in
   :height: 0.21875in
.. |image62| image:: media/image75.png
   :width: 0.81250in
   :height: 0.39583in
.. |image63| image:: media/image76.png
   :width: 0.68750in
   :height: 0.72917in
.. |image64| image:: media/image77.png
   :width: 0.67708in
   :height: 0.39583in
.. |image65| image:: media/image78.png
   :width: 0.77986in
   :height: 0.66845in
.. |image66| image:: media/image79.png
   :width: 1.07292in
   :height: 0.39583in
.. |image67| image:: media/image80.png
   :width: 2.33155in
   :height: 0.70525in
.. |image68| image:: media/image81.png
   :width: 2.34225in
   :height: 1.17833in
.. |image69| image:: media/image82.png
   :width: 2.31788in
   :height: 0.92338in
.. |image70| image:: media/image83.jpeg
   :width: 2.32450in
   :height: 2.08460in
.. |image71| image:: media/image84.png
   :width: 5.30000in
   :height: 0.76798in
.. |image72| image:: media/image85.png
   :width: 5.30000in
   :height: 1.12169in
.. |image73| image:: media/image86.png
   :width: 5.30000in
   :height: 1.99478in
.. |image74| image:: media/image87.png
   :width: 5.33708in
   :height: 2.82292in
.. |image75| image:: media/image88.png
   :width: 5.30000in
   :height: 1.15790in
.. |image76| image:: media/image89.png
   :width: 5.30000in
   :height: 1.85127in
.. |image77| image:: media/image90.png
   :width: 5.30000in
   :height: 0.66807in
.. |image78| image:: media/image62.png
   :width: 5.24002in
   :height: 3.46875in
.. |image79| image:: media/image91.png
   :width: 5.24301in
   :height: 2.67708in
.. |image80| image:: media/image92.png
   :width: 5.30000in
   :height: 1.49559in
.. |image81| image:: media/image93.png
   :width: 5.30000in
   :height: 1.53719in
.. |image82| image:: media/image94.png
   :width: 5.30000in
   :height: 1.36501in
.. |image83| image:: media/image84.png
   :width: 5.30000in
   :height: 0.76798in
.. |image84| image:: media/image95.png
   :width: 5.27083in
   :height: 2.63542in
.. |image85| image:: media/image96.png
   :width: 5.30000in
   :height: 0.65187in
.. |image86| image:: media/image97.png
   :width: 5.30000in
   :height: 1.74143in
.. |image87| image:: media/image62.png
   :width: 5.27083in
   :height: 3.48915in
.. |image88| image:: media/image91.png
   :width: 5.16140in
   :height: 2.63542in
.. |image89| image:: media/image98.png
   :width: 5.30000in
   :height: 1.50820in
.. |image90| image:: media/image99.png
   :width: 5.30000in
   :height: 1.68697in
.. |image91| image:: media/image100.png
   :width: 5.30000in
   :height: 1.22439in
.. |image92| image:: media/image101.png
   :width: 5.30000in
   :height: 0.79244in
.. |image93| image:: media/image103.png
   :width: 5.30972in
   :height: 2.69931in
.. |image94| image:: media/image105.png
   :width: 5.30972in
   :height: 0.59444in
.. |image95| image:: media/image91.png
   :width: 5.26341in
   :height: 2.68750in
.. |image96| image:: media/image107.png
   :width: 5.30972in
   :height: 0.75764in
.. |image97| image:: media/image109.png
   :width: 5.30972in
   :height: 1.15208in
.. |image98| image:: media/image111.png
   :width: 5.30972in
   :height: 2.97292in
.. |image99| image:: media/image113.png
   :width: 5.30972in
   :height: 0.60972in
.. |image100| image:: media/image115.png
   :width: 5.30972in
   :height: 0.93889in
.. |image101| image:: media/image116.png
   :width: 5.30303in
   :height: 4.37500in
.. |image102| image:: media/image117.png
   :width: 7.12660in
   :height: 6.38679in
.. |image103| image:: media/image118.png
   :width: 5.19522in
   :height: 2.69811in
.. |image104| image:: media/image119.png
   :width: 4.10232in
   :height: 2.00000in
.. |image105| image:: media/image120.png
   :width: 5.42016in
   :height: 2.34906in
.. |image106| image:: media/image121.png
   :width: 5.40905in
   :height: 2.59434in
.. |image107| image:: media/image122.png
   :width: 5.30000in
   :height: 0.85031in
.. |image108| image:: media/image123.png
   :width: 5.30000in
   :height: 0.84337in
.. |image109| image:: media/image124.png
   :width: 5.28746in
   :height: 3.27358in
.. |image110| image:: media/image90.png
   :width: 5.30000in
   :height: 0.66807in
.. |image111| image:: media/image62.png
   :width: 5.30000in
   :height: 3.50845in
.. |image112| image:: media/image125.png
   :width: 5.11837in
   :height: 2.00000in
.. |image113| image:: media/image126.png
   :width: 5.30000in
   :height: 0.84941in
.. |image114| image:: media/image127.png
   :width: 5.30972in
   :height: 4.79653in
.. |image115| image:: media/image128.png
   :width: 5.30000in
   :height: 1.20644in
.. |image116| image:: media/image129.png
   :width: 5.30000in
   :height: 1.82419in
.. |image117| image:: media/image130.png
   :width: 5.30000in
   :height: 1.69202in
.. |image118| image:: media/image131.png
   :width: 5.32157in
   :height: 2.28302in
.. |image119| image:: media/image132.png
   :width: 5.34906in
   :height: 4.64626in
.. |image120| image:: media/image133.png
   :width: 5.31606in
   :height: 2.28302in
.. |image121| image:: media/image134.png
   :width: 5.34736in
   :height: 2.14151in
.. |image122| image:: media/image135.png
   :width: 5.30000in
   :height: 1.20906in
.. |image123| image:: media/image136.png
   :width: 5.40652in
   :height: 2.17924in
.. |image124| image:: media/image137.png
   :width: 5.30000in
   :height: 0.67850in
.. |image125| image:: media/image62.png
   :width: 5.32075in
   :height: 3.52219in
.. |image126| image:: media/image138.png
   :width: 5.29448in
   :height: 2.56604in
.. |image127| image:: media/image140.png
   :width: 5.30972in
   :height: 2.95764in
.. |image128| image:: media/image141.png
   :width: 5.30000in
   :height: 0.31129in
.. |image129| image:: media/image142.png
   :width: 7.13208in
   :height: 4.56955in
.. |image130| image:: media/image143.png
   :width: 5.30000in
   :height: 1.16923in
.. |image131| image:: media/image145.png
   :width: 5.30972in
   :height: 4.63194in
.. |image132| image:: media/image147.png
   :width: 5.30972in
   :height: 6.07083in
.. |image133| image:: media/image148.png
   :width: 5.30000in
   :height: 1.12308in
.. |image134| image:: media/image149.png
   :width: 5.30000in
   :height: 0.93846in
.. |image135| image:: media/image150.png
   :width: 5.29570in
   :height: 3.03125in
.. |image136| image:: media/image151.png
   :width: 5.30000in
   :height: 0.98462in
.. |image137| image:: media/image152.png
   :width: 5.30000in
   :height: 0.98025in
.. |image138| image:: media/image153.png
   :width: 5.30000in
   :height: 0.90810in
.. |image139| image:: media/image154.png
   :width: 5.30000in
   :height: 1.37069in
.. |image140| image:: media/image155.png
   :width: 5.30000in
   :height: 1.09365in
.. |image141| image:: media/image156.png
   :width: 5.30000in
   :height: 0.91667in
.. |image142| image:: media/image157.png
   :width: 5.30000in
   :height: 0.62207in
.. |image143| image:: media/image158.png
   :width: 5.30972in
   :height: 2.10556in
.. |image144| image:: media/image159.png
   :width: 5.30972in
   :height: 1.06944in
.. |image145| image:: media/image160.png
   :width: 5.30972in
   :height: 4.00625in
.. |image146| image:: media/image34.png
   :width: 5.30000in
   :height: 5.20239in
.. |image147| image:: media/image162.png
   :width: 5.30000in
   :height: 1.79246in
.. |image148| image:: media/image62.png
   :width: 5.20855in
   :height: 3.44792in
.. |image149| image:: media/image163.png
   :width: 5.30650in
   :height: 2.30208in
.. |image150| image:: media/image165.png
   :width: 5.30972in
   :height: 3.97778in
.. |image151| image:: media/image166.png
   :width: 5.30874in
   :height: 2.17708in
.. |image152| image:: media/image167.png
   :width: 5.36458in
   :height: 5.70163in
.. |image153| image:: media/image168.png
   :width: 5.30000in
   :height: 1.03609in
.. |image154| image:: media/image169.png
   :width: 5.30000in
   :height: 0.62673in
.. |image155| image:: media/image170.png
   :width: 5.30000in
   :height: 0.44278in
.. |image156| image:: media/image171.png
   :width: 5.30863in
   :height: 2.36458in
.. |image157| image:: media/image167.png
   :width: 5.30000in
   :height: 5.63299in
.. |image158| image:: media/image172.png
   :width: 5.30000in
   :height: 1.03018in
.. |image159| image:: media/image173.png
   :width: 5.30000in
   :height: 0.84903in
.. |image160| image:: media/image174.png
   :width: 5.30000in
   :height: 0.93630in
.. |image161| image:: media/image175.png
   :width: 5.35417in
   :height: 3.94587in
.. |image162| image:: media/image176.png
   :width: 5.28105in
   :height: 2.06250in
.. |image163| image:: media/image177.png
   :width: 5.33333in
   :height: 4.00000in
.. |image164| image:: media/image178.png
   :width: 5.30000in
   :height: 1.08922in
.. |image165| image:: media/image179.png
   :width: 5.30000in
   :height: 1.44665in
.. |image166| image:: media/image180.png
   :width: 5.30000in
   :height: 0.62353in
.. |image167| image:: media/image171.png
   :width: 5.31250in
   :height: 2.36631in
.. |image168| image:: media/image181.png
   :width: 5.30000in
   :height: 3.32850in
.. |image169| image:: media/image182.png
   :width: 5.30000in
   :height: 0.66085in
.. |image170| image:: media/image47.png
   :width: 5.30972in
   :height: 1.95069in
.. |image171| image:: media/image184.png
   :width: 5.30972in
   :height: 1.00139in
.. |image172| image:: media/image186.png
   :width: 5.30972in
   :height: 2.29722in
.. |image173| image:: media/image188.png
   :width: 5.30972in
   :height: 2.81458in
.. |image174| image:: media/image189.png
   :width: 5.30000in
   :height: 0.65717in
.. |image175| image:: media/image171.png
   :width: 5.33201in
   :height: 2.37500in
.. |image176| image:: media/image190.png
   :width: 5.30000in
   :height: 3.00185in
.. |image177| image:: media/image191.png
   :width: 4.73405in
   :height: 7.02083in
.. |image178| image:: media/image192.png
   :width: 4.19722in
   :height: 0.55208in
.. |image179| image:: media/image193.png
   :width: 4.20764in
   :height: 0.53125in
.. |image180| image:: media/image194.png
   :width: 4.16597in
   :height: 0.51042in
.. |image181| image:: media/image195.png
   :width: 7.12500in
   :height: 3.23000in
.. |image182| image:: media/image196.png
   :width: 2.70833in
   :height: 3.44092in
.. |image183| image:: media/image197.png
   :width: 5.30000in
   :height: 1.98962in
.. |image184| image:: media/image198.png
   :width: 5.30000in
   :height: 0.45050in
.. |image185| image:: media/image199.png
   :width: 5.30000in
   :height: 0.43945in
.. |image186| image:: media/image200.png
   :width: 5.31250in
   :height: 7.78721in
