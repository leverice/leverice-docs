Introduction to Leverice
========================

 Leverage a platform designed to be a powerful engine of team management and productivity: fueled by focus, not distraction.

It may sounds like a good advertisement, but what it actually means for us. Let's investigate it.

How i meet my Leverice
######################

First of all, you need to login in your workspace in `Leverice <https://leverice.com/public/client/>`_.  Then create any channel using context menu and send message. It was easy, right? Prepare yourself for following statement. In Leverice:

Everything is a command
#######################

Every action, which you can do through UI, you can simply do using commands, which you can put in input field and send as usual message. Each command starts with **slash (/)**. For example:

.. code-block:: bash

 /archive
 /unarchive

There is few types of commands:

* commands with prefix **ro:** (read-only). This commands usually retrieve some information from application and do it in synchronous way. In other words current request will wait response from backend. For example:

.. code-block:: bash

 /ro:retrieveChannelSettings
 /ro:retrieveUserRights

* commands with prefix **fe:** (front end). This commands call some UI forms or change some elements on UI. For example:

.. code-block:: bash

 /fe:collapse
 /fe:expand
 /fe:inviteMembers

* commands without prefixes. Usual commands which can change state of application, for example manipulation with channel tree, messages users in channel etc.

Also, command may have different types of arguments. Let's explain them:

* named string argument. It starts with **single minus (-)**. In following command **-m** is named argument which means message body of our post:

.. code-block:: bash

 /post -m "message body"

* named boolean argument. It starts with **two minuses (--)** and not contains additional value after. In following command **--make-private** is named boolean argument which means that created channel will be visible only for you at moment of creation:

.. code-block:: bash

 /createChannel -channel-type default.team -name "MyTeam1" --make-private -position.parentChannelId "11111111111" -source.channelId "11111111111"

* unnamed arguments. Usually there are other words after command, which don't starts with minuses. For example in following command text **"MyReason"** is unnamed argument:

.. code-block:: bash

 /unarchive "MyReason"

It was a theory. Let's practise usage of commands. Switch to any channel and paste in input following line then send it as usual message:

.. code-block:: bash

 /post -m "Sending post via command works!"

As you can see, your message appeared in channel. As you remember, argument **-m** in this case is named string argument, which means body of our message. Now create new channel under current using following command and switch to it:

.. code-block:: bash

 /createChannel -channel-type default.public -name "MyChannel1" && /cd "MyChannel1"

Let's talk about it. In Leverice you can send chain of commands separated by **&&**. It may very useful for aromatization of some work related to creation of bunches of channels or sending several messages and etc.
Let's explain commands. First command, *createChannel* uses to create channel and we passed 2 named arguments

* **-name** - name of channel to create. Should not contains both slashes (\ and /)
* **-channel-type** - predefined channel type. Full list of available types you can find in :ref:`channel-type-reference-label`. In current case we set usual public channel as type

Second command *cd* allows to switch channel to specified in unnamed argument. It may have following types:

* relative channel name from current. If our current channel have child channel with name "Channel1", you can switch to it using command

.. code-block:: bash

 /cd "Channel1"

* absolute path from "root" of workspace. This path should start with **slash (/)** and should contains all channel names from root-parent of needed channel to needed channel, separated by slashes. For example, if you need switch to channel "MyChannel" under channel "Announcements" which is root channel in workspace, you should call

.. code-block:: bash

 /cd "/Announcements/MyChannel"

If you can change name of current channel, you can run following command where is named argument *-n* means new channel name:

.. code-block:: bash

 /moveRenameChannel -n "Renamed Channel"

Full list of available command you can find in :ref:`command-reference-label`. In further documents we will explain you programming aspects in Leverice