org.springframework.web.filter.RequestContextFilter- Servlet 2.3 (older web conrtainer-If you are using an older web container

(Servlet 2.3), you will need to use the provided javax.servlet.Filter implementation.)this extra setup is not required if you just

want to use the 'standard' scopes (namely singleton and prototype).

org.springframework.web.context.request.RequestContextListener- Servlet 2.4+ listener that exposes the http request object to the

current thread, through both LocaleContextHolder and RequestContextHolder. for requests processed outside of Spring's

DispatcherServlet,for use with third-party servlets like flex servlet.
When using a Servlet 2.4+ web container, with requests processed outside of Spring's DispatcherServlet (e.g. when using JSF or

Struts), you need to add the following javax.servlet.ServletRequestListener to the declarations in your web application's 'web.xml'

file.


org.springframework.web.context.ContextLoaderListener- Servlet 2.3+ to integrate into any Java-based web framework.Bootstrap listener

to start up and shut down Spring's root WebApplicationContext.

org.springframework.web.context.ContextLoaderServlet- Servlet 2.2 Bootstrap servlet to start up Spring's root WebApplicationContext.
This servlet should have a lower load-on-startup value in web.xml than any servlets that access the root web application context.


HTTP Session-scoped bean 'userPreferences'

> <!-- a HTTP Session-scoped bean exposed as a proxy -->
> > 

&lt;bean id="userPreferences" class="com.foo.UserPreferences" scope="session"&gt;




> <!-- this next element effects the proxying of the surrounding bean -->
> 

&lt;aop:scoped-proxy/&gt;


> 

&lt;/bean&gt;



> <!-- a singleton-scoped bean injected with a proxy to the above bean -->
> 

&lt;bean id="userService" class="com.foo.SimpleUserService"&gt;



> <!-- a reference to the proxied 'userPreferences' bean -->
> 

&lt;property name="userPreferences" ref="userPreferences"/&gt;



> 

&lt;/bean&gt;



org.springframework.orm.hibernate3.LocalSessionFactoryBean- hibernate


3.4.4.1. Initial web configuration

In order to support the scoping of beans at the request, session, and global session levels (web-scoped beans), some minor initial configuration is required before you can set about defining your bean definitions. Please note that this extra setup is not required if you just want to use the 'standard' scopes (namely singleton and prototype).

Now as things stand, there are a couple of ways to effect this initial setup depending on your particular Servlet environment...

If you are accessing scoped beans within Spring Web MVC, i.e. within a request that is processed by the Spring DispatcherServlet, or DispatcherPortlet, then no special setup is necessary: DispatcherServlet and DispatcherPortlet already expose all relevant state.

When using a Servlet 2.4+ web container, with requests processed outside of Spring's DispatcherServlet (e.g. when using JSF or Struts), you need to add the following javax.servlet.ServletRequestListener to the declarations in your web application's 'web.xml' file.



&lt;web-app&gt;


> ...
> 

&lt;listener&gt;


> > 

&lt;listener-class&gt;

org.springframework.web.context.request.RequestContextListener

&lt;/listener-class&gt;



> 

&lt;/listener&gt;


> ...


&lt;/web-app&gt;



If you are using an older web container (Servlet 2.3), you will need to use the provided javax.servlet.Filter implementation. Find below a snippet of XML configuration that has to be included in the 'web.xml' file of your web application if you want to have access to web-scoped beans in requests outside of Spring's DispatcherServlet on a Servlet 2.3 container. (The filter mapping depends on the surrounding web application configuration and so you will have to change it as appropriate.)



&lt;web-app&gt;


> ..
> 

&lt;filter&gt;


> > 

&lt;filter-name&gt;

requestContextFilter

&lt;/filter-name&gt;


> > 

&lt;filter-class&gt;

org.springframework.web.filter.RequestContextFilter

&lt;/filter-class&gt;



> 

&lt;/filter&gt;


> 

&lt;filter-mapping&gt;


> > 

&lt;filter-name&gt;

requestContextFilter

&lt;/filter-name&gt;


> > 

&lt;url-pattern&gt;

/**&lt;/url-pattern&gt;



> 

&lt;/filter-mapping&gt;


> ...


&lt;/web-app&gt;**

That's it. DispatcherServlet, RequestContextListener and RequestContextFilter all do exactly the same thing, namely bind the HTTP request object to the Thread that is servicing that request. This makes beans that are request- and session-scoped available further down the call chain.

Singleton A with protoype B dependency-Beanfactoryaware in dependent A bean with  Beanfactory refernce

InitializingBean and DisposableBean change the life cycle of the bean within conrtext