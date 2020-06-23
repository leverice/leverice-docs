Simple greet app tutorial
===========================

Let's write simple app 'greet'. This app should contain at least one facet "someone" and one command "greet" in this facet.

Create app structure
#####################

Use maven archetype "leverice-external-app-archetype" to create the maven project with needed structure.

* greet-app
   * pom.xml
   * src
      * main
         * groovy
            * **global.groovy**
            * someone
               * **someone.groovy**
         * resources
            * **descriptor.json**
            * assets
               * **someone-channel-logo.svg**
            * channel
               * **channel-type.json**

descriptor.json
#####################

.. rubric:: /src/main/resources/descriptor.json

We need descriptor for the app that defines facets, app identity and other configuration of the app.

.. code-block:: javascript

 {
   "id": "greet",
   "version": 0,
   "facets": [
     {
       "id": "someone",
       "name": "Greet Someone",
       "requires": [
         "default.default"
       ],
       "iconName": "plugin:greet:someone-channel-logo.svg"
     }
   ]
 }

As you can see we define app with id = "greet" and version = 0.

This app contain one facet with id = "someone". FullFacetId will be "greet.someone"

Facet name is "Greet Someone". It requires "default.default" facet. When we add facet "greet.someone" to the channel facet "default.default" will be added with it.

Channel with this facet will have **someone-channel-logo.svg** icon from our assets folder. Common pattern is: **"plugin:${appId}:${assetFileName}.svg"**

:ref:`app-desc-reference-label`

assets
#####################

.. rubric:: /src/main/resources/assets

We can add .svg icons here to use it in the facet descriptors as channel icons. Use **fill="currentColor"** to inherit channels' tree icon colors.

Example:

.. code-block:: xml

 <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 16 16">
     <g fill="currentColor" fill-rule="evenodd">
         <path fill="#475F7B" d="M12.817 10.922l-.102 2.394c-.03.386-.232.73-.555.943l-2.417 1.598c-.143.095-.307.143-.472.143-.118 0-.236-.024-.348-.074-.142-.063-.725-.66-1.751-1.794 2.14-.749 4.064-1.83 5.645-3.21zM15.014 0c.31 0 .625.01.946.033.017.228.029.453.035.675l.005.374c0 6.762-5.187 10.438-9.474 11.891l-.266.088-3.321-3.317C4.166 5.562 8.01 0 15.014 0zM1.878 8.762l-.019.058C.732 7.8.136 7.218.074 7.077c-.119-.269-.093-.575.069-.82L1.74 3.84c.214-.323.557-.525.943-.555l2.387-.102C3.66 4.765 2.576 6.675 1.878 8.762zm10.204-3.538c-.722 0-1.306-.584-1.306-1.306 0-.721.584-1.306 1.306-1.306.721 0 1.306.585 1.306 1.306 0 .722-.585 1.306-1.306 1.306z"/>
         <path fill="#FFB300" d="M4.482 13.66c.183-.183.48-.183.663 0 .183.183.183.48 0 .663l-1.53 1.53c-.092.092-.212.138-.332.138-.12 0-.24-.046-.331-.138-.183-.183-.183-.48 0-.663zm-2.805-2.805c.183-.183.48-.183.663 0 .183.183.183.48 0 .663l-1.53 1.53c-.092.092-.212.137-.332.137-.12 0-.24-.045-.331-.137-.183-.183-.183-.48 0-.663z"/>
         <path fill="#FA321E" d="M3.743 12.257c-.183-.183-.48-.183-.663 0L.137 15.2c-.183.183-.183.48 0 .663.092.091.212.137.332.137.12 0 .24-.046.331-.137l2.943-2.943c.183-.183.183-.48 0-.663z"/>
     </g>
 </svg>


groovy scripts
#####################

.. rubric:: /src/main/groovy/global.groovy

You can use this file to add command to the all channels in your workspace.

If you want it write further implementation in this file and don't create facet specific groovy files.

For your custom apps you can combine this file with other facet specific groovy files.

.. rubric:: /src/main/groovy/someone/someone.groovy

Use this file to implement the "greet.someone" facet scoped business logic.

File name can differ with facet name and there are can be many files.

But path to the file must match to the pattern "/src/main/groovy/${facetId}/"

.. rubric:: implementation

.. code-block:: groovy

 def greet(name) {
  sendPost().messageBody("Hello, ${name}!").submit();
 }

Just add this to your groovy file to define "/greet" command with string "name" parameter.
We use internal api "sendPost" method here. If we want to call existing "post" command from the command api we should implement it like that

.. code-block:: groovy

 def greet(name) {
  executeCommand(command: "/post -m \"Hello, ${name}!\"");
 }

channel types
##################

.. rubric:: /src/main/resources/channel

This is the place for the channel type descriptors. Skip it for this tutorial.

:ref:`channel-type-reference-label`

build zip archive to deploy
############################

run "mvn clean assembly:single" to build the app. You can find it in the newly created "target" folder.

pom.xml (you can use it to create app w/o maven archetype)

.. code-block:: xml

 <?xml version="1.0" encoding="UTF-8"?>
 <project xmlns="http://maven.apache.org/POM/4.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
     <version>0.0.1-SNAPSHOT</version>

     <properties>
         <groovy-all.version>2.5.6</groovy-all.version>
         <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
         <maven.compiler.source>11</maven.compiler.source>
         <maven.compiler.target>11</maven.compiler.target>
     </properties>

     <artifactId>greet-app</artifactId>

     <name>${project.artifactId}: Tutorial Greeting App</name>
     <groupId>com.example</groupId>
     <packaging>jar</packaging>

     <dependencies>
         <dependency>
             <groupId>org.codehaus.groovy</groupId>
             <artifactId>groovy-all</artifactId>
             <version>${groovy-all.version}</version>
             <type>pom</type>
         </dependency>
     </dependencies>
     <build>
         <plugins>
             <plugin>
                 <artifactId>maven-assembly-plugin</artifactId>
                 <version>3.1.1</version>
                 <configuration>
                     <descriptorRefs>
                         <descriptorRef>src</descriptorRef>
                     </descriptorRefs>
                     <tarLongFileMode>posix</tarLongFileMode>
                 </configuration>
             </plugin>
             <plugin>
                 <groupId>org.codehaus.gmaven</groupId>
                 <artifactId>groovy-maven-plugin</artifactId>
                 <version>2.1.1</version>
             </plugin>
         </plugins>
     </build>
 </project>

Congratulations
############################
You create the greet app for you `Leverice <https://leverice.com/public/client/>`_ workspace. Lets :ref:`deploy-reference-label`!
