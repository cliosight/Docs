# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

## Performing the basic CRUD operations with Cliosight ##
Cliosight will support the top database servers like MySQL, Postgres, MS SQL Server, Google BigQuery, Amazon Dynamo DB and others.
With SQL interface users can perform all possible CRUD operations on the data in both in-house and external databases.
   
  
## A relational database example ##
Let's consider a database design for company meetings. A **meeting** can be either an **interview** or a **consultation** . A meeting can be sent to individuals or **group** of individuals whose **contact** information is already saved in the database. Therefore, a meeting can of two types and they can have both individuals added to the meeting and/or pre-existing groups of contacts. In order to track the individuals who attended a meeting, there has to be a direct association between a meeting and a contact. This is because group details can be edited by design for instance, after a meeting has taken place.
    
## Schema details ##
### Meeeting table ###

CREATE TABLE `meeting` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_code` varchar(255) NOT NULL, `title` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `purpose` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) ) 
    
### Interview table ###

CREATE TABLE `interview` ( `id` int NOT NULL AUTO_INCREMENT, `candidate_email` varchar(255) NOT NULL, `candidate_name` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )     
       
### Consultation table ###

CREATE TABLE `consultation` ( `id` int NOT NULL AUTO_INCREMENT, `client_email` varchar(255) NOT NULL, `client_name` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `country_code` varchar(255) DEFAULT NULL, `client_mobile` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )     
      
### Contact table ###

CREATE TABLE `contact` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) DEFAULT NULL, `email` varchar(255) NOT NULL, `phone` varchar(255) DEFAULT NULL,  PRIMARY KEY (`id`) )     

### Group table ###

CREATE TABLE `group` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) NOT NULL, `admin_email` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )
      
### Group_Contact Association ###

CREATE TABLE `groups_contacts` ( `id` int NOT NULL AUTO_INCREMENT, `contact_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) )    
    
### Group_Meeting Association ###

CREATE TABLE `groups_meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) )   
     
### Contacts_Meetings Association ###   
CREATE TABLE `contacts_meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `contact_id` int NOT NULL, PRIMARY KEY (`id`) )


## Handling multiple datasources and connection pools ##
Paid users will have the option to create multiple datasources for in-house and remote databases. Free tier will only have one in-house MySQL database with shared a connection pool. This makes uploading huge volumes of data faster for paid users. Optionally, they will also have the option to deploy Cliosight into their VPC for a dedicated connection pool and higher security of their sensitive data.
   

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







