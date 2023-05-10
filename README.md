# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

## SQL interface for structured data ##
Cliosight will support the top database servers like MySQL, Postgres and MS SQL. Also AWS Dynamo DB, Azure Cosmos DB and other cloud services like Google BigQuery.
With our SQL interface, users can perform all possible analytical operations in both in-house and remote databases. We will also expand to other sources like distributed ledgers in future releases.


## Support for multiple datasources ##
Paid users will have the option to create multiple datasources for in-house and remote databases. Free tier will only have one in-house MySQL database with shared resources. Uploading large volumes of data will be much faster for paid users due to dedicated resources.
   
## User permissions and access control ##
Each component created using Cliosight will have fine-grained access control. The admin will provide necessary permissions to users so that data can be uploaded, viewed or edited in a controlled manner. For instance, restricting access to datasets and reports based on the geographic location of users. Similarly, the admin can specify users who can create or execute triggers and workflows for actions on data and insights. A simple use case for this is geographically targeted online marketing campaigns. Same applies for files like images and PDFs that are stored in the cloud storage. Trial accounts will have limited database and file storage capacity. 

## A relational database example ##
Let's consider a database design for company meetings. A **meeting** can be either an **interview** or a **consultation** . A meeting can be sent to individuals or a **group** of individuals whose **contact** information is already saved in the database. Also, pre-existing contacts can be added to the meeting explicitly. A meeting, group and contact can be edited at any time. In order to track the individuals who were invited for a meeting, there has to be a direct association between the meeting and a contact. This is because a group might be edited after a meeting has taken place. Alternately, if a contact is removed or added to a group in a meeting, after it is scheduled (created) but prior to the start time, then the table connecting a meeting and a contact has to be updated accordingly. Also, a meeting cancellation or invite email has to sent to the contact's email. This is similar to meeting scheduled, updated and cancelled notifications for all participants. This scenario can only be handled with SQL triggers and scheduled jobs.  
Similarly, other important considerations may include:    
1. Handling time conflicting meetings  
2. Acknowledgement from attendees
3. Tracking the session length and who all attended a meeting 
4. Keeping an account of the highlights  
5. Summary in follow up email to attendees and stakeholders    
6. Taking care of time zone differences   


The forms created are given below:   
[Contact](https://demo.cliosight.com/app/forms/35/show?noNavbar=true)  
[Group](https://demo.cliosight.com/app/forms/34/show?noNavbar=true)   
[Meeting](https://demo.cliosight.com/app/forms/52/show?noNavbar=true)  
    
## Schema details ##

### Contacts table ###

CREATE TABLE `contacts` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) DEFAULT NULL, `email` varchar(255) NOT NULL, `phone` varchar(255) DEFAULT NULL, `type` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )     

### Groups table ###

CREATE TABLE `groups` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) NOT NULL, `admin_email` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )
      
### Groups_Contacts Association ###

CREATE TABLE `groups_contacts` ( `id` int NOT NULL AUTO_INCREMENT, `contact_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) )    

### Meetings table ###

CREATE TABLE `meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_code` varchar(255) NOT NULL, `title` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `purpose` varchar(255) DEFAULT NULL, `soft_delete` tinyint(1) DEFAULT '0', PRIMARY KEY (`id`) ) 
    
### Interviews table ###

CREATE TABLE `interviews` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `skill` varchar(255) DEFAULT NULL, `experience` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) ) 
       
### Consultations table ###

CREATE TABLE `consultations` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `module` varchar(255) DEFAULT NULL, `category` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )    
      
    
### Groups_Meetings Association ###

CREATE TABLE `groups_meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) ) 

### Contacts_Meetings Association ###

CREATE TABLE `contacts_meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `contact_id` int NOT NULL, PRIMARY KEY (`id`) ) 
     
   

## JSON fields of a Form ##

## Examples of Forms in Cliosight's syntax ##
   
   
  

## JSON fields of a Report ##

## Examples of Reports in Cliosight's syntax ##

## Using reports' data for plotting graphs using various JavaScript libraries ##



## JSON fields of a Dashboard ##

## Examples of Dashboards in Cliosight's syntax ##
  
## Claiming trustworthiness 


## JSON fields of a Trigger ##

## Examples of commonly used Triggers in Cliosight's syntax ##







