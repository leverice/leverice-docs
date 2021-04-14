## Built with You in Mind

Unlike messaging tools that only provide a REST API and webhooks,  Leverice is a platform. This means you can write  Apps that run right in the Leverice  workspace. No hosting, no REST APIs, no JSON. Instead: tight integration with our core, ability to fully concentrate on your logic, and nearly endless possibilities.

![dev](https://leverice.com/wp-content/uploads/2020/07/img-platform.svg)

## Equal Opportunity  is Our Core Value

As a Leverice Apps developer, you have almost all the possibilities our core developers have. We keep the Leverice core tiny and implement most of the functionality in what we call  “Default application”. When you create an empty workspace, it is just a Leverice microkernel with the “Default application” deployed in it.

Every time we  design a new feature  for Leverice, we ask ourselves:  
“What if I were an external developer, would I be able to add this feature as a part of my App?”.

And if the answer is No, this means that either there is a very compelling reason for this (usually, security concern) or the design is wrong, and the pull request is not getting approved. In most cases, the answer is YES, because we start developing all new features in our Default application, and if it turns out to be impossible to implement the feature in an App, we expand our microkernel to add necessary support.

So, as a Leverice Application Developer, your App can do all this:
* Create new channels (and even channel types)
* Invite and exclude channel members
* Define new rights and roles
* Send posts (also with control markup - buttons, fields, etc)
* Send REST requests to the outside world
* Accept incoming REST requests

...and much much more

## Bind Your Commands  to UI

All this  functionality  you define  in your slash-commands, which users can call directly. Even cooler: you can bind your slash commands to various menu items, buttons in posts, dialog window controls, and many other places. Dialog windows: yes, you can show custom dialogs (defined by you statically or even dynamically at runtime).

![dev](https://leverice.com/wp-content/uploads/2020/07/img-UI.svg)

## Transactional Stores

You can use our  transactional stores to save the data  you may need later. Every object in Leverice (User, Channel, Workspace, etc.) has its persistent context, available to Leverice Application Developers. You can control whether you store to the private context (visible just to your application), protected context (visible to all applications, but modifiable only by you) or public context (visible to everybody, and modifiable by everybody). This ensures the right balance between flexibility, interoperability, and high security at the same time.

And yes: stores are transactional. The transaction starts when our core receives a command and commits only if all logic worked without errors. So you don’t need to worry about  data consistency – it is granted.

Have fun along the way

## Let Us  Get You Started

All you need to know is basics of Apache Groovy  – a straightforward scripting language (actually, Java is just a subset of Groovy, so Java knowledge is usually enough).

Beyond documentation, we provide Leverice developers with the API library, so if you develop with a popular IDE (like IntelliJ IDEA, for example), you’ll always be able to figure out what methods are available on users, channels, workspace, etc. by just hitting  **Ctrl+Space**.

![img](https://leverice.com/wp-content/uploads/2020/07/img-code.png.webp)

Just to give you a sneak preview: this is how an extended Hello World example looks in Leverice:

``` groovy  
def  greet(name) {  
  currentChannel.post("Hello, $name!");  
}  
```

Once deployed, you can invoke it by entering the slash command

![dev](https://leverice.com/wp-content/uploads/2020/07/img-GreetWorld.svg)

It will send a post “Hello, World!” to the current channel.

A somewhat more impressive example that features persistent channel storage would be:

``` groovy 
// define a persistent context structure  
class  Vars {  
    String lastGreeted;  
}  

// define a new slash command "greet" accepting one argument "name"  
def  greet(name) {  
  assert  name:  "Please, specify the name as an argument to /greet"  
  // post the greeting  
  currentChannel.post("Hello, $name!");  
  // save the name value for future use  
  currentChannel.privateVars(Vars).lastGreeted = name;  
} 
 
// define a new slash command "greetAgain" with no arguments  
def  greetAgain() {  
  // retrieve the lastGreeted value  
  def toBeGreeted =  
     currentChannel.privateVars(Vars).lastGreeted;  
  // assert it is not empty  
  assert toBeGreeted:  "Call /greet someName first!"  
  // greet that name again. Print current channel name as a part of greeting  
  currentChannel.post("Hello again in $currentChannel.name, $toBeGreeted!");  
}  
```

Want to see more? –  [continue with our tutorial.](https://docs.leverice.com/)

## Deployment

Once you  get your Leverice Developer ID, you will be able to build your App with Maven or Gradle and deploy your application to Leverice by just drag-and-dropping the resulting artifact to the deployment channel in your workspace – App gets deployed and ready to be used in a few seconds. So the code-test cycle is almost instant.

![dev](https://leverice.com/wp-content/uploads/2020/07/img-Deplyment.svg)

![background](https://leverice.com/wp-content/uploads/2020/07/img-Kafka.svg)

## What we need from you?
* Build awesome bi-directional integrations with third party tools
* Create cool automation apps for specific Use Cases, inspired by our current App Store
* Bring to life clever bots, to make life even more simpler
* Got other ideas - we are game!

## Under the Hood

Leverice is built on  Apache Kafka architecture, which provides us with almost unlimited scalability and fault tolerance.

**Read more on**  [docs.leverice.com](https://docs.leverice.com/)
