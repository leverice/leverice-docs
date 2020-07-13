.. _introduction-label:

Introduction to Leverice
========================

 Leverice is a platform designed to be a powerful engine of team management and productivity: fueled by focus, not distraction.

It may sounds like a good advertisement, but what it actually means for us. Let's investigate it.

.. note::

 Many of the developer features that you will learn in this guide are not available in our live installation. If you want to try them in practice, you will need to register in our development program and get the Leverice Developer ID. Send your request to dev-program@leverice.com. Got Leverice Developer ID? Nice, then you need to login in Leverice using provided credentials.

Everything is a command
#######################

Every action, which you can do through UI, you can simply do using commands, which you can put in input field and send as usual message. Each command starts with **slash (/)**. For example try send following command:

.. code-block:: bash

 /archive

You will see message like **"Your name archived this channel"**. If you switch from this channel to another one, you cannot see him without toggling filter "Show archived channels". To undo this state run following command:

.. code-block:: bash

 /unarchive

You will see message **"Channel unarchived by Your Name"** and channel will be returned in normal state. Let's talk about command types.

Transaction support
-------------------

Every command: single, chain of commands or commands for child channels runs in transaction. That means if there is any error during execution, all actions before error will be rolled back. For example, following command chain contains command for sending message and switching in not existing command:

.. code-block:: bash

 /post -m "New message which should not appear" && /cd "NotExistingChannel"

After running this command you will see error message and message will noy be appear in channel.

Type of commands
----------------

There are a three types of commands

Commands without prefixes
^^^^^^^^^^^^^^^^^^^^^^^^^

Usual commands which not contains any special prefixes and can change state of application, for example manipulation with channel tree, messages users in channel etc.

RO-commands
^^^^^^^^^^^

Commands with prefix **ro:** (read-only). This commands usually retrieve some information from application and do it in synchronous way. In other words current request will wait response from backend. For example, following command:

.. code-block:: bash

 /ro:retrieveUserRights

will return response like bellow, which shows your rights (or permissions) in current workspace:

.. code-block:: json

 {
  "messageType": "COMMAND_EXECUTED_CLIENT_MESSAGE",
  "events": [
    {
      "messageType": "USER_RIGHTS_EVENT",
      "rights": [
        "c.add_role",
        "c.add_tag",
        "c.create_channel",
        "c.delete",
        "c.download_file",
        "c.install_integrations",
        "c.manage_subscribers",
        "c.read",
        "c.remove_role",
        "c.remove_tag",
        "c.see",
        "c.subscribe",
        "c.update",
        "c.update_prefix",
        "c.upload_file",
        "c.write"
      ],
      "channelId": "1nKvCCRVUKe",
      "projectId": "1ZeoxCXm239",
      "crtd": 1593598828187
    }
  ],
  "status": "success",
  "correlationId": "1593598791822:9"
 }

.. note::

 You can see response of ro-commands only in developer tools of your browser

FE-commands
^^^^^^^^^^^

Commands with prefix **fe:** (front end). This commands call some UI forms or change some elements on UI. Try to run following command:

.. code-block:: bash

 /fe:inviteMembers

You will see new invitation window for your workspace. You can show this window also through context menu of workspace by choosing menu "Invite Members"

Type of arguments
-----------------

Commands may have arguments. And there are three types of them. Let's explain each one.

Named string argument
^^^^^^^^^^^^^^^^^^^^^

Starts with **single minus (-)**. In following command **-m** is named argument which means message body of our post. Try to run following command in any channel:

.. code-block:: bash

 /post -m "message body"

You will see your message as sent in current channel. Try to replace "message body" with your string and send it too. Got it? Nice, go to next type of argument.

Named boolean argument
^^^^^^^^^^^^^^^^^^^^^^

Starts with **two minuses (--)** and not contains additional value after it. In following command **--make-private** is named boolean argument which means that created channel will be visible only for you at moment of creation. Let's create it using following command:

.. code-block:: bash

 /createChannel -channel-type default.team -name "MyTeam1" --make-private -position.parentChannelId "11111111111" -source.channelId "11111111111"

Other mandatory arguments are:

* **-name** - name of channel to create. Should not contains both slashes (\\ and /)
* **-channel-type** - predefined channel type. Full list of available types you can find in :ref:`channel-type-reference-label`. In current case we set folder as type

Don't take a look on other arguments in this command, we will explain them a bit later. After executing this command you will see folder-like channel with name **MyTeam1**. Switch to it using UI and run another command dor channel creation:

.. code-block:: bash

 /createChannel -channel-type default.public -name "MyChannel1"

You should see channel **"MyChannel1"** under **"MyTeam1"**

Unnamed arguments
^^^^^^^^^^^^^^^^^

Usually there are other words after command, which don't starts with minuses. For example in following command text **"/MyTeam1"** is unnamed argument, which means channel path for switching. Try to run following command:

.. code-block:: bash

 /cd "/MyTeam1"

You will see folder **"MyTeam1"** as current channel. You can try ti switch via UI to another channel and run this command again.

Lets's talk about channel path, using in **cd** command. There are 3 types:

* Absolute path from "root" of workspace. This path should start with **slash (/)** and should contains all channel names from root-parent of needed channel to needed channel, separated by slashes. For example, if you need switch to channel **"MyChannel1"** under folder **"MyTeam1"** which is root channel in workspace, you should call:

.. code-block:: bash

 /cd "/MyTeam1/MyChannel1"

* POSIX-like path **".."**, which means parent channel from current. If your current channel is **"MyChannel1"** under **"MyTeam1"**, running following command will switch you to **"MyTeam1"**:

.. code-block:: bash

 /cd ..

After executing of this command your current channel should be **"MyTeam1"**

* Relative path from current channel. This path should contain child channel names separated by slashes. For example, if you need switch to channel **"MyChannel1"** and your current channel is **"MyTeam1"**, you should run:

.. code-block:: bash

 /cd "MyChannel1"

After executing of this command your current channel should be **"MyChannel1"**

Full list of available command you can find in :ref:`command-reference-label`. In further documents we will explain you programming aspects in Leverice

Advanced reading
----------------

Following tips are important to know, when you start using commands

Chain of commands
^^^^^^^^^^^^^^^^^

There is a possibility to send more than one command in single message. To use this possibility, you should put **&&** between commands. To demonstrate it, we will create 3 channels under current using following command chain:

.. code-block:: bash

 /createChannel -channel-type default.public -name "MyChannelForChain1" && /createChannel -channel-type default.public -name "MyChannelForChain2Test" && /createChannel -channel-type default.public -name "MyChannelForChain3Test"

You will see channels **"MyChannelForChain1"**, **"MyChannelForChain2Test"** and **"MyChannelForChain3Test"**

Run commands in child channels using wildcard
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

From `Chain of commands`_ you have three channels under current: **"MyChannelForChain1"**, **"MyChannelForChain2Test"** and **"MyChannelForChain3Test"**. If you want to run some command on child channels, you can use command **in** with following syntax:

.. code-block:: bash

 /in [wildcard or channels list] -do [command]

For example, following command sends message in all child channels. Try it:

.. code-block:: bash

 /in * -do /post -m "Message for all child channels"

You can switch to every channel and check that message was sent in each one. Then switch to the parent channel and try to run command, which will send message in channels which end by "Test" (**"MyChannelForChain2Test"** and **"MyChannelForChain3Test"**):

.. code-block:: bash

 /in "*Test" -do /post -m "Message for channels which end with Test"

As you can see, channel **"MyChannelForChain1"** does not contains new message. Another channels have new message. Also, you can just set channel names in command. For example:

.. code-block:: bash

 /in "MyChannelForChain1" "MyChannelForChain2Test" -do /post -m "Message for first and second channel"

And you will see message only in **"MyChannelForChain1"**, **"MyChannelForChain2Test"**