.. meta::
  :description: This document contains all the technical information about Web API of Leverice.

.. _app-desc-reference-label:

Leverice Web API
================

Activating WebAPI in the Leverice workspace
########

Follow this steps to activate WebAPI:

- Execute the following command in any workspace channel as a Workspace Administrator: /createWapi -botName "Leverice"
- Copy and save the link from the popup window that appeared on the screen - this is the WebAPI URL to access Leverice API

What request template to use on the URL
################

On this URL we can POST requests. POST requests have two headers and payload.

HTTP Headers
--------

There are two HTTP headers that are required:

- Content-Type: application/json
- X-Request-Id: SomeUniqueId

X-Request-Id header is specific for Leverice. You shall provide a unique id for each of your requests. In case you experience any issues with some specific request, you can communicate that X-Request-Id to our support team: this will help us to find your request and see what happened to it. We ourselves usually generate it like this:

<SOME_CONSTANT>:<CURRENT_TIME_IN_MILIS>:<REQUEST_COUNTER>

For example: LVRC:1617184321337:1

Note, that it will lead to no errors on Leverice, even if you provide clashing request ids, however, this may hinder us from effectively supporting you.

Payload
--------

The POST request body looks like this:

.. code-block:: json

 {
    "channel": "channelRef",
    "command": "/commandSpec"
 }


**Where:**

- **channelRef**: is either ChannelID (standard 11 chars ID) or full channel path like: "/Folder One/Hello Channel"
- **commandSpec**: is either string like "/post -m Hello" or array of strings like: ["post", "-m", "Hello"]

The example with operation /post:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": "/post -m Hello"
 }

or

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": ["post", "-m", "Hello"]
 }

Please, note that in case you use first notation (command as one string), you shall take arguments that contain spaces in doublequotes and escape them. Like this:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": "/post -m \"Hello World!\""
 }

This is where second notation (array) comes handy:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": ["post", "-m", "Hello World!"]
 }

Of course, this doesn’t free you from escaping double quotes if they come as part of command argument:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": ["post", "-m", "Who wrote \"Clean code?\""]
 }

However, if you use any JSON framework to generate JSON for you, usually, such escaping is done by the framework itself.

Response
--------

Response comes in JSON format - unless some really weird things happen which will usually result in 5XX HTTP Response.

A JSON response will contain following properties:

- **"status"**: will be either "success" or "failed". If a request fails, the response will also contain a property “message”.
- **"message"**: if a request fails, message will give you a clue what went wrong.
- **"correlationId"**: will contain the value from your X-Request-Id header.
- **"messageType"**: technical info. Ignore it.

Other properties of the response depend on the request type.

What operations work in WebAPI
################

/inviteUser
--------

This operation which will result in inviting a user to the workspace with a particular role.

Inviting a workspace member:

.. code-block:: json

 {
    "channel": "/",
    "command": "/inviteUser -e member@gmail.com -r projectMember"
 }

Inviting a workspace administrator:

.. code-block:: json

 {
    "channel": "/",
    "command": "/inviteUser -e admin@gmail.com -r projectMember -r projectAdmin"
 }

And the successful response will be:

.. code-block:: json

 {
    {
    "messageType": "COMMAND_EXECUTED_CLIENT_MESSAGE",
    "events": [
        {
            "messageType": "NEW_USER_EVENT",
            "email": "member@gmail.com",
            "invited": true,
            "deactivated": false,
            "properties": [
                "can_be_invited+default.direct"
            ],
            "userId": "2Kr2uxauJGi",
            "projectId": "2Ffup62wDz7",
            "crtd": 1621414757734
        },
        {
            "messageType": "INVITED_RESULTS_EVENT",
            "correctEmails": [
                "member@gmail.com"
            ],
            "wrongEmails": [],
            "existedEmails": [],
            "deactivatedEmails": [],
            "crtd": 1621414757708
        }
    ],
    "status": "success",
    "correlationId": "LVRC:123321:1"
    }
 }

/deactivateUsers
--------

This operation will result in deactivating a particular user. To perform the command you’ll need to know the userId parameter, which can be gotten from the inviteUser command, for instance.

Payload example:

.. code-block:: json

 {
    "channel": "/",
    "/deactivateUsers 2Lu3iqauYAi"
 }

And the successful response will be:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "User deactivated",
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

/subscribe
--------

This operation will result in the Bot subscribing to the channel. This is necessary for some operations: for instance, you can only post via API to channels to which API bot is subscribed.

Payload example:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": "/subscribe"
 }

And the successful response will be:

.. code-block:: json

 {
    "messageType": "COMMAND_EXECUTED_CLIENT_MESSAGE",
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

Other response properties shall be ignored (may be removed in the future).

/post
--------

This operation will result in the Bot posting a post to the channel. But first of all, the bot needs to subscribe to the channel.

Payload example:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": ["post", "-m", "Hello"]
 }

And the successful response will be:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "Post accepted",
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

/ro:listChannels
--------

This operation will result in listing the channel on the workspace level or in subchannels of the specified in the command channel.

Can be executed on the workspace level:

.. code-block:: json

 {
    "channel": "/",
    "command": "/ro:listChannels"
 }

Or on the channel level:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": "/ro:listChannels"
 }

The successful response will contain a property "result" that is a map of channels. Key is channel id (that can be used in subsequent calls to Leverice API in the "channel" request property, for example). Value has three properties:

- **"name"**: Channel display name
- **"type"**: Our channel type id. Play with this command to see different channel type ids.
- **"private"**: Boolean value. If true, channel is private.

Example response:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "Channels list",
    "status": "success",
    "correlationId": "LVRC:123321:1"
    "result": {
        "2F4CHZT7epT": {
            "name": "direct",
            "type": "default.direlc",
            "private": false
        },
        "2F4CHZc1aWX": {
            "name": "Announcements",
            "type": "default.public",
            "private": false
        },
        "2F4CHZgTYMZ": {
            "name": "Getting Started",
            "type": "default.public",
            "private": false
        },
        "2F4CHZosUmH": {
            "name": "Invite Users",
            "type": "default.commandLink",
            "private": false
        },
        "2F4CQjL8Bpo": {
            "name": "Create new Folder",
            "type": "default.commandLink",
            "private": true
        },
        "2F4CQjV27Ws": {
            "name": "Miscellaneous",
            "type": "default.public",
            "private": false
        },
        "2F4CQjdv3Cw": {
            "name": "Regular channel",
            "type": "default.public",
            "private": false
        },
        "2F4CQjsFvk3": {
            "name": "Memes",
            "type": "default.public",
            "private": false
        }
    }
 }

/ro:listUsers
--------

This operation will result in listing the users in the specific channel or in the workspace. With optional flag “--with-deactivated” the deactivated users can be shown.

Can be executed on the workspace level:

.. code-block:: json

 {
    "channel": "/",
    "command": "/ro:listUsers"
 }

Or on the channel level:

.. code-block:: json

 {
    "channel": "/Announcements",
    "command": ["ro:listUsers"]
 }

The successful response will contain a property "result" that is a map of users. Key is user id. Value has following properties:

- **"firstName"**: First name of the user (may absent, if the user did not join the workspace yet);
- **"lastName"**: Last name of the user (may absent, if the user did not join the workspace yet);
- **"email"**: User’s email;
- **"status"**:
    - *ACTIVE* - user is active (joined workspace and has not been deactivated);
    - *DEACTIVATED* - user has been deactivated by WS administrator;
    - *SYSTEM* - user is bot;
    - *INVITED* - user received invite link, but did not join the workspace yet (and hasn’t been deactivated either).
- **"grantedRoles"**: Array of User’s roles ids;
- **"syntheticRoles"**: Optional. If supplied will contain an array of additional (so called synthetic) User’s roles ids.

An example of the response:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "users",
    "result": {
        "2F4CHZHUjVD": {
            "grantedRoles": [
                "projectAdmin",
                "projectMember"
            ],
            "firstName": "Alice",
            "lastName": "Alison",
            "email": "alice@gmail.com",
            "status": "ACTIVE"
        },
        "2F4F6GpdtMZ": {
            "grantedRoles": [
                "projectMember"
            ],
            "email": "bob@gmail.com",
            "status": "INVITED"
        }
    },
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

By default, API returns only active WS users. If you want to see all users, use "--with-deactivated" flag:

.. code-block:: json

 {
    "channel": "/",
    "command": ["ro:listUsers", "--with-deactivated"]
 }

The response will be:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "users",
    "result": {
        "2F4CHZHUjVD": {
            "grantedRoles": [
                "projectAdmin",
                "projectMember"
            ],
            "firstName": "Alice",
            "lastName": "Alison",
            "email": "alice@gmail.com",
            "status": "ACTIVE"
        },
        "2F4F6GpdtMZ": {
            "grantedRoles": [
                "projectMember"
            ],
            "firstName": "Bob",
            "lastName": "Bobson",
            "email": "bob@gmail.com",
            "status": "DEACTIVATED"
        }
    },
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

/archive
--------

This operation will archive the channel. But first of all, the bot needs to subscribe to the channel.

Payload example:

.. code-block:: json

 {
    "channel": "/Miscellaneous",
    "command": "/archive"
 }

And the successful response will be:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "Channel archived",
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

/unarchive
--------

This operation will unarchive the channel. The bot needs to be subscribed to the channel first of all.

Payload example:

.. code-block:: json

 {
    "channel": "/Miscellaneous",
    "command": "/unarchive"
 }

And the successful response will be:

.. code-block:: json

 {
    "messageType": "WAPI_EXECUTED_CLIENT_MESSAGE",
    "message": "Channel unarchived",
    "status": "success",
    "correlationId": "LVRC:123321:1"
 }

