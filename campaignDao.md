import java.lang.reflect.Array;
import java.sql.Timestamp;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Date;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;
import java.util.Set;

import org.hibernate.FetchMode;
import org.hibernate.HibernateException;
import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.criterion.CriteriaSpecification;
import org.hibernate.criterion.DetachedCriteria;
import org.hibernate.criterion.Order;
import org.hibernate.criterion.Projections;
import org.hibernate.criterion.Restrictions;
import org.springframework.dao.DataAccessException;
import org.springframework.orm.hibernate3.HibernateCallback;
import org.springframework.orm.hibernate3.HibernateTemplate;
import org.springframework.orm.hibernate3.support.HibernateDaoSupport;



public class CampaignDaoImpl extends HibernateDaoSupport implements
> ICampaignDao {


> private static final Log LOGGER=new Log(CampaignDaoImpl.class);
> private static String THIS\_CLASS=CampaignDaoImpl.class.getName();

> /
    * This method is used to persist the new campaign information and return
    * the new campaign details. It will performs all the necessary business
    * validation like checking whether campaign name already exists, checking
    * whether campaign name contains only alphanumeric characters or update an
    * existing Campaign or disable a campaign. The mandatory fields are name,
    * description and status inside campaign object along with the user id.
    * 
    * @param campaign
    * the campaign object with new campaign information
    * @return the new campaign information.
    * @throws WrappedSystemException
    * all the system exception will be wrapped in this class and
    * throws to the caller.
    * 

> public Campaign saveCampaign(Campaign campaign)
> > throws WrappedSystemException {
> > try{
> > > // persisting in database
> > > List

&lt;Assessment&gt;

 campaignList = new ArrayList

&lt;Assessment&gt;

();
> > > Set

&lt;Assessment&gt;

 newList = new HashSet

&lt;Assessment&gt;

();
> > > Set

&lt;Assessment&gt;

 previousList = campaign.getAssessments();


> //Defect - 6941 - nbkqroq
> /**if(null != previousList){
> > for (Iterator iterator = previousList.iterator(); iterator
> > > .hasNext();) {
> > > Assessment assessment = (Assessment) iterator.next();
> > > final Assessment previousAssessment =
> > > (Assessment) getHibernateTemplate().get(
> > > Assessment.class, null!=assessment.getAssessmentId()?assessment.getAssessmentId().intValue():0);**


> previousAssessment.setModifiedBy(assessment.getModifiedBy());
> previousAssessment.setUpdatedOn(assessment.getUpdatedOn());
> newList.add(previousAssessment);
> campaignList.add(previousAssessment);
> }
> campaign.setAssessments(newList);
> } **/**

> campaign.setAssessments(null);

> // #7250- fix - nbkzwyw -start
> /**if(campaign.getCampaignStatus().getValue().equals(
> > Status.INACTIVE.getValue())||campaign.getCampaignStatus().getValue().equals(
> > > Status.DISABLED.getValue())){

> > removeAssessmentFromCampaign(campaignList);
> > campaign.setAssessments(null);

> }**/
> // #7250- fix - nbkzwyw -end

> campaign = (Campaign) getHibernateTemplate().merge(campaign);

> //Added for Defect 6941 - nbkqroq
> if(null != previousList){
> > for (Iterator iterator = previousList.iterator(); iterator
> > > .hasNext();) {
> > > Assessment assessment = (Assessment) iterator.next();
> > > updateCampaignId(campaign.getCampaignId(), assessment.getAssessmentId());

> > }

> }

> }catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }
> // Return Campaign object
> return campaign;
> }

> // Added new method for Defect - 6941 - nbkqroq - start
> /
    * This method is used to update the campaign Id in Assessment table.
    * 
    * @param campaignId
    * the campaign id.
    * @return the result, the boolean value.
    * @throws WrappedSystemException
    * all the system exception will be wrapped in this class and
    * throws to the caller.
    * 
> public Boolean updateCampaignId( final int campaignId, final int assessmentId)
> throws WrappedSystemException {

> HibernateTemplate hibernateTemplate = getHibernateTemplate();
> Boolean result = Boolean.FALSE;

> try {
> > result =
> > > (Boolean)hibernateTemplate.execute(new HibernateCallback() {
> > > > public Object doInHibernate(final Session session)
> > > > throws HibernateException {


> String hql = "update Assessment set campaignId = :newCampaignId where assessmentId = :assessmentId";
> Query query = session.createQuery(hql);
> query.setInteger("newCampaignId", campaignId);
> query.setInteger("assessmentId", assessmentId);


> int rowCount = query.executeUpdate();

> Boolean result = Boolean.FALSE;
> if (rowCount > 0) {
> > result = Boolean.TRUE;

> }

> return result;
> }
> });
> }
> catch(DataAccessException dataAccessException){
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }
> return result;
> }
> // Added new method for Defect - 6941 - nbkqroq - end

> /
    * This method is used to retrieve all the available campaign information
    * and return the list of campaigns.
    * 
    * @return the list of {@link Campaign} object
    * @throws WrappedSystemException
    * all the system exception will be wrapped in this class and
    * throws to the caller.
    * 
> public List

&lt;Campaign&gt;

 getAllAvailableCampaigns()
> throws WrappedSystemException {
> > final List

&lt;Campaign&gt;

 campaignList = new ArrayList

&lt;Campaign&gt;

();


> // String buffer to hold the criteria for the search query
> StringBuffer queueWhereQuery = new StringBuffer(70);

> // List to hold the search parameter
> final List

&lt;Object&gt;

 queueParameters = new ArrayList

&lt;Object&gt;

(8);

> // String buffer to hold the from clause of the search query
> StringBuffer queueFromQuery = new StringBuffer(70);

> // From part of the query
> queueFromQuery.append(" Select campaign.campaignId,");
> queueFromQuery.append("campaign.campaignName,campaign.description, ");
> queueFromQuery.append(" campaign.campaignStatus,campaign.createdOn,");
> queueFromQuery.append("campaign.createdBy, ");
> queueFromQuery.append(" campaign.updatedOn,campaign.modifiedBy,campaign.reasonForInactive, ");

> // Count number of assessment id from assessment
> //queueFromQuery.append(" count(assessment.assessmentId) ");

> queueFromQuery.append(" (select count(assessment.assessmentId) From Assessment as assessment where  assessment.campaignId=campaign.campaignId)");

> queueFromQuery.append(" From Campaign as campaign  ");
> //Defect 6941 - nbkqroq
> //queueFromQuery.append(" left outer join campaign.assessments ");
> //queueFromQuery.append("as assessment ");

> // Where part of the query
> queueWhereQuery.append(" where ");
> queueWhereQuery.append(" campaign.campaignStatus !=? ");

> //Defect 6941 - nbkqroq
> //queueWhereQuery.append(" and (assessment.assessmentActiveStatus=? ");
> //queueWhereQuery.append(" or assessment is null) ");

> queueWhereQuery.append(" group by campaign.campaignId,");
> queueWhereQuery.append("campaign.campaignName,campaign.description, ");
> queueWhereQuery.append(" campaign.campaignStatus,");
> queueWhereQuery.append(	"campaign.createdOn,campaign.createdBy, ");
> queueWhereQuery.append(" campaign.updatedOn,campaign.modifiedBy,campaign.reasonForInactive ");

> // Set query parameters
> // queueParameters.add(Status.ACTIVE);
> queueParameters.add(Status.DISABLED);

> //Defect 6941 - nbkqroq
> //queueParameters.add(AssessmentActiveStatus.ACTIVE);


> try {
> > final String query = queueFromQuery.append(
> > > queueWhereQuery).toString();


> // querying from the database.
> final List

&lt;Object&gt;

 objects = getHibernateTemplate().find(query,
> > queueParameters.toArray());


> if(objects != null){
> > for (final Iterator

&lt;Object&gt;

 iterator = objects.iterator();
> > iterator.hasNext();) {


> final Campaign campaign = new Campaign();
> Object element[.md](.md) = (Object[.md](.md)) iterator.next();
> // setting the values in campaign object
> campaign.setCampaignId((Integer) element[0](0.md));
> campaign.setCampaignName((String) element[1](1.md));
> campaign.setDescription((String) element[2](2.md));
> campaign.setCampaignStatus((Status) element[3](3.md));
> campaign.setCreatedOn((Timestamp) element[4](4.md));
> campaign.setCreatedBy((User) element[5](5.md));
> campaign.setUpdatedOn((Timestamp) element[6](6.md));
> campaign.setModifiedBy((User) element[7](7.md));
> campaign.setReasonForInactive((String) element[8](8.md));
> campaign.setNoOfAssessments(((Long) element[9](9.md)).intValue());

> campaignList.add(campaign);
> }
> }

> } catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }

> return campaignList;

> }


> /
    * Gets List of assessments not related with any campaign.
    * 
    * @return the assessments,the list of assessments not related with any
    * campaign.
    * @throws WrappedSystemException
    * the wrapped system exception
    * 
> public List

&lt;Assessment&gt;

 getAssements() throws WrappedSystemException {
> > List

&lt;Assessment&gt;

 list = null;


> // String buffer to hold the criteria for the  query
> final StringBuffer query = new StringBuffer(100);

> // Create query
> query.append(" From  Assessment as assessment WHERE ");
> query.append("(assessment.assessmentStatus !=? or ");
> query.append("assessment.assessmentStatus is null) AND  " );
> query.append("assessment.campaignId > ? ");

> try {
> > // Querying from the database.
> > list = getHibernateTemplate()
> > > .find(query.toString(),
> > > > new Object[.md](.md) {
> > > > > AssessmentStatus.ASSESSMENT\_FINALIZED, 0 });

> } catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }

> return list;
> }

> /
    * Gets List assessments related with a particular campaign.
    * 
    * @param campaignId
    * the campaign id
    * @return the assessments,the list of assessments related with a particular
    * campaign.
    * @throws WrappedSystemException
    * all the system exception will be wrapped in this class and
    * throws to the caller.
    * 
> @Deprecated
> public Set

&lt;Application&gt;

 getCampaignAssementsBackup(final int campaignId)
> > throws WrappedSystemException {
> > Set

&lt;Application&gt;

 applicationsResult = new HashSet

&lt;Application&gt;

(10);




> // String buffer to hold the criteria for the search query
> final StringBuffer queueWhereQuery = new StringBuffer(100);
> // List to hold the search parameter
> final List

&lt;Object&gt;

 queueParameters = new ArrayList

&lt;Object&gt;

(8);
> // String buffer to hold the from clause of the search query
> final StringBuffer queueFromQuery = new StringBuffer(100);

> // query to retrieve assessments from Assessment table based on the
> // assessment status.
> queueWhereQuery.append(" where ");
> queueFromQuery.append(" From Application as application ");
> queueFromQuery
> .append(" inner join application.assessments as assessment ");
> queueWhereQuery.append("  assessment.campaignId = ? ");
> queueWhereQuery.append("  and assessment.assessmentActiveStatus !=? ");
> queueParameters.add(campaignId);
> queueParameters.add(AssessmentActiveStatus.INACTIVE);
> try {
> > final String query = queueFromQuery.append(
> > > queueWhereQuery).toString();


> // querying from the database.
> final List

&lt;Object&gt;

 objects = getHibernateTemplate().find(query,
> > queueParameters.toArray());


> if (objects != null) {
> > applicationsResult = getApplicationObjects(objects);

> }
> } catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }

> return applicationsResult;
> }

//Defect # 6294 & 6658 fix - - start
> /
    * Gets List assessments related with a particular campaign.
    * 
    * @param campaignId
    * the campaign id
    * @return the assessments,the list of assessments related with a particular
    * campaign.
    * @throws WrappedSystemException
    * all the system exception will be wrapped in this class and
    * throws to the caller.
    * 
> @Override
> public Set

&lt;Application&gt;

 getCampaignAssements(final int campaignId)
> > throws WrappedSystemException {


> Set

&lt;Application&gt;

 applicationsResult = new HashSet

&lt;Application&gt;

(10);
> //Defect # 6294 & 6658 fix - - 02092011 start
> String assessorSubQry = "(select distinct users.first\_name as firstname from _owner.Assessor assessor,_owner.tusers users  where users.users\_id=assessor.users\_id and assessor.ASSIGNMENT\_END\_DT is null and rownum<=1  and assessor.assessment\_id=assessment1_.assessment\_id  ) as assessorFirstname" ;
> String assessorSubQryLastName = "(select distinct users.last\_name as lastname from_owner.Assessor assessor,_owner.tusers users  where users.users\_id=assessor.users\_id and assessor.ASSIGNMENT\_END\_DT is null and rownum<=1  and assessor.assessment\_id=assessment1_.assessment\_id  ) as assessorname" ;
> String beaSubQry = "(select distinct users.first\_name as firstname from _owner.ENGAGEMENT\_OWNER bea,_owner.tusers users  where users.users\_id=bea.username and bea.engagement\_end\_dt is null and rownum<=1  and bea.assessment\_id=assessment1_.assessment\_id  ) as beaFirstname" ;
> String beaSubQryLastName = "(select distinct users.last\_name as lastname from_owner.ENGAGEMENT\_OWNER bea,_owner.tusers users  where users.users\_id=bea.username and bea.engagement\_end\_dt is null and rownum<=1  and bea.assessment\_id=assessment1_.assessment\_id  ) as beaname" ;

> //Defect # 6294 & 6658 fix - - 02092011 end
> DetachedCriteria criteria = DetachedCriteria.forClass(Application.class)
> .createAlias("assessments", "assessment")
> .createAlias("assessments.businessEngagementAssosciate", "bea",CriteriaSpecification.LEFT\_JOIN)
> .setFetchMode("assessment.assessmentType", FetchMode.JOIN)
> .setFetchMode("adsfScores" , FetchMode.SELECT)
> .setFetchMode("modifiedBy", FetchMode.SELECT)
> .setFetchMode("assessment.prerequisites",FetchMode.SELECT )
> .setFetchMode("assessment.assessmentCheckListItems",FetchMode.SELECT )
> .setFetchMode("assessment.businessEngagementAssosciate",FetchMode.SELECT )
> .setFetchMode("assessment.assessor",FetchMode.SELECT )
> .setFetchMode("assessment.assessmentType.teams",FetchMode.SELECT )
> .setProjection(Projections.distinct(Projections.projectionList()
> > .add(Projections.property("id"))
> > .add(Projections.property("applicationNumber"))
> > .add(Projections.property("applicationManager"))
> > .add(Projections.property("assessment.assessmentName"))
> > .add(Projections.property("assessment.assessmentId"))
> > .add(Projections.property("assessment.assessmentType"))
> > .add(Projections.property("assessment.requestedStartDate"))
> > .add(Projections.property("tenDotHierarchy"))
> > .add(Projections.property("assessment.assessmentStatus"))
> > .add(Projections.property("assessment.assessmentActiveStatus"))
> > //Defect # 6294 & 6658 fix - - 02092011 start
> > //	.add(Projections.property("bea.userName"))
> > //	.add(Projections.property("bea.endDate"))
> > > .add(Projections.sqlProjection(beaSubQry, new String[.md](.md){"beaFirstname"},
> > > > new org.hibernate.type.Type[.md](.md){new org.hibernate.type.StringType()}))

> > > .add(Projections.sqlProjection(beaSubQryLastName, new String[.md](.md){"beaname"},
> > > > new org.hibernate.type.Type[.md](.md){new org.hibernate.type.StringType()}))

> > > .add(Projections.sqlProjection(assessorSubQry, new String[.md](.md){"assessorFirstname"},
> > > > new org.hibernate.type.Type[.md](.md){new org.hibernate.type.StringType()}))

> > > .add(Projections.sqlProjection(assessorSubQryLastName, new String[.md](.md){"assessorname"},
> > > > new org.hibernate.type.Type[.md](.md){new org.hibernate.type.StringType()})

> > > )
> > > //Defect # 6294 & 6658 fix - - 02092011 end

> ))
> .add(Restrictions.eq("assessment.campaignId",campaignId));

> //	.add(Restrictions.eq("assessment.assessmentActiveStatus", AssessmentActiveStatus.ACTIVE));
> //	.add(Restrictions.isNull("bea.endDate"));
> > criteria.addOrder(Order.asc("assessment.requestedStartDate"))	;
> > // Defect 2963 - Query changed to get Deassigned Bea Assessments in unassigned Queue- - 1/6/11
> > try {


> final List

&lt;Object&gt;

 objects = getHibernateTemplate().findByCriteria(criteria);

> if (objects != null) {

> applicationsResult = getApplicationObjectSet(objects);
> }
> } catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }

> return applicationsResult;
> }
> //Defect # 6294 & 6658 fix - nbkfddx- end

> /
    * Method to copy the application object.
    * 
    * @param application
    * the application object
    * @return the application object
    * 
> private Application getApplication(final Application application) {
> > final Application applicationCopy = new Application();


> if (application != null) {

> applicationCopy.setId(application.getId());
> applicationCopy.setApplicationNumber(application
> > .getApplicationNumber());

> applicationCopy.setFullName(application.getFullName());
> applicationCopy.setApplicationManager(application
> > .getApplicationManager());

> applicationCopy.setApplicationDestination(application
> > .getApplicationDestination());

> applicationCopy.setApplicationFacing(application
> > .getApplicationFacing());

> applicationCopy.setApplicationNumber(application
> > .getApplicationNumber());

> applicationCopy
> .setTenDotHierarchy(application.getTenDotHierarchy());
> applicationCopy.setOverview(application.getOverview());
> applicationCopy.setAdsfScores(application.getADSFScores());
> }
> return applicationCopy;
> }

> //Defect # 6294 & 6658 fix - nbkfddx - 02042011- start
> /
    * Gets the application object.
    * 
    * @param objects
    * the objects
    * @param methodName
    * the method from where it is called.
    * @return the application object
    * 
> private Campaign getCampaignObject(final List

&lt;Object&gt;

 objects) {
> > final Set

&lt;Application&gt;

 applicationsResult = new HashSet

&lt;Application&gt;

(10);
> > Campaign campaign = null;
> > for (final Iterator

&lt;Object&gt;

 objectIterator = objects.iterator(); objectIterator
> > .hasNext();) {




> final Object[.md](.md) elements = (Object[.md](.md)) objectIterator.next();
> campaign = new Campaign();


> campaign.setCampaignId((Integer) elements[0](0.md));
> campaign.setCampaignName((String) elements[1](1.md));
> campaign.setDescription((String) elements[2](2.md));
> campaign.setCampaignStatus((Status) elements[3](3.md));
> campaign.setReasonForInactive((String) elements[4](4.md));

> }
> return campaign;
> }
> //Defect # 6294 & 6658 fix - nbkfddx - 02042011- end

> //Defect # 6294 & 6658 fix - nbkfddx - 02032011 - start
> /
    * Gets the application object.
    * 
    * @param objects
    * the objects
    * @param methodName
    * the method from where it is called.
    * @return the application object
    * 
> private Set

&lt;Application&gt;

 getApplicationObjectSet(final List

&lt;Object&gt;

 objects) {

> final Set

&lt;Application&gt;

 applicationsResult = new HashSet

&lt;Application&gt;

(10);

> for (final Iterator

&lt;Object&gt;

 objectIterator = objects.iterator(); objectIterator
> .hasNext();) {

> Date beaEndDate = null;
> Date assessorEndDate = null;
> Application application = null;
> Assessment assessment = null;
> final Object[.md](.md) elements = (Object[.md](.md)) objectIterator.next();
> application = new Application();
> assessment = new Assessment();

> application.setId((Integer) elements[0](0.md));
> application.setApplicationNumber((Integer) elements[1](1.md));
> application.setApplicationManager((String) elements[2](2.md));
> application.setFullName((String) elements[3](3.md));

> assessment.setAssessmentId((Integer) elements[4](4.md));
> assessment.setAssessmentType((AssessmentType) elements[5](5.md));
> assessment.setRequestedStartDate((Date) elements[6](6.md));
> // Change for time zone.
> assessment.setRequestedStartDateStr(DateUtil.convertDateToString(assessment.getRequestedStartDate()));

> application.setTenDotHierarchy((String) elements[7](7.md));
> assessment.setAssessmentStatus((AssessmentStatus) elements[8](8.md));
> assessment
> .setAssessmentActiveStatus((AssessmentActiveStatus) elements[9](9.md));
> //Defect # 6294 & 6658 fix - nbkfddx - 02092011 start
> final Set

&lt;BusinessEngagementAssosciate&gt;

 beas =
> > new HashSet

&lt;BusinessEngagementAssosciate&gt;

(
      1. ;


> final BusinessEngagementAssosciate bea = new BusinessEngagementAssosciate();
> User User2 = new User();

> if (elements[10](10.md)!=null){
> > vampUser2.setFirstName(elements[10](10.md).toString());


> if(null!=elements[11](11.md)){
> vampUser2.setLastName(elements[11](11.md).toString());
> }

> bea.setUserName(User2);
> beas.add(bea);
> assessment.setBusinessEngagementAssosciate(beas);
> }
> /**if (elements[10](10.md) != null) {
> > VAMPUser vampUser = (VAMPUser) elements[10](10.md);
> > final BusinessEngagementAssosciate bea = new BusinessEngagementAssosciate();
> > bea.setUserName(vampUser);
> > beaEndDate = (Date) elements[11](11.md);**


> if (beaEndDate == null) {
> > beas.add(bea);
> > assessment.setBusinessEngagementAssosciate(beas);

> }
> }**/**

> final Set

&lt;Assessor&gt;

 assessors = new HashSet

&lt;Assessor&gt;

(1);
> final Assessor assessor = new Assessor();
> VAMPUser vampUser = new VAMPUser();

> if (elements[12](12.md)!=null){
> > vampUser.setFirstName(elements[12](12.md).toString());


> if(null!=elements[13](13.md)){
> vampUser.setLastName(elements[13](13.md).toString());
> }

> assessor.setUserName(vampUser);
> assessors.add(assessor);
> assessment.setAssessor(assessors);
> }
> //Defect # 6294 & 6658 fix - - 02092011 end



> if (assessorEndDate == null) {

> final Set

&lt;Assessment&gt;

 assessments = new HashSet

&lt;Assessment&gt;

(100);
> assessments.add(assessment);
> application.setAssessments(assessments);


> applicationsResult.add(application);
> }
> }


> return applicationsResult;
> }

> //Defect # 6294 & 6658 fix - - 02032011 - end

> /
    * Gets the application object.
    * 
    * @param objects
    * the objects
    * 
    * @return the application object
    * 
> private Set

&lt;Application&gt;

 getApplicationObjects(final List

&lt;Object&gt;

 objects) {

> final Set

&lt;Application&gt;

 applicationsResult = new HashSet

&lt;Application&gt;

(10);

> for (final Iterator

&lt;Object&gt;

 objectIterator = objects.iterator(); objectIterator
> .hasNext();) {

> Application application = new Application();
> Assessment assessment = new Assessment();
> final Object[.md](.md) elements = (Object[.md](.md)) objectIterator.next();
> for (Object object : elements) {

> if (object instanceof Application) {
> > application = (Application) object;


> }
> if (object instanceof Assessment) {
> > assessment = (Assessment) object;

> }
> }


> Application applicationCopy = getApplication(application);
> final Set

&lt;Assessment&gt;

 assessments = new HashSet

&lt;Assessment&gt;

(100);
> assessments.add(assessment);
> applicationCopy.setAssessments(assessments);
> applicationsResult.add(applicationCopy);
> }

> return applicationsResult;
> }



> /
    * Adds the selected assessments to the Campaign.Only Business Engagement
    * manager and assessment manager can add assessments to campaign.
    * 
    * @param list
    * the list
    * @return the list
    * @throws WrappedSystemException
    * all the system exception will be wrapped in this class and
    * throws to the caller.
    * 
> public List

&lt;Assessment&gt;

 addAssessmentToCampaign(
> > List

&lt;Assessment&gt;

 assessmentList) throws WrappedSystemException {
> > // Create a list to hold the result
> > final List

&lt;Assessment&gt;

 newAssessmentList = new ArrayList

&lt;Assessment&gt;

();
> > // Iterate through assessment list
> > for (Iterator

&lt;Assessment&gt;

 assessmentListIterator
> > > = assessmentList.iterator(); assessmentListIterator.hasNext();) {
> > > // Get the assessment object from the assessment list
> > > Assessment assessment = (Assessment) assessmentListIterator.next();


> try {

> final Assessment previousAssessment
> > = (Assessment) getHibernateTemplate().get(
> > Assessment.class, null!=assessment.getAssessmentId()?assessment.getAssessmentId().intValue():0);


> previousAssessment.setModifiedBy(assessment.getModifiedBy());
> previousAssessment.setUpdatedOn(assessment.getUpdatedOn());
> previousAssessment.setCampaignId(assessment.getCampaignId());

> // Persist the changes in the database
> assessment = (Assessment) getHibernateTemplate()
> > .merge(previousAssessment);

> }
> catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes
> > > .ErrorCode("9000"), Layer.DATAACCESS, "",
> > > dataAccessException);

> }
> newAssessmentList.add(assessment);
> }
> // Return the assessment list with result
> return newAssessmentList;
> }

> /
    * This method is used to retrieve all the available campaign names
    * list of campaigns.
    * @return list of campaign names.
    * @throws WrappedSystemException  all the system exception will be wrapped
    * in this class and throws to the caller.
    * 
> public List

&lt;String&gt;

 getAvailableCampaignNames()throws WrappedSystemException {
> > List

&lt;String&gt;

 campaignNames = null;
> > try {
> > > // Querying from the database
> > > campaignNames = getHibernateTemplate()
> > > > .find(
> > > > > "Select campaign.campaignName from Campaign AS campaign " +
> > > > > "WHERE campaign.campaignStatus != ? ",
> > > > > new Object[.md](.md) { Status.DISABLED });

> > } catch (DataAccessException dataAccessException) {
> > > // 9000=Database related error, please try again later.
> > > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > > Layer.DATAACCESS, "", dataAccessException);

> > }


> return campaignNames;

> }


> //Defect # 6294 & 6658 fix - -02042011 - start performance issue while linking and delinking assessments to campaign
> @Override
> public List

&lt;Assessment&gt;

 addOrRemoveAssessmentToCampaign(
> > List

&lt;Assessment&gt;

 assessmentList,final int differentiator) {
> > // Create a list to hold the result
> > final List

&lt;Assessment&gt;

 newAssessmentList = new ArrayList

&lt;Assessment&gt;

();
> > // Iterate through assessment list
> > for (Iterator

&lt;Assessment&gt;

 assessmentListIterator
> > > = assessmentList.iterator(); assessmentListIterator.hasNext();) {
> > > // Get the assessment object from the assessment list
> > > final Assessment assessment = (Assessment) assessmentListIterator.next();
> > > final int assessmentId = null!=assessment.getAssessmentId()?assessment.getAssessmentId().intValue():0 ;
> > > try {


> /**final Assessment previousAssessment
> > = (Assessment) getHibernateTemplate().get(
> > Assessment.class, null!=assessment.getAssessmentId()?assessment.getAssessmentId().intValue():0);**


> previousAssessment.setModifiedBy(assessment.getModifiedBy());
> previousAssessment.setUpdatedOn(assessment.getUpdatedOn());
> previousAssessment.setCampaignId(null);

> // Persist the changes in the database
> assessment = (Assessment) getHibernateTemplate()
> > .merge(previousAssessment);**/

> //Update the Assessor table to set the end date of verification
> Boolean updateResult =  (Boolean) getHibernateTemplate().execute(new HibernateCallback() {
> > public Object doInHibernate(final Session session)
> > throws HibernateException {
> > > String changer=null;**


> if(differentiator==1){
> > changer="campaignId = null , ";

> }
> else if(differentiator==0){
> > changer="campaignId = :campaignId , ";
> > }



> String hql = "update Assessment set "+ changer
> + " MODIFIED\_DT = :newUpdatedOn, modifiedBy = :newModifiedBy " +
> " where assessmentId = :assessmentId ";

> Query query = session.createQuery(hql);

> if(differentiator==0){
> > query.setInteger("campaignId",assessment.getCampaignId());

> }
> query.setDate("newUpdatedOn", assessment.getUpdatedOn());
> query.setEntity("newModifiedBy", assessment.getModifiedBy());
> query.setInteger("assessmentId", assessmentId);
> int rowCount = query.executeUpdate();
> LOGGER.debug(THIS\_CLASS,"No of Rows Updated : "+rowCount);
> Boolean updateResult = Boolean.FALSE;
> if (rowCount > 0) {
> > updateResult = Boolean.TRUE;

> }

> return updateResult;
> }
> });
> }
> catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes
> > > .ErrorCode("9000"), Layer.DATAACCESS, "",
> > > dataAccessException);

> }
> newAssessmentList.add(assessment);
> }
> // Return the assessment list with result
> return newAssessmentList;
> }


> //Defect # 6294 & 6658 fix - -02042011 - end performance issue while linking and delinking assessments to campaign



> //Defect # 6294 & 6658 fix - -02042011 - start

> public Campaign getCampaignById(int CampaignId) throws WrappedSystemException{

> Campaign campaign = null;

> DetachedCriteria criteria = DetachedCriteria.forClass(Campaign.class)
> //.createAlias("assessments", "assessment")
> //.setFetchMode("assessment", FetchMode.EAGER)
> .setProjection(Projections.distinct(Projections.projectionList()
> > .add(Projections.property("campaignId"))
> > .add(Projections.property("campaignName"))
> > .add(Projections.property("description"))
> > .add(Projections.property("campaignStatus"))
> > .add(Projections.property("reasonForInactive"))


> ))
> .add(Restrictions.eq("campaignId",CampaignId));
> // Defect 2963 - Query changed to get Deassigned Bea Assessments in unassigned Queue- - 1/6/11
> try {

> final List

&lt;Object&gt;

 objects = getHibernateTemplate().findByCriteria(criteria);

> if (objects != null) {

> campaign = getCampaignObject(objects);
> }
> } catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes.ErrorCode("9000"),
> > > Layer.DATAACCESS, "", dataAccessException);

> }

> return campaign;
/**return (Campaign)getHibernateTemplate().get(Campaign.class, CampaignId);**/
> }


> //Defect # 6294 & 6658 fix - -02042011 - end
> public Campaign saveUpdateCampaign(Campaign campaign) throws WrappedSystemException{
> > return (Campaign)getHibernateTemplate().merge(campaign);

> }

> //Defect # 6294 & 6658 fix - -02042011 - start

> /
    * Update campaign after removing.
    * 
    * @param campaign the campaign
    * @return the campaign
    * @throws WrappedSystemException the wrapped system exception
    * 
> public Campaign updateCampaignAfterRemoving(final Campaign campaign) throws WrappedSystemException{
> > try{
> > Boolean updateResult =  (Boolean) getHibernateTemplate().execute(new HibernateCallback() {
> > > public Object doInHibernate(final Session session)
> > > throws HibernateException {




> String hql = "update Campaign set "+" updatedOn = :newUpdatedOn, modifiedBy = :newModifiedBy " +
> " where campaignId = :campaignId ";

> Query query = session.createQuery(hql);
> query.setDate("newUpdatedOn", campaign.getUpdatedOn());
> query.setEntity("newModifiedBy", campaign.getModifiedBy());
> query.setInteger("campaignId", campaign.getCampaignId());

> int rowCount = query.executeUpdate();
> LOGGER.debug(THIS\_CLASS,"No of Rows Updated : " +rowCount);
> Boolean updateResult = Boolean.FALSE;
> if (rowCount > 0) {
> > updateResult = Boolean.TRUE;

> }

> return updateResult;
> }
> });
> }
> catch (DataAccessException dataAccessException) {
> > // 9000=Database related error, please try again later.
> > throw new WrappedSystemException(new ErrorCodes
> > > .ErrorCode("9000"), Layer.DATAACCESS, "",
> > > dataAccessException);

> }

// Return the assessment list with result
> return campaign;
> }

}