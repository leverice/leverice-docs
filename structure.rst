Architecture
===================

Apps
########
Apps in Leverice are sometimes called "plugins", but please, do not mix this with other messengers' plugins, as our plugins are much more powerful and should be rather seen as apps.
App is a set of artifacts (assets, channel type descriptors, facet descriptors as well as the groovy scripts, that contain actual logic) that belong together and present a complete solution to some business problem.

Facets: Definition
########################
Let's talk about facets. Facet is a piece of configuration and code that provides some specific behaviour of a channel it is attached to.
Think of facet as a java interface (however, groovy trait is more correct example, as together with logic, your facet may contain some state attached to the channel, user, workspace and so on).

You can add facets to any channel and get that additional functionality/behavior in that channel.

Facets are added by calling a Leverice command "/addFacet <facetId>".

Facet id is composed of two parts: app id and facet id, separated by dot. For example: "default.ontop", "default.noedit" or "trello.task".
Application id (first part before dot) acts like a namespace to the facet. So, facet abc.someFacet is not the same as bcd.someFacet.

There is a number of standard default facets in Leverice, that you can try to use straight away. Let's play around with them.

Create a workspace called Leverice Tutorial, then create a folder "Facets Sandbox" on a top level. Under it, create three public channels: "Channel 1", "Channel 2", "Channel 3".

Once you've done this, it should look like this in your Leverice:

* Leverice Tutorial
   * Facets Sandbox
      * Channel 3
      * Channel 2
      * Channel 1

Go to Channel 1 and post following message to the chat:

.. code-block:: bash

   /addFacet default.ontop

This should bring your Channel 1 to the top among children. You can try the following facets to explore a bit:

.. code-block:: bash

   default.noWrite
   default.noleave
   default.noArchive
   default.initMuted

And see how it changes the channel behavior

Facets: Custom Application Facets
#######################################

There is plenty of facets defined by Leverice (above you saw some examples), however, power of Leverice is more about extendability. As a Leverice Developer, you
can create your own facets. Once you create your custom application facet, it will let you to:

* define and set up new commands that can be either invoked by user or bound to some controls in Leverice
* change channel look and feel (for example, add custom icon to the channel)
* add new channel context menu items and bind them to commands
* add new post context menu items and bind them to commands
and many more...

You can set up facet in the app descriptor. It looks like this.

.. code-block:: javascript

 {
   "id": "exampleAppId",
   "version": 1,
   "facets": [
     {
       "id": "exampleFacetId",
       "name": "Example",
       "requires": [
         "default.default",
         "default.whiteboard",
         "default.noWrite"
       ],
       "iconName": "plugin:exampleAppId:example.svg",
     }
   ]
 }


Assets
########
You can add assets to your app. It should be .svg image.
It can be added as the channel icon in the facet descriptor's field "iconName".
Use **"plugin:${appId}:${assetName}.svg"** to add it.

Channel Type
##############
Channel Type is a specific channel type descriptor. When facets are being used to extend channel functionality channel types
are being used for defining of the set of facets for newly created channel. We should add channel type descriptor to create new channel type.
Channel type descriptor defines:

* channel icon
* name of this channel type in the "new channel" menu
* set of facets to add to the created channel with this channel type
* fields visibility and default values of the inline and full "create channel" windows
* restricts channels to create channel with this channel type under
* restricts moving of the channel with this channel type under parents with defined facets or channel types

.. code-block:: javascript

 {
   "id": "exampleAppId.exampleChannelTypeId",
   "name": "Example Type",
   "iconName": "plugin:exampleAppId:example.svg",
   "facets": [
     "exampleAppId.exampleFacetId",
     "default.whiteboard",
     "default.default"
   ],
   "rank": 53000,
   "createDialogDescriptor": {
     "dialogWindowPopupDescriptors": {
       "inline": {...},
       "full": {...}
     },
     "messageType": "CREATE_CHANNEL_ATTACHMENT"
   },
   "canBeCreatedUnderChannelsTypes": [
     "default.private",
     "default.project"
   ],
   "canBeMovedUnderChannelsTypes": [
     "default.private",
     "default.project"
   ],
   "canBeCreatedUnderParentsWithFacets": [],
   "canBeMovedUnderParentsWithFacets": [],
   "canBeCreatedUnderFacets": [],
   "canBeMovedUnderFacets": []
 }

Command
##############
All users' interactions with the channel and the workspace are commands. You want to create new channel? Run command.
UI provide us possibility to run complicated commands in convenient way, but all of this buttons run commands. Let's speak about it.

Command parts:

* command name (post)
* parameters

Parameter types:

* option (-m "message to send")
* flag (--pn)
* arg (one two three)

Examples:

* /post -m "message to send" (command with option)
* /set a b (command with args)
* /post -m "message to send" --pn --p (command with option and flags)

Command parameters order:

#. args
#. options
#. flags

Groovy Scripts
###############
Groovy scripts is a file with actual business logic. You can create new or extends existing commands here.
You can run any existing command like it was called from the frontend client or/and use our internal api to do more specific things.

.. code-block:: groovy

 def greet(name) {
  sendPost().messageBody("Hello, ${name}!").submit();
 }

This code defines command "greet" that has one parameter "name". This command sends post to the current channel and greets somebody from the "name" parameter.
Imagine this command added to the "greet" facet in the "polite" app. How to call it in the Leverice

#. add facet to the channel (send "/addFacet polite.greet" w/o quotes as a common message in the Leverice workspace)
#. run greet command in this channel (send "/greet -name John" w/o quotes as a common message in the Leverice workspace)
#. system sends message from you to this channel with the text "Hello, John!"

Until you add this facet to the channel this command won't work.


