LAB III – Session Variables
======================================================
This section will provide some guidance on how to both retrieve and set
session variables within a policy for a user’s session. Session
Variables are very useful in many areas of policy execution. They can be
used to assist in areas like authentication or single sign-on or
assigning resource items for users based on information APM can collect
from the backend AAA server and its associated directory.

Use of session variables
-------------------------------------------------------------------
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

.. |image101| image:: /_static/image116.png
   :width: 5.30303in
   :height: 4.37500in
.. |image102| image:: /_static/image117.png
   :width: 7.12660in
   :height: 6.38679in
.. |image103| image:: /_static/image118.png
   :width: 5.19522in
   :height: 2.69811in
.. |image104| image:: /_static/image119.png
   :width: 4.10232in
   :height: 2.00000in
.. |image105| image:: /_static/image120.png
   :width: 5.42016in
   :height: 2.34906in
.. |image106| image:: /_static/image121.png
   :width: 5.40905in
   :height: 2.59434in
.. |image107| image:: /_static/image122.png
   :width: 5.30000in
   :height: 0.85031in
.. |image108| image:: /_static/image123.png
   :width: 5.30000in
   :height: 0.84337in
.. |image109| image:: /_static/image124.png
   :width: 5.28746in
   :height: 3.27358in
.. |image110| image:: /_static/image90.png
   :width: 5.30000in
   :height: 0.66807in
.. |image111| image:: /_static/image62.png
   :width: 5.30000in
   :height: 3.50845in
.. |image112| image:: /_static/image125.png
   :width: 5.11837in
   :height: 2.00000in
.. |image113| image:: /_static/image126.png
   :width: 5.30000in
   :height: 0.84941in
.. |image114| image:: /_static/image127.png
   :width: 5.30972in
   :height: 4.79653in
.. |image115| image:: /_static/image128.png
   :width: 5.30000in
   :height: 1.20644in
.. |image116| image:: /_static/image129.png
   :width: 5.30000in
   :height: 1.82419in
.. |image117| image:: /_static/image130.png
   :width: 5.30000in
   :height: 1.69202in
.. |image118| image:: /_static/image131.png
   :width: 5.32157in
   :height: 2.28302in
.. |image119| image:: /_static/image132.png
   :width: 5.34906in
   :height: 4.64626in
.. |image120| image:: /_static/image133.png
   :width: 5.31606in
   :height: 2.28302in
.. |image121| image:: /_static/image134.png
   :width: 5.34736in
   :height: 2.14151in
.. |image122| image:: /_static/image135.png
   :width: 5.30000in
   :height: 1.20906in
.. |image123| image:: /_static/image136.png
   :width: 5.40652in
   :height: 2.17924in
.. |image124| image:: /_static/image137.png
   :width: 5.30000in
   :height: 0.67850in
.. |image125| image:: /_static/image62.png
   :width: 5.32075in
   :height: 3.52219in
.. |image126| image:: /_static/image138.png
   :width: 5.29448in
   :height: 2.56604in
.. |image127| image:: /_static/image140.png
   :width: 5.30972in
   :height: 2.95764in
.. |image128| image:: /_static/image141.png
   :width: 5.30000in
   :height: 0.31129in
.. |image129| image:: /_static/image142.png
   :width: 7.13208in
   :height: 4.56955in
