First application for Leverice: step by step
============================================

In this tutorial we will create and deploy application for Leverice. Make sure that you have Leverice Developer ID and access to developer installation. See :ref:`introduction-label` for details

Prerequisites
#############

You should have basic understanding of json files structure, maven and groovy. Nice to have: basics of object-oriented programming (OOP)

Create structure of project
###########################

TODO: Replace this part with guide Leverice Application Initializer

Folder structure
----------------

In your root folder you should create folders:

* src/main/groovy - placement of groovy-files
* src/main/resources - placement of resources like configs, assets, etc

After that you should have folder structure like below:

* root-folder
    * src
        * main
            * groovy
            * resources

POM-file
--------

It is an XML file that resides in the base directory of the project as pom.xml. The POM contains information about the project and various configuration detail used by Maven to build the project(s). For detailed information about pom-file please refer to `official documentation <https://maven.apache.org/guides/introduction/introduction-to-the-pom.html>`_. For our app create file **pom.xml** and paste content below:

.. code-block:: xml

 <?xml version="1.0" encoding="UTF-8"?>
 <project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>net.leverice.hackathon</groupId>
        <artifactId>app-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>leverice-simple-app</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Simple Leverive Application</name>

    <repositories>
        <repository>
            <id>hackathon-releases</id>
            <name>Leverice Hackathon Maven Release Repository</name>
            <url>https://hackathon.leverice.net/repository/maven-releases/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>hackathon-snapshots</id>
            <name>Leverice Hackathon Maven Snapshot Repository</name>
            <url>https://hackathon.leverice.net/repository/maven-snapshots/</url>
            <releases>
                <enabled>false</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
 </project>

After adding pom file your project structure should looks like:

* root-folder
    * src
        * main
            * groovy
            * resources
    * **pom.xml**

TODO: should i clarify some fields in pom-file?

Application descriptor
----------------------

It is a file **descriptor.json** which should be placed in **src/main/resources** and contains configuration of our application. Minimal configuration file listed below:

.. code-block:: json

 {
  "id": "simpleApp",
  "version": 0,
  "facets": [
    {
      "id": "simpleAppFacetId",
      "name": "Simple App Facet Name",
      "requires": [
        "default.default"
      ],
      "iconName": "plugin:default:ChannelType_Public@24px.svg"
    }
  ]
 }


and contains following fields:

* id - identifier of our application. Must be unique in workspace scope
* version - should start with 0. Needed for upgrading functionality
* facets and iconName leave as is, we will explain them later

After adding this file in project structure should looks like:

* root-folder
    * src
        * main
            * groovy
            * resources
                * **descriptor.json**
    * pom.xml

Build and Deploy application
############################

Further in this documents we will show how every change affects our application in Leverice, so we need to know, how to build and deploy it

Build
-----

To build our application you should run following command from root of the project:

.. code-block:: bash

 mvn clean install

This command do following things:

* removes previously generated output in **target** folder
* copies resources in **target** folder
* generates zip-archive with our application in **target** folder in format **${artifactId}_${version}.zip**. In our case it is **leverice-simple-app_0.0.1-SNAPSHOT.zip**. This archive needed for `Deploy`_

Deploy
------

First of all, to deploy your application to Leverice you need generated archive after `Build`_. After that log in your workspace in Leverice and do following things, described in further sections

Create channel for uploading application
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This is action should be executes once on current workspace. To save your time just copy and paste following script into message box and send it:

.. code-block:: bash

 /createChannel -position.parentChannelId 11111111111 -name Development -sortChildrenBy crtd -channel-type default.team
 /createChannel -position.parentChannelId /Development -name Deployment -sortChildrenBy crtd -channel-type default.public -additional-facet default.appdev
 /createChannel -position.parentChannelId /Development -name Debug -sortChildrenBy crtd -channel-type default.public -additional-facet default.debug
 /cd /Development/Deployment

This script creates folder "Development" and channel "Deployment" and switch to it.

To deploy your application you should drag and drop or select as attachment your zip file, which created after `Build`_ and send it as usual messages. In successful case you will see message like
 App '${yourAppId}' loaded to the workspace.

In our case we will see message: *App 'simpleApp' loaded to the workspace.*

Our first commands
##################

During previous steps we built and deployed our first app to your workspace, but it does not provide any functionality. Let's fix it.

Create file **global.groovy** under directory "src/main/groovy". After creation your project structure should look like:

* root-folder
    * src
        * main
            * groovy
                * **global.groovy**
            * resources
                * descriptor.json
    * pom.xml

Then put into it following code:

.. code-block:: groovy

 def greet() {
     currentChannel.post "Hello, world!"
 }

Name of the method (**greet** in our case) define command, which we can use after `Build`_ and `Deploy`_ our application. **currentChannel** is special object which related to current visible channel for user, who executes command. **currentChannel** has various method, but in our case we need only *post* to send message in current channel using command **greet**. Let's `Build`_ and `Deploy`_ to check.

After deployment send command **/greet** to any channel:

.. code-block:: bash

 /greet

And you will see message:

 Hello, world!

Now let's modify our method to take one argument:

.. code-block:: groovy

 def greet(name) {
     currentChannel.post "Hello, ${name}!"
 }

**name** in this case is unnamed argument, which should be passed after command. `Build`_ and `Deploy`_ and send following command in any channel:

.. code-block:: bash

 /greet "My friend"

You will see message *"Hello, my friend!"*. You can change argument in this command and check, that it still works

Follow our further guides to deeper knowledge of possibilities for programming in Leverice.
