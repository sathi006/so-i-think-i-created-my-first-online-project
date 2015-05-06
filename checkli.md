<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN" "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">


&lt;hibernate-mapping&gt;


> <class dynamic-update="true" mutable="true"
> > name="domain.checklist.CheckList" optimistic-lock="version"
> > polymorphism="implicit" select-before-update="true" table="Checklist\_Template">


> 

&lt;id column="CHECKLIST\_TEMPLATE\_ID" name="listId"&gt;


> > <!-- Changes to use Oracle Sequences instead of Hibernate Incrementor - Zayeem - Starts -->
> > <!--
> > 

&lt;generator class="increment"&gt;



&lt;/generator&gt;


> > > -->

> > 

&lt;generator class="native" &gt;


> > > 

&lt;param name="sequence"&gt;

seq\_chlist\_template

&lt;/param&gt;



> > 

&lt;/generator&gt;


> > <!-- Changes to use Oracle Sequences instead of Hibernate Incrementor - Zayeem - Ends -->

> 

&lt;/id&gt;


> 

&lt;property column="Checklist\_Desc" name="description" /&gt;


> 

&lt;set name="checkListItems" lazy="false" cascade="all" fetch="join"&gt;


> > 

&lt;key column="CHECKLIST\_TEMPLATE\_ID" /&gt;


> > <!--			

&lt;list-index column="CHECKLIST\_TEMPLATE\_ITEMS\_ID" /&gt;

-->
> > 

&lt;one-to-many class="domain.checklist.CheckListItem" /&gt;



> 

&lt;/set&gt;




> 

&lt;component name="createdBy" class="domain.user.User"&gt;


> > 

&lt;property column="CREATED\_BY" name="userId" update="false" /&gt;



> 

&lt;/component&gt;


> 

&lt;component name="modifiedBy" class="domain.user.User"&gt;


> > 

&lt;property column="MODIFIED\_BY" name="userId" /&gt;



> 

&lt;/component&gt;


> 

&lt;property column="CREATED\_DT" name="createdOn" update="false" /&gt;


> 

&lt;property column="MODIFIED\_DT" name="updatedOn" /&gt;


> 

Unknown end tag for &lt;/class&gt;




&lt;/hibernate-mapping&gt;

