<%@ page autoFlush="true" %>



&lt;HTML&gt;




&lt;script type="text/javascript"&gt;


/**function submit1(){
> String name = request.getParameter( "textd" );
alert("hey");
> session.setAttribute("sessionBoo", request.getParameter("textd"));
> if(textd.value==true){
> > session.setAttribute("sessionBoo", "truedame");

> }
> else{
> > session.setAttribute("sessionBoo", "falselame");

> }
> innertext.value =  session.getAttribute("sessionBoo");
}**/


&lt;/script&gt;




&lt;BODY&gt;


Hello, world<br>
</br>


<%-- 

&lt;jsp:setProperty property="boolean1" name="boolean1" /&gt;

 --%>
<%-- <%! Boolean boolean1= true;%>
<% if(Boolean.parseBoolean(request.getParameter("username"))) {%>


&lt;jsp:include page="/WEB-INF/NewFile.jsp"&gt;



&lt;/jsp:include&gt;


<% }
else { %>


&lt;jsp:forward page="/WEB-INF/NewFile.jsp"&gt;



&lt;/jsp:forward&gt;


<% } %>
<%
System.out.println( "Evaluating date now" );
java.util.Date date= new java.util.Date(); %> --%>
Hello!  The time is now <%= request.getContextPath() %> <% out.println(new java.util.Date());%>


&lt;FORM METHOD=POST Action="&lt;%= request.getContextPath() %&gt;/NewFile.jsp"&gt;




&lt;jsp:useBean id="user" class="user.UserData" scope="session"/&gt;




&lt;jsp:setProperty name="user" property="\*"/&gt;


What's your name? 

&lt;INPUT TYPE="TEXT" name="username" size="20"&gt;



&lt;BR&gt;


What's your e-mail address? 

&lt;INPUT TYPE="TEXT" name="email" size="20"&gt;



&lt;BR&gt;


What's your age? 

&lt;INPUT TYPE="TEXT" name="age" size="4"&gt;





&lt;input type="submit"  title="click" value="Click me"&gt;


hey this is the input i gave previously <%= session.getAttribute("sessionBoo") %>


Unknown end tag for &lt;/FORM&gt;





&lt;Br/&gt;




Unknown end tag for &lt;/BODY&gt;




Unknown end tag for &lt;/HTML&gt;

