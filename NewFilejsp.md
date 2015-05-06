

&lt;jsp:useBean id="user" class="user.UserData" scope="session"/&gt;




&lt;jsp:getProperty  name="user" property="username"/&gt;




&lt;jsp:getProperty  name="user" property="email"/&gt;




&lt;jsp:getProperty  name="user" property="age"/&gt;




&lt;html&gt;




&lt;head&gt;




&lt;meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1"&gt;




&lt;title&gt;

Insert title here

&lt;/title&gt;




Unknown end tag for &lt;/head&gt;




&lt;body&gt;



name 

&lt;input TYPE="TEXT" value="&lt;%=user.getUsername() %&gt;" SIZE="20"&gt;



&lt;BR&gt;


Email 

&lt;input TYPE="TEXT" value="&lt;%=user.getEmail() %&gt;" SIZE="20"&gt;



&lt;BR&gt;


age 

&lt;input TYPE="TEXT" NAME="&lt;%=user.getAge() %&gt;" SIZE="4"&gt;




Unknown end tag for &lt;/body&gt;




Unknown end tag for &lt;/html&gt;

