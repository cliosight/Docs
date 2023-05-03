# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

## SQL interface for structured data ##
Cliosight will support the top database servers like MySQL, Postgres, MS SQL, Google BigQuery, Amazon Dynamo DB, Azure Cosmos DB and others.
With our SQL interface, users can perform all possible analytical operations in both in-house and remote databases. We will also expand to other sources like distributed ledgers in future releases.


## Support for multiple datasources ##
Paid users will have the option to create multiple datasources for in-house and remote databases. Free tier will only have one in-house MySQL database with shared resources. Uploading large volumes of data will be much faster for paid users due to dedicated resources.
   
  
## A relational database example ##
Let's consider a database design for company meetings. A **meeting** can be either an **interview** or a **consultation** . A meeting can be sent to individuals or a **group** of individuals whose **contact** information is already saved in the database. Therefore, a meeting can of two types and it can have both individuals added at the time of creation and pre-existing groups of contacts. In order to track the individuals who attended a meeting, there has to be a direct association between a meeting and a contact. This is because a group might be edited after a meeting has taken place. Alternately, if a contact is removed or added to a group in a meeting, after it is scheduled (created) but prior to the start time, then a meeting cancellation or invite email has to sent to the contact's email id. This scenario can only be handled with triggers.
    
## Schema details ##
### Meeetings table ###

CREATE TABLE `meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_code` varchar(255) NOT NULL, `title` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `purpose` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) ) 
    
### Interviews table ###

CREATE TABLE `interviews` ( `id` int NOT NULL AUTO_INCREMENT, `candidate_email` varchar(255) NOT NULL, `candidate_name` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )     
       
### Consultations table ###

CREATE TABLE `consultations` ( `id` int NOT NULL AUTO_INCREMENT, `client_email` varchar(255) NOT NULL, `client_name` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `country_code` varchar(255) DEFAULT NULL, `client_mobile` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )   

### Meetings_Interviews Association ###
   
CREATE TABLE `meetings_interviews` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int DEFAULT NULL, `interview_id` int DEFAULT NULL, PRIMARY KEY (`id`) )   

### Meetings_Consultations Association ###
   
CREATE TABLE `meetings_consultations` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int DEFAULT NULL, `consultation_id` int DEFAULT NULL, PRIMARY KEY (`id`) ) 
      
### Contacts table ###

CREATE TABLE `contacts` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) DEFAULT NULL, `email` varchar(255) NOT NULL, `phone` varchar(255) DEFAULT NULL,  PRIMARY KEY (`id`) )     

### Groups table ###

CREATE TABLE `groups` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) NOT NULL, `admin_email` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )
      
### Groups_Contacts Association ###

CREATE TABLE `groups_contacts` ( `id` int NOT NULL AUTO_INCREMENT, `contact_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) )    
    
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







