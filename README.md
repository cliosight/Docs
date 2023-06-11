# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

## SQL interface for structured data ##
Cliosight will support the top database servers like MySQL, Postgres and MS SQL. Also AWS Dynamo DB, Azure Cosmos DB and other cloud services like Google BigQuery.
With our SQL interface, users can perform all possible analytical operations for both in-house and user-owned databases. We will also expand to other emerging data sources like distributed ledgers in future releases.


## Support for multiple data sources ##
Paid users will have the option to use multiple data sources by saving the configurations for each. These can be created for the account's in-house databases or that on the user's virtual machines or for cloud database instances. Free tier will only have one in-house MySQL database with a shared connection pool. Uploading large volumes of data will be much faster for paid users due to dedicated resources.
   
## User permissions and access control ##
Each component created using Cliosight will have fine-grained access control. The admin will provide necessary permissions to users so that data can be uploaded, viewed or edited in a controlled manner. For instance, restricting access to datasets and reports based on the geographic location of users. Similarly, the admin can specify users who can create or execute triggers and workflows for actions on data and insights. A simple use case for this is geographically targeted online marketing campaigns. Same applies for files like images and PDFs that are stored in the cloud storage. Trial accounts will have limited database and file storage capacity. 

## A relational database example ##
Let's consider a database design for company meetings. A **meeting** can be either an **interview** or a **consultation** . A meeting can be sent to individuals or a **group** of individuals whose **contact** information is already saved in the database. Also, pre-existing contacts can be added to the meeting explicitly. A meeting, group and contact can be edited at any time. In order to track the individuals who were invited for a meeting, there has to be a direct association between the meeting and a contact. This is because a group might be edited after a meeting has taken place. Alternately, if a contact is removed or added to a group in a meeting, after it is scheduled (created) but prior to the start time, then the table connecting a meeting and a contact has to be updated accordingly. Also, a meeting cancellation or invite email has to sent to the contact's email. This is similar to meeting scheduled, updated and cancelled notifications for all participants. This scenario can only be handled with SQL triggers and scheduled jobs.  
Similarly, other important considerations or enhancements may include:    
1. Handling time conflicting meetings  
2. Acknowledgement from attendees
3. Tracking the session length and who all attended a meeting 
4. Keeping an account of the highlights  
5. Summary in follow up email to attendees and stakeholders    
6. Taking care of time zone differences   

Now, let's consider the design for email notification of a meeting. Meetings scheduled can also be viewed through a web interface that lists all past, ongoing and scheduled  meetings with controlled access. This can be done through a report in a tabular format where individual records can be edited through a form. But the common way meetings are communicated today is through email. These emails can be deleted at any time by the invitees or organizers. So let's say our application will have a record of all meetings and only an admin can delete any of them.

So we have a **meeting email** entity that is created once a meeting is scheduled or updated. Parameters specific to an email is stored in the corresponding table. An email can also have multiple attachments. The one-to-one record of an attachment and a meeting email is stored in the **meeting email attachments** table.  


The possible modifications in the meetings table that will trigger an update email notification are:   
1. Change in recipients (sent to specific invitees)
2. Change in date and time 
3. Change in the meeting title 
4. Adding or modifying the meeting note 
5. Adding attachments
6. Cancelling a meeting 

### The use of automation and AI components ###
Actions on a meeting can be automated through triggers and jobs as mentioned above. However, there are other parameters that can create a better experience for the users through the use of AI. The meeting email for instance, has user-entered text sections for the subject and note. These can be generated or refined using AI tools to precisely highlight the purpose and topics to be covered in the discussion and other details like the passcode to be used for joining online or in the metaverse. The meeting organizer might want the attendees to read some text material or go through an illustration in the form of an image or a video for the best use of the meeting time. Providing a summary of the attachments will attract their attention and motivate them to spend some time preparing for the meeting. 

The same concept can be used for sending out follow-up emails as was stated in the possible enhancements section. The `highlights` field of a meeting can store the summary derived from the transcript which is a type of meeting attachement. An email therefore can be a meeting scheduled, updated or cancelled notification or it can be an automated follow-up for the attendees and stakeholders or all invitees. So, just as we have a `purpose` for a meeting that can have the values `interview` or `consultation`, we need to have an `email_type` field for a meeting email that can also hold the value `follow-up` apart from `creation`, `updation` and `cancellation`. Creating an follow-up email is an advanced feature that requires additional APIs for speech to text conversion. The application should also provide an interface as simple as a form, for the organizer to edit the contents of the email before sending it out.

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

CREATE TABLE `meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_code` varchar(255) NOT NULL, `title` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `purpose` varchar(255) DEFAULT NULL, `location` varchar(255) NOT NULL, `meeting_note` varchar(255) DEFAULT NULL, `email_id` varchar(255) DEFAULT NULL, `meeting_passcode` varchar(255) DEAFULT NULL, `meeting_highlights` varchar(255) DEFAULT NULL, `soft_delete` tinyint(1) DEFAULT '0', PRIMARY KEY (`id`) )

### Meeting Email table ###

CREATE TABLE `meeting_email` ( `id` int NOT NULL AUTO_INCREMENT, `email_type` varchar(255) NOT NULL, `from_email` varchar(255) NOT NULL, `attachements_id` varchar(255) DEFAULT NULL, `meeting_id` int NOT NULL, PRIMARY KEY (`id`) )

### Meeting Email Attachments table ###

CREATE TABLE `meeting_email_attachments` ( `id` int NOT NULL AUTO_INCREMENT, `attachment_link` varchar(255) NOT NULL, `email_id` int DEFAULT NULL, PRIMARY KEY (`id`) )
    
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

## Examples of commonly used SQL Triggers in Cliosight's syntax ##







