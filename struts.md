What is the difference between Struts 1 vs Struts 2 ?
http://struts.apache.org/2.1.6/docs/comparing-struts-1-and-2.html
Which design pattern the Interceptors in Struts2 is based on ?
Interceptors in Struts2 are based on Intercepting Filters.
What are Pull-MVC and push-MVC based architecture ? Whicharchitecture does Struts2 follow ?
Pull-MVC and Push-MVC are better understood with how the view layer is getting data i.e. Model to render. In case of Push-MVC the data( Model) is constructed and given to the view layer by the Controllers by putting it in the scoped variables like request or session. Typical example is Spring MVC and Struts1. Pull-MVC on the other hand puts the model data typically constructed in Controllers are kept in a common place i.e. in actions, which then gets rendered by view layer. Struts2 is a Pull-MVC based architecture, in which all data is stored in Value Stack and retrieved by view layer for rendering.
Are Interceptors in Struts2 thread safe ?
No,Unlike Struts2 action, Interceptors are shared between requests, so thread issues will come if not taken care of.
Are Interceptors and Filters different ? , If yes then how ?
Apart from the fact that both Interceptors and filters are based on intercepting filter,there are few differences when it comes to Struts2.
Filters: (1)Based on Servlet Specification (2)Executes on the pattern matches on the request.(3) Not configurable method calls
Interceptors: (1)Based on Struts2. (2)Executes for all the request qualifies for a front controller( A Servlet filter ).And can be configured to execute additional interceptor for a particular action execution.(3)Methods in the Interceptors can be configured whether to execute or not by means of excludemethods or includeMethods. ( see tutorial on this Controlling Interceptor Behavior)
In Struts1, the front-controller was a Servlet but in Struts2, it is a filter. What is the possible reason to change it to a filter ?
There are two possibilities why filter is designated as front controller in Strtus2
(1)Servlet made as front controller needs developer to provide a right value in 

&lt;load-on-startup&gt;

 which lets the framework to initialize many important aspects( viz. struts configuration file)as the container starts.In absense of which the framework gets initialized only as the first request hits.Struts2 makes our life easy by providing front-controller as a filter,and by nature the filters in web.xml gets initialized automatically as the container starts.There is no need of such load-on-startup tag.
(2).The second but important one is , the introduction of Interceptors in Struts2 framework.It not just reduce our coding effort,but helps us write any code which we would have used filters for coding and necessary change in the web.xml as opposed to Struts1.So now any code that fits better in Filter can now moved to interceptors( which is more controllable than filters), all configuration can be controlled in struts.xml file, no need to touch the web.xml file.
(3).The front controller being a filter also helps towards the new feature of Struts ie UI Themes. All the static resources in the Themes now served through the filter
Which class is the front-controller in Struts2 ?
The class "org.apache.struts2.dispatcher.FilterDispatcher " is the front controller in Struts2. In recent time Struts 2.1.3 this class is deprecated and new classes are introduced to do the job. Refer:  http://struts.apache.org/2.1.8/struts2-core/apidocs/org/apache/struts2/dispatcher/FilterDispatcher.html
What is the role of Action/ Model ?
Actions in Struts are POJO , is also considered as a Model. The role of Action are to execute business logic or delegate call to business logic by the means of action methods which is mapped to request and contains business data to be used by the view layer by means of setters and getters inside the Action class and finally helps the framework decide which result to render
How does Interceptors help achieve Struts2 a better framework than Struts1 ?
> Most of the trivial work are made easier to achieve for example automatic form population.
> Intelligent configuration and defaults for example you can have struts.xml or annotation based configuration and out of box interceptors can provide facilities that a common web application needs
>Now Struts2 can be used anywhere in desktop applications also, with minimal or no change of existing web application,since actions are now POJO.POJO actions are even easier to unit test.Thanks to interceptors
>Easier UI and validation in form of themes and well known DOJO framework.
>Highly plugable,Integrate other technologies like Spring,Hibernate etc at ease.
> Ready for next generation RESTFUL services
What is the relation between ValueStack and OGNL ?
A ValueStack is a place where all the data related to action and the action itself is stored. OGNL is a mean through which the data in the ValueStack is manipulated.

Struts2 Interview Questions and Answers

Page 2 of 4
Can annotation-based and XML based configuration of actions coexists ?
Yes
What is struts.devMode and why it is used ?
struts.devMode is a key used in struts.properties file (Can also be configured in struts.xml file as 

&lt;constant name="struts.devMode" value="true" /&gt;

) , to represent whether the framework is running in development mode or production mode by setting true or false. If set to development mode, it gives the following benefits : -
> Resource bundle reload on every request; i.e. all localization properties file can be modified and the change will be reflected without restarting the server.
> struts.xml or any configuration  files can be modified without restarting or redeploying the application
> The error occurs in the application will be reported, as oppose to production mode.
Also remember that struts.devMode should be marked as false in production environment to reduce impact of performance. By default it is "false".
How do you configure the annotation based action mapping ?
What is the difference between empty default namespace and root name space ?
If the namespace attribute is not defined in the package tag or assigned "" value then it is called empty default namespace.While if "/" is assigned as value to the namespace attribute then it is called as root namespace.
The root namespace is treated as all other explicit namespaces and must be matched. It’s important to distinguish between the empty default namespace, which can catch all request patterns as long as the action name matches, and the root namespace, which is an actual namespace that must be matched.
Which interceptor is responsible for setting action's JavaBean properties ?
com.opensymphony.xwork2.interceptor.ParametersInterceptor is the interceptor class who sets the action's JavaBean properties from request.
What is the difference between Action and ActionSupport ?
Action is interface defines some string like SUCCESS,ERROR etc  and an execute() method. For convenience Developer implement this interface to have access to String field in action methods. ActionSupport on other hand implements Action and some other interfaces and provides some feature like data validation and localized error messaging  when extended in the action classes by developers.
How do you get the HttpServletRequest object in an interceptor ?
Here is the intercept method
?
1
2
3
4
5
public String intercept(ActionInvocation invoke) throws Exception {
> ActionContext action=invoke.getInvocationContext();
> HttpServletRequest req=(HttpServletRequest)action.get(StrutsStatics.HTTP\_REQUEST);
> return null;
}
In the similar way you can get the response, by using StrutsStatics.HTTP\_RESPONSE in get() method as above.
> What is execute and wait interceptor ?
The ExecuteAndWaitInterceptor is great interceptor provided out of box in Struts2 for running long-lived actions in the background while showing the user a nice progress meter or a progress bar. For example while uploading a large file to the server we can use this interceptor to display a nice running progress bar instead of leaving the user in confusion that the application is not responding.This also prevents the HTTP request from timing out when the action takes more than 5 or 10 minutes.
> Does the order in which interceptors execute matters ? If yes then why ?
Well, the answer is yes and no.Some Interceptors are designed to be independent so the order doesn't matter,but some interceptors are totally dependent on the previous interceptors execution.For example the validation and workflow interceptors,the validation interceptors checks if there is any error in the form being submitted to the action, then comes the workflow interceptor who checks if validation ( occured in the last) has any error,in presence of error it will not let the rest of the interceptors ( if any ) in the stack to execute.So this is very important that the validation interceptors execute first before the workflow. On the other hand lets say you wrote an interceptors who is doing the authentication and you have the user credential provided ( by the time this executes) it doesn't matter where this interceptors is placed( It is a different fact that you would like to keep it in the top ).
Who loads the struts.xml file ? Which Struts2 API loads the struts.xml file?
In Struts2 FilterServlet is the responsible class for loading struts.xml file as we deploy the application on the container.Unlike Servlet (as with Struts1) needs the load-on-startup tag to load the front controller,a filter doesn't need to have load on startup tag to be loaded as the application is deployed. As with servlet specification a filter is loaded/executed as the application  starts up

What is the difference between RequestAware and ServletRequestAware interface ?
RequestAware and ServletRequestAware both makes your action to deals with the servlet request, but in a difffrent ways,RequestAware gives you the attributes in the servlet request as a map( key as attribute name and value is the object added),But ServletRequestAware gives you the  HttpServletRequest object itslef giving you more flexibility, with a price that ServletRequestAware makes your  Action class too much tied to the Servlet environment making it dificult to unit test. So whenever a need to access only the attributes use the RequestAware interface.
What is the difference between EL and OGNL ?
OGNL is much like EL in JSPs,a language to traverse or manupulate objects like request , session or application in web context.OGNL stands for Object graph navigation language,which is used internally by Struts2, however we are not bound to use OGNL in our JSPs, we can use EL.But OGNL provides much more facilities than plain EL.For example while El interacts with the objects by means of getters/setters, OGNL supports whatever EL does along with lambda experssion, helps create functions on fly. OGNL has more flexible ways to deal with collection of objects.
What are the difference between ActionContext and ServletActionContext ?
ActionContext represents the context in which Action is executed, which itself doesn't hold any web parameters like session, request etc. ServletActionContext, extends the ActionContext and provides the web parameters  to the Action.
What is the life cycle of Interceptor ?
Who executes/Which class in Struts2 executes the interceptors ?
In what order the interceptors execute in an interceptor stack ?
What is action scope and how is it different from request scope ?
What happens if you don't call the invoke() method in any interceptors' intercept() method ?
How can two interceptors in a stack communicate or If you were to pass some value from one interceptor to another, by using this value the next interceptor executes some specific statements, how would you do it ?
What is dynamic method invocation ?..

Page 4 of 4
What is the DispatchAction (Struts1) equivalent in Strtus2 ?
Struts1 provided the facility of having related action methods in a Single Action class,depending on the method parameter, the mapped methods were executed.To achieve this we have to extend the DispatchAction class in Struts1.But this comes as default in Struts2, We need to provide the qualified methods in the action class( no need to inherit any class), and provide the mapping of action path/name to the method attribute in action tag(struts.xml) and proper code in view layer.
What is the difference between DispatchAction and dynamic method invocation in Struts2 ?
How many different ways can you retrive the request parameters from within interceptor ?
Two ways.
In the first way retrieve the HttpServletRequest from the ActionContext object and retrieve the parameters the usual way. Code
?
1
2
3
ActionContext context=(ActionContext)invocation.getInvocationContext();
HttpServletRequest request=(HttpServletRequest)context.get(StrutsStatics.HTTP\_REQUEST);
String username=(String)request.getParameter("user");
The second way is pretty much the Struts2, by invoking the getParameters method on context object. Code
?
1
2
3
ActionContext context=(ActionContext)invocation.getInvocationContext();
Map requestParameters=context.getParameters();
String usernames=((String[.md](.md))m.get("user"))[0](0.md);
As you can see the map doesn't return the parameter as String unlike the Servlet specification, but an array of String  (can be convinient for multivalued UI controls check box,single value UI controls data can be retrived as in above code ).
What is abstract package in Struts2, and what is its use ?
An abstract package usually defines inheritable components such as intercetpor,different interceptor stacks,result types etc.But doesn't contain any actions.The way we declare a package as abstract is through the package elements attribute as abstract and setting the value to "true". By default ( if abstract attribute is not mentioned) it is false.Struts-default.xml is an example of abstract package.
Does Struts2 mandates of implementing the Action interface in action classes to have the default Action method (execute)?
Struts2 doesn't mind if the action classes doesn't implement the Action interface, to have the execute method executed on default action method selection.Even though it appears that Action interfaces has the execute method declaration and one must implement it to have the execute method overriden in the action classes,Struts2 takes it as an informal contract with the developer by letting the developer to have an execute method conforming to the signature of execute method.Whenever Struts2 finds any voilation to the declaration of the method in unimplemented action class it throws the exeception.