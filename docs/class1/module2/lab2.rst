LAB II – Visual Policy Editor (VPE)
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

.. |image58| image:: /_static/image71.png
   :width: 1.53743in
   :height: 0.80214in
.. |image59| image:: /_static/image72.png
   :width: 0.48663in
   :height: 0.31660in
.. |image60| image:: /_static/image73.png
   :width: 0.58333in
   :height: 0.28125in
.. |image61| image:: /_static/image74.png
   :width: 0.19792in
   :height: 0.21875in
.. |image62| image:: /_static/image75.png
   :width: 0.81250in
   :height: 0.39583in
.. |image63| image:: /_static/image76.png
   :width: 0.68750in
   :height: 0.72917in
.. |image64| image:: /_static/image77.png
   :width: 0.67708in
   :height: 0.39583in
.. |image65| image:: /_static/image78.png
   :width: 0.77986in
   :height: 0.66845in
.. |image66| image:: /_static/image79.png
   :width: 1.07292in
   :height: 0.39583in
.. |image67| image:: /_static/image80.png
   :width: 2.33155in
   :height: 0.70525in
.. |image68| image:: /_static/image81.png
   :width: 2.34225in
   :height: 1.17833in
.. |image69| image:: /_static/image82.png
   :width: 2.31788in
   :height: 0.92338in
.. |image70| image:: /_static/image83.jpeg
   :width: 2.32450in
   :height: 2.08460in
.. |image71| image:: /_static/image84.png
   :width: 5.30000in
   :height: 0.76798in
.. |image72| image:: /_static/image85.png
   :width: 5.30000in
   :height: 1.12169in
.. |image73| image:: /_static/image86.png
   :width: 5.30000in
   :height: 1.99478in
.. |image74| image:: /_static/image87.png
   :width: 5.33708in
   :height: 2.82292in
.. |image75| image:: /_static/image88.png
   :width: 5.30000in
   :height: 1.15790in
.. |image76| image:: /_static/image89.png
   :width: 5.30000in
   :height: 1.85127in
.. |image77| image:: /_static/image90.png
   :width: 5.30000in
   :height: 0.66807in
.. |image78| image:: /_static/image62.png
   :width: 5.24002in
   :height: 3.46875in
.. |image79| image:: /_static/image91.png
   :width: 5.24301in
   :height: 2.67708in
.. |image80| image:: /_static/image92.png
   :width: 5.30000in
   :height: 1.49559in
.. |image81| image:: /_static/image93.png
   :width: 5.30000in
   :height: 1.53719in
.. |image82| image:: /_static/image94.png
   :width: 5.30000in
   :height: 1.36501in
.. |image83| image:: /_static/image84.png
   :width: 5.30000in
   :height: 0.76798in
.. |image84| image:: /_static/image95.png
   :width: 5.27083in
   :height: 2.63542in
.. |image85| image:: /_static/image96.png
   :width: 5.30000in
   :height: 0.65187in
.. |image86| image:: /_static/image97.png
   :width: 5.30000in
   :height: 1.74143in
.. |image87| image:: /_static/image62.png
   :width: 5.27083in
   :height: 3.48915in
.. |image88| image:: /_static/image91.png
   :width: 5.16140in
   :height: 2.63542in
.. |image89| image:: /_static/image98.png
   :width: 5.30000in
   :height: 1.50820in
.. |image90| image:: /_static/image99.png
   :width: 5.30000in
   :height: 1.68697in
.. |image91| image:: /_static/image100.png
   :width: 5.30000in
   :height: 1.22439in
.. |image92| image:: /_static/image101.png
   :width: 5.30000in
   :height: 0.79244in
.. |image93| image:: /_static/image103.png
   :width: 5.30972in
   :height: 2.69931in
.. |image94| image:: /_static/image105.png
   :width: 5.30972in
   :height: 0.59444in
.. |image95| image:: /_static/image91.png
   :width: 5.26341in
   :height: 2.68750in
.. |image96| image:: /_static/image107.png
   :width: 5.30972in
   :height: 0.75764in
.. |image97| image:: /_static/image109.png
   :width: 5.30972in
   :height: 1.15208in
.. |image98| image:: /_static/image111.png
   :width: 5.30972in
   :height: 2.97292in
.. |image99| image:: /_static/image113.png
   :width: 5.30972in
   :height: 0.60972in
.. |image100| image:: /_static/image115.png
   :width: 5.30972in
   :height: 0.93889in
