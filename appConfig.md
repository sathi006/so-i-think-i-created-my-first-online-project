<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
> xmlns:security="http://www.springframework.org/schema/security"
> xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
> xsi:schemaLocation="
> > http://www.springframework.org/schema/beans
> > http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
> > http://www.springframework.org/schema/security
> > http://www.springframework.org/schema/security/spring-security-2.0.4.xsd
> > http://www.springframework.org/schema/context
> > http://www.springframework.org/schema/context/spring-context-2.5.xsd">


> <!-- Properties file -->

> 

&lt;context:property-placeholder location="classpath:spring-config.properties" /&gt;



> 

&lt;bean id="vamDataSource" class="org.springframework.jndi.JndiObjectFactoryBean"&gt;


> > 

&lt;property name="jndiName" value="java:vamDS"&gt;



&lt;/property&gt;



> 

&lt;/bean&gt;



> <!--

&lt;bean id="vamEmailSource" class="org.springframework.jndi.JndiObjectFactoryBean"&gt;


> > 

&lt;property name="jndiName" value="mail/VAMNotify"&gt;



&lt;/property&gt;



> 

&lt;/bean&gt;



> 

&lt;bean id="vamEmailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl"&gt;


> > 

&lt;property name="session" ref="vamEmailSource"&gt;



&lt;/property&gt;



> 

&lt;/bean&gt;


> --><!--  Datasource using  JDBC
> <bean id="vampDataSource"
> > class="org.springframework.jdbc.datasource.DriverManagerDataSource">
> > 

&lt;property name="driverClassName" value="${jdbc.driverClassName}" /&gt;


> > 

&lt;property name="url" value="${jdbc.url}" /&gt;


> > 

&lt;property name="username" value="${jdbc.username}" /&gt;


> > 

&lt;property name="password" value="${jdbc.password}" /&gt;



> 

Unknown end tag for &lt;/bean&gt;


-->

> <!-- Hibernate SessionFactory -->
> <bean id="vamDBSessionFactory"
> > class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
> > 

&lt;property name="dataSource"&gt;


> > > 

&lt;ref local="vamDataSource" /&gt;



> > 

&lt;/property&gt;


> > 

&lt;property name="mappingResources"&gt;


> > > 

&lt;list&gt;


> > > > 

&lt;value&gt;

/hibernate/Somhbm.hbm.xml
> > > > 

&lt;/value&gt;




> 

&lt;/list&gt;


> 

&lt;/property&gt;


> 

&lt;property name="hibernateProperties"&gt;


> > 

&lt;props&gt;


> > > 

&lt;prop key="hibernate.dialect"&gt;


> > > > ${hibernate.dialect}
> > > > > 

&lt;/prop&gt;



> > > 

&lt;prop key="hibernate.show\_sql"&gt;

true

&lt;/prop&gt;


> > > 

&lt;prop key="hibernate.default\_schema"&gt;

${jdbc.database}

&lt;/prop&gt;


> > > 

&lt;prop key="hibernate.use\_outer\_join"&gt;

true

&lt;/prop&gt;



> > 

&lt;/props&gt;



> 

&lt;/property&gt;


> 

&lt;property name="eventListeners"&gt;


> > 

&lt;map&gt;


> > > 

&lt;entry key="merge"&gt;


> > > > <bean
> > > > > class="org.springframework.orm.hibernate3.support.IdTransferringMergeEventListener" />

> > > 

&lt;/entry&gt;



> > 

&lt;/map&gt;



> 

&lt;/property&gt;


> 

Unknown end tag for &lt;/bean&gt;



> <!--
> > Enable annotation-based configuration.

> -->
> <!--

&lt;context:annotation-config /&gt;


> --><!-- 	

&lt;context:component-scan base-package="com.bo.vam" /&gt;

  -->

> <!--
> > ========================= TRANSACTION MANAGEMENT
> > 

> -->
> <bean id="txManager"
> > class="org.springframework.orm.hibernate3.HibernateTransactionManager">
> > 

&lt;property name="sessionFactory"&gt;


> > > 

&lt;ref local="vampDBSessionFactory" /&gt;



> > 

&lt;/property&gt;



> 

Unknown end tag for &lt;/bean&gt;



> <!--
    * Log4j
> > initializer

> -->

> <bean id="log4jInitialization"
> > class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">
> > 

&lt;property name="targetClass" value="org.springframework.util.Log4jConfigurer" /&gt;


> > 

&lt;property name="targetMethod" value="initLogging" /&gt;


> > 

&lt;property name="arguments"&gt;


> > > 

&lt;list&gt;


> > > > 

&lt;value&gt;

classpath:conf/log4j.xml

&lt;/value&gt;



> > > 

&lt;/list&gt;



> > 

&lt;/property&gt;



> 

Unknown end tag for &lt;/bean&gt;



> 

&lt;bean id="loggingInterceptor" class="common.core.logging.LoggingInterceptor" /&gt;





Unknown end tag for &lt;/beans&gt;

