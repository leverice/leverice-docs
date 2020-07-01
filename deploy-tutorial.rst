.. _deploy-reference-label:

Deploy an app into the workspace
=================================
You have zip archive with application. We must deploy it to the workspace to use it. Let's do it.

Create release track channel
#############################

Create any public or private channel in your workspace in the Leverice communication platform for your application deploy.

#. For example create the top level folder **"Apps"** and public **"Greet App"** channel under this folder.
#. Run command **"/addFacet default.appdev"** in the **"Greet App"** channel. (send it as post to the channel w/o quotes)

Deploy your app
#############################

Attach your zip archive with an app to the new post and send it. You should receive message that your app was successfully loaded.
Remember that there are can be some log posts from the app install logic or error post.

That's it. Use your app in this workspace and enjoy extended functionality of the Leverice communication platform.

.. warning:: Do not forget add facet from your app to the channel if you define commands for this facet to use it.

