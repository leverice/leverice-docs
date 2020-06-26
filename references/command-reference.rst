.. _command-reference-label:

Command Reference
=================

This document contains descriptions of Leverice commands, listed alphabetically. Each description contains the following parts:

* small description about usage
* syntax
* examples

Each command starts with **slash (/)**. For example:

.. code-block:: bash

 /archive
 /unarchive

Command may have different types of arguments listed below:

* named string argument. It starts with **single minus (-)**. For example:

.. code-block:: bash

 -m "message body"
 -name "Rodion"

* named boolean argument. It starts with **two minuses (--)**. For example:

.. code-block:: bash

 --make-private
 --pinned

* unnamed arguments. Usually there are other words after command, which don't starts with minuses. For example in following `unarchive`_ command text **"MyReason"** is unnamed argument:

.. code-block:: bash

 /unarchive "MyReason"

archive
#######
 Archives current channel. In terms of Leverice that means ending current discussion and hide this channel for current user and for others who suggest to archive this channel too. Archived channels may unarchive using `unarchive`_ command or by sending post to this channel

**Syntax:**

.. code-block:: bash

 /archive

**Arguments:**

This command don't have any arguments

**Examples:**

.. code-block:: bash

 /archive

post
####
 Send message in current channel

**Syntax:**

.. code-block:: bash

 /post -m "Your message"

**Arguments:**

+----------+------------------------+----------+-------------------------+
| Name     | Type                   | Required | Description             |
+==========+========================+==========+=========================+
| m        | named string argument  | yes      | text of message to send |
+----------+------------------------+----------+-------------------------+

**Examples:**

.. code-block:: bash

 /post -m "Hello world"
 /post -m "I send my first message!"

subscribe
#########
 Subscribes to current public channel to provide possibility to post and normal reading messages. By default, if you are not subscribed, you can see only 30 last messages

**Syntax:**

.. code-block:: bash

 /subscribe

**Arguments:**

This command don't have any arguments

**Examples:**

.. code-block:: bash

 /subscribe

unarchive
#########
 Unarchives current channel which was archived using `archive`_ command. After execution channel will be visible for all, who archives it earlier

**Syntax:**

.. code-block:: bash

 /unarchive ["Reason"]

**Arguments:**

+----------+--------------------------+----------+-------------------------+
| Name     | Type                     | Required | Description             |
+==========+==========================+==========+=========================+
|          | Unnamed string argument  | No       | Reason for unarchiving  |
+----------+--------------------------+----------+-------------------------+

**Examples:**

.. code-block:: bash

 /unarchive
 /unarchive "Want to discuss about one more thing"

.. note::
 if you specify reason for example "Want to discuss about one more thing", you will see it in system message about successful operation:
  Channel unarchived because *Your Name* **Want to discuss about one more thing**.

unsubscribe
###########
 Unsubscribes or in other words leaves current channel. After running this command you cannot post messages here and see this channel at all, if it is not public. If this channel is public, you can subscribe to it again using `subscribe`_ command

**Syntax:**

.. code-block:: bash

 /unsubscribe

**Arguments:**

This command don't have any arguments

**Examples:**

.. code-block:: bash

 /unsubscribe