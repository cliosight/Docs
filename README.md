# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

## SQL interface for structured data ##
Cliosight will support the top database servers like MySQL, Postgres and MS SQL. Also AWS Dynamo DB, Azure Cosmos DB and other cloud services like Google BigQuery.
With our SQL interface, users can perform all possible analytical operations for both in-house and user-owned databases. We will also expand to other emerging data sources like distributed ledgers in future releases.


## Support for multiple data sources ##
Paid users will have the option to use multiple data sources by saving the configurations for each. These can be created for the account's in-house databases or that on the user's virtual machines or for cloud database instances. Free tier will only have one in-house MySQL database with a shared connection pool. Uploading large volumes of data will be much faster for paid users due to dedicated resources.
   
## User permissions and access control ##
Each component created using Cliosight will have fine-grained access control. The admin will provide necessary permissions to users so that data can be uploaded, viewed or edited in a controlled manner. For instance, restricting access to datasets and reports based on the geographic location of users. Similarly, the admin can specify users who can create or execute triggers and workflows for actions on data and insights. A simple use case for this is geographically targeted online marketing campaigns. Same applies for files like images and PDFs that are stored in the cloud storage. Trial accounts will have limited database and file storage capacity. 

## File upload ##
A form can be a way to attach files associated with an entity. A text, image, video or any other file uploaded through a form will be stored in the cloud storage such that the link can be used to access that resource. 

## A relational database example ##
Let's consider a database design for company meetings. A **meeting** can be either an **interview** or a **consultation**. A meeting can be sent to individuals or a **group** of individuals whose **contact** information is already saved in the database. Also, pre-existing contacts can be added to the meeting explicitly. A meeting, group and contact can be edited at any time. In order to track the individuals who were invited for a meeting, there has to be a direct association between the meeting and a contact. This is because a group might be edited after a meeting has taken place. Alternately, if a contact is removed or added to a group in a meeting, after it is scheduled (created) but prior to the start time, then the table connecting a meeting and a contact has to be updated accordingly. Also, a meeting cancellation or invite email has to sent to the contact. This is similar to meeting scheduled, updated and cancelled notifications for all participants. This scenario can only be handled with SQL triggers and scheduled jobs.  
Similarly, other important considerations or enhancements may include:    
1. Handling time conflicting meetings  
2. Acknowledgement from attendees
3. Tracking the session length and who all attended a meeting 
4. Keeping an account of the highlights  
5. Summary in follow up email to attendees and stakeholders    
6. Taking care of time zone differences   

Now, let's consider the database design for email notifications related to meetings. Meetings can be managed through a web interface with controlled access, where all past, ongoing, and scheduled meetings are listed. This can be achieved using a report widget with a tabular format that allows individual records to be edited through a form. However, the common way meetings are communicated to participants today is through email. It's important to note that these emails can be deleted at any time from the inbox by the invitees and organizers. To address this, our application will maintain a record of all meetings, and only an admin will have the authority to delete any of them.

To implement this functionality, we introduce an email entity that is instantiated once a meeting is scheduled or updated. Parameters specific to an email are stored in the corresponding table. Additionally, an email can have multiple attachments, and the one-to-one relationship between an attachment and an email is stored in the email attachments table.

One challenge to consider is that a contact might belong to multiple groups. When an email is sent to several groups and individual contacts, we need to ensure that there is no redundancy of email addresses, and recipients do not receive duplicate emails.

The modifications in the meetings table that will trigger an update email notification include:

    Change in recipients (sent to specific invitees)
    Change in date and time
    Change in the meeting title
    Adding or modifying a meeting note
    Adding attachments
    Cancelling a meeting

 ## Leveraging Automation and AI Components  ##

While some meeting actions can be automated through triggers, there are other aspects that can provide a better user experience through the use of AI. For example, the content of meeting emails, such as the subject and note, can be generated using AI tools to precisely highlight the purpose and topics to be covered in the discussion. Essential details like hyperlinks and passcodes for joining online meetings, especially in hybrid scenarios with both online and offline attendees, should be clearly stated by the organizer.

To enhance attendee interaction, the organizer may want to provide text materials, images, or videos relevant to the meeting's topics. Including a summary of the attachments in the email will attract their attention and motivate them to spend some time preparing for the meeting.

The concept of leveraging AI and automation can also be applied to sending follow-up emails, as mentioned in the possible enhancements section. The highlights field of a meeting can store a summary derived from the transcript or whiteboard, which are a type of meeting attachment. Therefore, an email can serve as an automated follow-up for attendees, stakeholders, or all invitees. To support this, we can introduce an `email_type` field for an email, which can hold values such as `follow-up`, `creation`, `updation`, or `cancellation`. The application should also provide a simple interface, such as a form, for the organizer to edit the contents of the email before sending it out.

Creating a follow-up email is an advanced feature that requires additional APIs for speech-to-text conversion in the video conferencing solution. However, note-taking facilities are already provided by software like Etherpad. Integration with open-source collaboration applications like Jitsi Meet, which has Etherpad already integrated, can be considered by an application developer. Storing the values back into the database would require another API integration.    

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

CREATE TABLE `meetings` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_code` varchar(255) NOT NULL, `title` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `purpose` varchar(255) DEFAULT NULL, `link` varchar(255) DEFAULT NULL, `location` varchar(255) DEFAULT NULL, `meeting_note` varchar(255) DEFAULT NULL, `email_id` varchar(255) DEFAULT NULL, `meeting_passcode` varchar(255) DEAFULT NULL, `meeting_highlights` varchar(255) DEFAULT NULL, `soft_delete` tinyint(1) DEFAULT '0', PRIMARY KEY (`id`) )

### Meeting Email table ###

CREATE TABLE `meeting_email` ( `id` int NOT NULL AUTO_INCREMENT, `email_type` varchar(255) NOT NULL, `from_email` varchar(255) NOT NULL, `attachements_id` int DEFAULT NULL, `meeting_id` int NOT NULL, PRIMARY KEY (`id`) )

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







