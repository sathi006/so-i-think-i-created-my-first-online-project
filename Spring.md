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

