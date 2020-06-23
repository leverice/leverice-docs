Architecture
===================

Apps
########
Apps in `Leverice <https://leverice.com/public/client/>`_ are sometimes called "plugins", but please, do not mix this with other messengers' plugins, as our plugins are much more powerful and should be rather seen as apps.
App is a set of artifacts (assets, channel type descriptors, facet descriptors as well as the groovy scripts, that contain actual logic) that belong together and present a complete solution to some business problem.

Facet
########
Any app contains of facets. Any facet extends channel functionality and give possibility to:

* define and set up new commands
* run facet's commands in the channel with this facet
* change channel icon to the facet specific
* restrict to do some actions in the channel with this facet
* define required facets that will be added with this facet to the channel

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
Imagine this command added to the "greet" facet in the "polite" app. How to call it in the `Leverice <https://leverice.com/public/client/>`_

#. add facet to the channel (send "/addFacet polite.greet" w/o quotes as a common message in the `Leverice <https://leverice.com/public/client/>`_ workspace)
#. run greet command in this channel (send "/greet -name John" w/o quotes as a common message in the `Leverice <https://leverice.com/public/client/>`_ workspace)
#. system sends message from you to this channel with the text "Hello, John!"

Until you add this facet to the channel this command won't work.


