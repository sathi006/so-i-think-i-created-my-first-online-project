Q 1. Types of Binding
Ans: Using the curly braces ({}) syntax
Using ActionScript expressions in curly braces
Using the tag in MXML
Using bindings in ActionScript(BindingUtils)

Q 2. How to create your own event
Ans: Creating a subclass from the Event class
Using the Event metadata tag
Dispatching an event

Q 3. Event Bubbling
Ans: The mechanism through which event objects are passed from the objects that generates an event up through the containership hierarchy

Q 4. Life cycle of Flex Application/Component?
Ans: Preinitialize: The application has been instantiated but has not yet created any child components.
Initialize: The application has created child components but has not yet laid out those components.
creationComplete: The application has been completely instantiated and has laid out all components

Q 5. How you implement MVC in your Application
Ans: Cairngorm is based on the MVC model. It is specifically designed to facilitate complex state and data synchronization between the client and the server, while keeping the programming of the View layer detached from the data implementation.
The role of the View layer in a Cairngorm application is to throw events and bind to data stored in the Model. Components on the View can bind to Value Objects or other properties in the Model (data) layer.
In a Cairngorm Model, related data are stored in Value Objects (VOs), while simple variables can be stored as direct properties of the ModelLocator class. A static reference to the ModelLocator singleton instance is used by the View layers to locate the required data.
The Controller is the most sophisticated part of the Cairngorm architecture. The Controller layer is implemented as a singleton FrontController. The FrontController instance, which receives every View-generated event, dispatches the events to the assigned Command class based on the event's declared type.
The Command class then processes the event by running the Command class' execute() method, which is an ICommand interface method. The event object may include additional data if required by the developer. The execute() method can update the central Model, as well as invoke a Service class which typically involves communication with a remote server. The IResponder interface, which is also implemented by the Command class, includes onResult and onFault methods to handle responses returned from the invoked remote service.

Q 6. Difference btw Java and Flex Getters Setters
Ans: When it comes to getters and setters, Java and AS are quite different, in that getters and setters are part of the core ECMAScript language, whereas in Java, getters and setters are done through a naming convention.
In Java, it is almost never a good idea to make member variables public. If you do decide to make member variables public and then later want to change the interface to use getter/setter functions, you will have to modify all callers of your interfaces, which is onerous at best and in many cases, not possible (expecially when you are creating code that is used by other people).
Meanwhile, in ECMAScript, the externally visible interface doesn’t change when I go from a member variable to a getter/setter and back again. In some sense, the interface hiding is already accomplished in the language. Creating public member variables is “safe” in this sense.
Perhaps this is already obvious to all the AS-heads out there, but it took me a bit of time to get used to the concept.

Q 7. How many events are fired when your focus goes in one text box, you enter some text and then press tab.
Ans: PreinitializeHandler(), initializeHandler(), itemEditBegin, itemEditEnd, creationComplete()

Q 8. How you use styles different ways of using Style sheet
Ans: Using external style sheets, Using local style definitions, Using the StyleManager class ,Using the setStyle() and getStyle() methods, Using inline stylesLoading style sheets at run time

Q 9. How can you use two Styles at the same time
Ans: Using external style sheets and use Inline style commands

Q 10. Try to remember properties of few imp components
Ans:
< id="WeatherService" wsdl="http:/example.com/ws/WeatherService?wsdl" useproxy="false">
< !-- Bind the value of the ZIP code entered in the TextInput control to the ZipCode parameter of the GetWeather operation. -->
< name="GetWeather">

<>{zip.text}
< /mx:request>
< /mx:operation>


Q 11. What is the difference between Flex 2.0 and Flex 3.0
Ans: Enhanced Features like Faster compilation time, SWF file size reduction, Flex/Ajax bridge, Advanced Datagrid, Interactive debugging, Cross-Domain, Versionable, Easy to Use,Security and Code Signing,Failover and Hosting,Cross-Domain RSL, Advanced DatagridDeep Linking, Resource Bundles and Runtime Localization, Flex Component Kit for Flash CS3, Compilation, Language IntelligenceRefactoring, Class Outline,Code Search, Profiler, Module Support, Multiple SDK Support, Skin Importer, Design View Zoom/Pan,Design Mode support for ItemRenderers, Advanced Constraints, CS3 Suite integration, CSS Outline, CSS Design View, Flex 3 SDK Skinning/Style Enhancements

Q 12. How will you call Java method from Flex?
Ans: Using RemoteObject. Explain the process to interviewer

Q 13. What are the config files used for connecting Java and Flex?
Ans:
data-management-config.xml,
messaging-config.xml,
proxy-config.xml,
remoting-config.xml,
services-config.xml

Q 14. What are the channels and their types
Ans: The Channel class is the base message channel class that all channels in the messaging system must extend.
Channels are specific protocol-based conduits for messages sent between MessageAgents and remote destinations. Preconfigured channels are obtained within the framework using the ServerConfig.getChannel() method. You can create a Channel directly using the new operator and add it to a ChannelSet directly
In Flex AMFChannel is used mostly. Action Message Format
Methods
applySettings (),connect(),connectFailed(),connectSuccess(), connectTimeoutHandler()
disconnect(),disconnectFailed(),disconnectSuccess(),flexClientWaitHandler(), getMessageResponder(),internalConnect(),internalDisconnect(),internalSend(),logout()
send(),setCredentials()
Properties
authenticated,channelSets,connected,connectTimeout,endpoint,failoverURIs,protocol,
reconnecting,recordMessageSizes,recordMessageTimes,requestTimeout,uri

Q 15. Give the name of Collection which can be mapped to java and Flex and vice-versa
Ans: java.lang.String String
java.lang.Boolean, boolean Boolean
java.lang.Integer, int int
java.lang.Short, short int
java.lang.Byte, byte[.md](.md) int
java.lang.Byte[.md](.md) flash.utils.ByteArray
java.lang.Double, double Number
java.lang.Long, long Number
java.lang.Float, float Number
java.lang.Character, char String
java.lang.Character[.md](.md), char[.md](.md) String
java. math.BigInteger String
java.math.BigDecimal String
java.util.Calendar Date
java.util.Date Date
java.util.Collection mx.collections.ArrayCollection(for example, java.util.ArrayList)java.lang.Object[.md](.md) Arrayjava.util.Map Object (untyped). For example, a java.util.Map[.md](.md) is converted to an array (of objects).
java.util.Dictionary Object (untyped)
org.w3c.dom.Document XML object
java.lang.Object (other than previously listed types) Typed Object
Objects are serialized by using JavaBean introspection rules and also include public fields. Fields that are static, transient, or nonpublic, as well as bean properties that are nonpublic or static, are excluded.

Q 16. How can you call JavaScript from MXML
Ans: IExternalInterface.call()

Q 17. How can you access a var defined in 1 MXML flex in to another MXML file
Ans: Create 1 object of MXML fiel into another MXML File

Q 18. Is it possible to make httpService Requests synchronous?
Ansvar mytoken:AsyncToken = yourservice.send();
mytoken.addResponder(new ItemResponder(function,errorFunction));
OR
You can create a result handler to your HTTPService.
remoteObjectName.Your method
name.addEventListener("result",HandlerFunction,false,0,true);

Q 19. I need to load an image from flickr into my application. Do I need a crossdomain.xml file on flickr?
Ans: every SWF file you view runs locally on your machine. This means that a SWF would have HTTP access to all machines behind the company firewall. To prevent this, every server other than the one the SWF is loaded from, needs to have a crossdomain.xml file in its root, listing all domains that have access to that particular server

Q 20. What is the difference between httpService and Data Service?
Basically, Flex allows three types of RPC services: HttpService, WebServices, and RemoteObject Services. In Flex, using the “RemoteObjects specifies named or unnamed sources and connects to an Action Message Format (AMF) gateway, whereas using the HTTPService and WebService use named services or raw URLs and connect to an HTTP proxy using text-based query parameters or XML”. Specifically, HTTPServices use raw HTTP requests, WebServices use the SOAP protocol and RemoteObjects uses AMF3.

Q 21. How do you generate random numbers within a given limit with actionscript?
Ans:var randNum:Number = Math.random()**100;**

Q 22. Have you built any components with actionscript? If so explain how you did it?
Ans:
package myComponents{
import mx.controls.Button;
public class MyButton extends Button {
public function MyButton() {
super();
label="Submit";}}}

Q 23. How do you implement push on a flex applications?
Ans
Messaging systems let separate applications communicate asynchronously as peers by passing packets of data called messages back and forth through a Message Service. A message usually consists of a header and a body. The header contains an identifier and routing information. The body contains application data.

So, you will be building an application that allows you to asynchronously send data through the DS message service to our Flex client application. Here are some key DS messaging terms:

Producer: Producers are applications that create/send messages to the destination.
Consumer: Consumers are applications that receive messages from the destination.
Message Destination: Destinations are the resources used for both publish-subscribe and point-to-point messaging.
Message Channel: The channel is the method for connecting producers and consumers to the destination (using an endpoint).
Message Endpoint: An endpoint is the interface responsible for encoding and decoding data into messages.
Message Adaptor: The adaptor defines the messaging implementation. Options include using the ActionScriptAdapter provided with DS, or an external Java Message Service (JMS) provider.

Q 24. I am going to add images into a tag. How will it resize itself?

Q 25. What is a resource Manager??

Q 26. What are the similarities between java and flex?

Q 27. What is the dynamic keyword used for?

Q 28. How do you implement push with flex data services?

Q 29. What are the methods called when a UI component is intialized?

Q 30. How do you implement drag and drop on components that do not support ondrag and ondrop?

Q 31. Can you write to the file system from flex?

Q 32. What is a drag manager?
Ans: Manages the drag-and-drop operations; for example, its doDrag() method starts the drag operation.

Q 33. How do you call javascript from Flex?
Ans:flash.external.ExternalInterface.call(function_name: String[, arg1, ...]):Object;

Q 34. How do you use a repeater?
Ans:
< id="rp" dataprovider="{dp}">
< height="49" width="50" click="Alert.show(String (event.currentTarget.getRepeaterItem()) + ' pressed')" label="{String (rp.currentItem)}">
< /mx:repeater>

Q 35. What are three ways to skin a component in flex?
Ans: Graphical skins: Images that define the appearance of the skin. These images can JPEG, GIF, or PNG files, or they can be symbols embedded in SWF files. Typically you use drawing software such as Adobe® PhotoShop® or Adobe® Illustrator® to create graphical skins.
Programmatic skins: ActionScript or MXML classes that define a skin. To change the appearance of controls that use programmatic skins, you edit an ActionScript or MXML file. You can use a single class to define multiple skins.
Stateful skins: A type of programmatic skin that uses view states, where each view state corresponds to a state of the component.The definition of the view state controls the look of the skin. Since you can have multiple view states in a component, you can use a single component to define multiple skins.

Q 36. How do you use css styles in flex?
Ans: Using external style sheets, Using local style definitions, Using the StyleManager class ,Using the setStyle() and getStyle() methods, Using inline stylesLoading style sheets at run time.

Q 37. What is the difference between sealed class and dynamic classes?
Ans: Sealed Classes: ActionScript 3.0 introduces the concept of sealed classes. A sealed class possesses only the fixed set of properties and methods that were defined at compile time; additional properties and methods cannot be added. This enables stricter compile-time checking, resulting in more robust programs. It also improves memory usage by not requiring an internal hash table for each object instance. Dynamic classes are also possible using the dynamic keyword. All classes in ActionScript 3.0 are sealed by default, but can be declared to be dynamic with the dynamic keyword.
Dynamic classes A dynamic class defines an object that can be altered at run time by adding or changing properties and methods. A class that is not dynamic, such as the String class, is a sealed class. You cannot add properties or methods to a sealed class at run time.
dynamic class Protean
{ //Use dynamic keyword before the name of class
}

Q 38. What is e4X and XML?
Ans: E4X means "ECMAScript For XML
Using E4X, we can develop code with XML data faster than was possible with previous programming techniques. E4X provides a set of classes and functionality for working with XML data. ActionScript 3.0 includes the following E4X classes: XML, XMLList, QName, and Namespace.
Here is an example of manipulating data with E4X:
var myXML:XML =
<>
< id="'1'">
<>burger
<>3.95
< /item>
< id="'2'">
<>fries
<>1.45
< /item>
< /order>
trace (myXML.item[0].menuName); // Output: burger
trace (myXML.item.(@id==2).menuName); // Output: fries
trace (myXML.item.(menuName=="burger").price); // Output: 3.95

Q 39. What is state? What is the difference between states and ViewStack?
Ans:View Stack is to handle different MXML file eg TAB control, and states is the transition within single MXML file

Q 40. How does item renderer work? How do I add item renderer at runtime?

Q 41.. What keyword allows you to refer to private variables of a class?

Q 42. How polymorphism works on actionscript?

Q 43.. How do you overload functions in actionscript?

Q 44. What is dynamic keyword used for?
Ans: Those Classes which should be extended and changed are called Dynamic classes

Q 45. What are sealed classes?
Ans: Classes whichyou cant extend. ex String

Q 46. What are runtime shared libraries?

Q 47. What is caringhorm? How do you use it? Have you worked with Cairngorms?

Q 48. What keyword allows you to implement abstraction better?

Q 49. What design patterns have you used? In Actionscript and java?

Q 50. What's the difference between Java and AS3 getters and setters? Ans: I have to explicitly call a setter/getter function in Java, while AS3 allows me to Access my setters and getters as though they are variables.
Q 51. Explain the component lifecycle.
Q 52. Tell me about dataServices
Q 53. understanding of MVC
Q 54. They ask what frameworks you aree familiar with When getting developers to help create "standard" application, I'd weight a lot more heavily on the as3 questions. Aside from basic "show me how you'd write X" algorithm questions, you can ask more in depth ones about the workings of the language:
Q 55. Explain how binding works in mxml components.

Q 56. Explain the ChangeWatcher.Wach()
Ans: You can detect when a binding occurs in your application. Flex includes the mx.binding.utils.ChangeWatcher class that you can use to define a watcher for a data binding. Typically, your event watcher invokes an event listener when the binding occurs

Q 57. Why would you want to keep a reference to a ChangeWatcher and call unwatch()?
Q 58. How do you add event listeners in mxml components. Now AS3 components?
Q 59. What does calling preventDefault() on an event do? How is this enforced?
Ans: Prevent defaults helps skipping the default behaviour or any componnent. Like in radio Button when you will click it will get selected, but if you want only the skin to be changed you can use preventDefault()

Q 60. (If applicable) Explain the lifecycle of a Cairngorm action.

Q 61. I was interviewed for a Flex position. The hiring manager kept on asking Java and J2EE questions. So, I asked him politely: you are hiring me for a flex position, why do you keep on asking Java questions?
Q 62. What are some ways to specify styles on components?

Q 63. What is the problem with calling setStyle()
Ans: When you are instantiating an object and setting the styles for the first time, you should try to apply style sheets rather than use the setStyle() method because it is computationally expensive. This method should only be used when you are changing an object's styles during run time
Q 64. Explain the difference between creating an effect and setting the target as opposed to adding an effectListener
Q 65. What do repeater components do?
Q 66. How do you identify a component created in a repeater?

Q 67. What is state? What is the difference between states and ViewStack? Ans: View Stack is to handle different MXML file eg TAB control.And states is the transition within single MXML file ViewStack should be used where there is complete change in the controls used and States should be used when you just want to add or remove afew components based on certain conditions. Login/Registration/Forgot password is the best example for using States as each page will either add or remove to the already existing one.

Q 68. What is the difference between httpService and Data Service? Ans. The Flex presentation layer communicates with the business layer by using Flex data services, which are objects you insert in a Flex file. Specifically, you can use Flex data services to interact with the following:
* Web services
* HTTP services
* Remote objects
A Flex data service is an object you insert in an MXML file to communicate with the business layer of a multi-tier application. You use data services to send and receive data from web services, HTTP URLs, and remote objects such as server-based Java objects. An HTTP service is nothing more than an HTTP request to a URL. The primary purpose of HTTP services is to retrieve XML data from an external source.

Q 69. Why you don’t Embed all images in your application
Ans: That increases the size of the SWF File

Q 70. What type of images can be loaded
Ans: (PNG, GIF, JPEG, SWF at run time and PNG, GIF, JPEG, SWF, SVG at compile time)

Q 71. Explain ContainerCreationPolicy
Ans: This is used with the containers like Accordian, ViewStack etc. depending upon the ContainerCreationPolicy you have set accordingly the child pages will be loaded.
ALL Immediately create all descendants.
AUTO: Delay creating some or all descendants until they are needed.
NONE: Do not create any children.
QUEUED: Add the container to a creation queue.

Q 72. What are the factory classes
Ans: Factory class The factory class creates an object of the instance class to perform the effect on the target. You create a factory class instance in your application, and configure it with the necessary properties to control the effect, such as the zoom size or effect duration.

Q 73. Explain Metadata
Ans: You insert metadata tags into your MXML and ActionScript files to provide information to the Adobe Flex compiler. Metadata tags do not get compiled into executable code, but provide information to control how portions of your code get compiled.

Q 74. I was asked too many questions on the event bubbling, Like how to capture and event if the component is disabled.
Ans: addEventListner on the parents and enable the component in that handler

Q 75. obj.addEventListener(MouseEvent.CLICK, MouseClickHAndler); in this obj should inherit which class?
Ans: DispachEvent
You can manually dispatch events using a component instance's dispatchEvent() method. All components that extend UIComponent have this method
objectInstance.dispatchEvent(event:Event):Boolean

Q 76.Why we extent Sprite Class in our Graphical Classes?

Q 77.What is Layout Manager and explain the Properties and Methods?
AnsThe LayoutManager is the engine behind Flex's measurement and layout strategy. Layout is performed in three phases; commit, measurement, and layout.
The commit phase begins with a call to validateProperties(), which walks through a list (sorted by nesting level) of objects calling each object's validateProperties()method.

The objects in the list are processed by nesting order, with the most deeply nested object accessed first. This can also be referred to as bottom-up inside-out ordering.

The measurement phase begins with a call to validateSize(), which walks through a list (sorted by nesting level) of objects calling each object's validateSize() method to determine if the object has changed in size.

The layout phase begins with a call to the validateDisplayList() method, which walks through a list (reverse sorted by nesting level) of objects calling each object's validateDisplayList() method to request the object to size and position all components contained within it (i.e. its children).

Property of LayoutManager
usePhasedInstantiation : Boolean 
A flag that indicates whether the LayoutManager allows screen updates between phases.

Q 78.How do you call a method in particular ItemRenderer. Also the ItemRenderer is your own Custom Component.
Ans
<>
< id="comboBox" dataprovider="{statesXMLList}" labelfield="@name" itemrenderer="ComboBoxItemRenderer">
< /mx:Application>
View ComboBoxItemRenderer.mxml
< ?xml version="1.0" encoding="utf-8"?>

< mx="http://www.adobe.com/2006/mxml" stylename="plain">
< text="{data.@name}" onclick="MyOwnMethodThatIWantToCall()">
< /mx:VBox>

Q 79.How can you implement Singleton in Flex Application?
Ans:If you are a Java Programmer dont be mistaken by saying create Private Constructor. We cant create private Constructor in Flex.

Q 80.Give similiarities btw Java and Flex
Ans: Both are object Oriented, Encapsulation, Inheritance, Abstraction and Polymorphism are implemeted in both of the technologies.
*The package like imports
*Availability of Classes in the scripting language
*Capabilities Arrays & ArrayCollections
*On the UI end, similarities to SWING```