# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

## SQL interface for structured data ##
Cliosight is a robust platform that offers support for various leading database servers, including MySQL, Postgres, and MS SQL. Additionally, it seamlessly integrates with popular cloud services such as AWS Dynamo DB, Azure Cosmos DB, and Google BigQuery. Our SQL interface empowers users to perform a wide range of analytical operations, encompassing both in-house and user-owned databases. As we continue to evolve, we plan to expand our compatibility to encompass emerging data sources, including distributed ledgers, in future releases.


## Support for multiple data sources ##
Paid users of our platform will enjoy the flexibility of utilizing multiple data sources by saving configurations for each source. These configurations can be created for in-house databases within the account, user-owned virtual machines, or cloud database instances. On the other hand, users on the free tier will have access to a **single in-house MySQL database** with a **shared connection pool**.

One of the advantages of being a paid user is the significantly faster upload speed for large volumes of data. This is made possible by dedicated resources allocated specifically for paid accounts, ensuring efficient data transfer and processing.

## File upload ##
A form can be a way to attach files associated with an entity. A text, image, video or any other type of file uploaded through a form will be stored in the cloud storage such that a URL can be used to access that resource. 
   
## User permissions and access control ##
Cliosight ensures that each component created within its platform incorporates fine-grained access control. Administrators can grant specific permissions to users, enabling controlled actions such as data upload, viewing, and editing. An example of this control is restricting access to datasets and reports based on the geographic location of users. Furthermore, administrators can designate users with the ability to create and execute triggers and workflows for data and insights. This functionality proves especially useful in geographically targeted online marketing campaigns. Additionally, the same access restrictions apply to files stored in cloud storage. As for trial accounts, they are allocated a limited capacity for database and file storage.

## A relational database example ##
Let's explore the database design for corporate meetings. A **meeting** in our design can be categorized as either an **interview** or a **consultation**. It can be sent to individuals or a **group** of individuals whose **contact** information is already stored in the database. Additionally, existing contacts can be explicitly added to a meeting. It's important to note that meetings, groups, and contacts can be edited at any time.

To accurately track the individuals invited to a meeting, we need a direct association between the meeting and a contact. This is necessary because a group may be edited after a meeting has already taken place. Conversely, if a contact is added or removed from a group in a meeting, after the meeting is scheduled (created) but before the start time, the table linking the meeting and contact must be updated accordingly. Furthermore, when such changes occur, a meeting invite or cancellation email should be sent to the contact's email address. Similar notifications regarding meeting scheduling, updates, and cancellations need to be sent to all participants. Achieving this scenario requires the utilization of SQL triggers and scheduled jobs for so that our system can handle the dynamic nature of meeting invitations, updates, and cancellations. 

Similarly, other important considerations or enhancements may include:    
1. Handling time conflicting meetings  
2. Acknowledgement from attendees
3. Tracking the session length and who all attended a meeting 
4. Keeping an account of the highlights  
5. Summary in follow up email to attendees and stakeholders    
6. Taking care of time zone differences   

Now, let's consider the database design for email notifications. Meetings can be managed through a web interface with controlled access, where all past, ongoing, and scheduled meetings are listed. This can be achieved using a reporting widget with a tabular format that allows individual records to be edited through a form. However, the common way meetings are communicated to participants today is through email. It is important to note that these emails can be deleted at any time from the inbox by the invitees and organizers. To address this, our application will maintain a record of all meetings, and only an admin will have the authority to delete any of them.

To implement this functionality, we introduce an **email** entity that is instantiated once a meeting is scheduled or updated. Parameters specific to an email are stored in the corresponding table. Additionally, an email can have multiple **attachments**, and the one-to-one relationship between an attachment and an email is stored in a separate table.

One challenge to consider is that a contact might belong to multiple groups. When an email is sent to several groups and individual contacts, we need to ensure that there is **no redundancy** of email addresses, and recipients do not receive duplicate emails.

The modifications in the meetings table that will trigger an update email notification include:
1. Change in recipients (sent to specific invitees)
2. Change in date and time
3. Change in the meeting title
4. Adding or modifying a meeting note
5. Adding attachments
6. Cancelling a meeting

 ## Leveraging Automation and AI Components  ##

While some meeting actions can be automated through triggers, there are other aspects that can provide a better user experience through the use of AI. For example, the content of meeting emails, such as the subject and note, can be generated using AI tools to precisely highlight the purpose and topics to be covered in the discussion. Essential details like the exact location or the meeting link with passcode for joining online, especially in hybrid scenarios with both online and offline attendees, should be clearly stated by the organizer. An online meeting can be organized through some self-hosted video calling application or some commonly used enterprise collaboration software like Zoom, Google Meet and Microsoft Teams, that will return a link created dynamically through an API call.   

To enhance attendee interaction, the organizer may want to provide text materials, images, or videos relevant to the subject. Including a summary of the attachments in the email will attract their attention and motivate them to spend some time preparing for the meeting.

The concept of leveraging AI and automation can also be applied to sending follow-up emails, as mentioned in the possible enhancements section. The highlights field of a meeting can store a summary derived from the transcript or whiteboard, which are a type of meeting attachment. The same can also be entered manually later by the users who have edit access to the report listing all meeting details.   
Therefore, an email can also serve as an automated follow-up for attendees, stakeholders, or all invitees. To support this, we can introduce an `email_type` field for the email entity, which can hold the values `follow-up`, `creation`, `updation`, or `cancellation`. The application should also provide a simple interface, such as a form, for the organizer to edit the contents of the email before sending it out.

Creating a follow-up email is an optional feature that necessitates additional APIs for speech-to-text conversion in the video conferencing solution. However, note-taking functionality is already available in software like Etherpad. Application developers can consider integrating with open-source collaboration applications such as Jitsi Meet, which already includes Etherpad. To store the values back into the database, another API integration would be required.    

The forms created are given below:   
[Create a Contact](https://demo.cliosight.com/app/forms/35/show?noNavbar=true)  
[Create a Group](https://demo.cliosight.com/app/forms/34/show?noNavbar=true)   
[Schedule a Meeting](https://demo.cliosight.com/app/forms/52/show?noNavbar=true)  
    
## Schema details ##

### Contact table ###

CREATE TABLE `contact` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) DEFAULT NULL, `email` varchar(255) NOT NULL, `phone` varchar(255) DEFAULT NULL, `type` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )     

### Group table ###

CREATE TABLE `group` ( `id` int NOT NULL AUTO_INCREMENT, `name` varchar(255) NOT NULL, `admin_email` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )
      
### Group and Contact Association ###

CREATE TABLE `group_contact` ( `id` int NOT NULL AUTO_INCREMENT, `contact_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) )    

### Meeting table ###

CREATE TABLE `meeting` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_code` varchar(255) NOT NULL, `title` varchar(255) DEFAULT NULL, `start_date_time` varchar(255) DEFAULT NULL, `purpose` varchar(255) DEFAULT NULL, `link` varchar(255) DEFAULT NULL, `location` varchar(255) DEFAULT NULL, `meeting_note` varchar(255) DEFAULT NULL, `meeting_passcode` varchar(255) DEAFULT NULL, `meeting_highlights` varchar(255) DEFAULT NULL, `soft_delete` tinyint(1) DEFAULT '0', PRIMARY KEY (`id`) )

### Meeting Email table ###

CREATE TABLE `meeting_email` ( `id` int NOT NULL AUTO_INCREMENT, `email_type` varchar(255) NOT NULL, `from_email` varchar(255) NOT NULL, `attachement_id` int DEFAULT NULL, `meeting_id` int NOT NULL, PRIMARY KEY (`id`) )

### Meeting Attachment table ###

CREATE TABLE `meeting_attachment` ( `id` int NOT NULL AUTO_INCREMENT, `attachment_link` varchar(255) NOT NULL, `meeting_email_id` int DEFAULT NULL, PRIMARY KEY (`id`) )
    
### Interview table ###

CREATE TABLE `interview` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `skill` varchar(255) DEFAULT NULL, `experience` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) ) 
       
### Consultation table ###

CREATE TABLE `consultation` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `module` varchar(255) DEFAULT NULL, `category` varchar(255) DEFAULT NULL, PRIMARY KEY (`id`) )    
      
    
### Group and Meeting Association ###

CREATE TABLE `group_meeting` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `group_id` int NOT NULL, PRIMARY KEY (`id`) ) 

### Contact and Meeting Association ###

CREATE TABLE `contact_meeting` ( `id` int NOT NULL AUTO_INCREMENT, `meeting_id` int NOT NULL, `contact_id` int NOT NULL, PRIMARY KEY (`id`) ) 
     
   

## JSON fields of a Form ##

{
   "datasource_id": <int value of the datasource> 
    "table": {
        "name": "<main-table-name>" // This is the root table of the form
    },
   "sub_form_definition": {
        "is_public": {
            "status": <boolean> // Indicates if the form is available without login
        },
"css_definition": "", // css for the form
        "inputs": [{
            "cols": <values 1-12>, //Sets column width in the page; This is optional
            "rules": [{ // show/hide current input based on the rules and action properties
                "column": "<column-name>", // Matching column[‘Field’] value of a sibling input
                "value": "<input-value>" // If value entered or selected matches the specified value
            }],
            "action": "show",
            "show": <true or false>, // default action value is false
            "input_category": "field | multiselect | form | report",  // form is for supporting sub-form definition; 
            "column": {
                "Field": "<column-name>"  // column of the main table or the polulated values via "multiselect" in input category
            },
            "input_type": "text | select | radio | checkbox | hidden", // checkbox can be used with multiselect as well
            "placeholder": "", // input placeholder attribute
            "input_label": "", // label element content goes here
            "options": [{      // required for select tag options; also applies to multiselect checkboxes
                "label": "",  
                "value": ""
            }],
            "table_select_table": “<table-name>”, // for single/multiselect dropdown where the options are from another table in database
            "table_select_search_column": "<column-name>", // column name of the table values in multiselect
            "table_select_join_column": "<column-name>", // the column name of the main table that is used to join with the search table
            "validation": {
                "isRequired": "0 | 1", // "1" if mandatory; throws an error if user does not enter the value and clicks submit
                "maxLength": 255
            },
            "sub_form_button_label": "", // This is the add sub form button label
            "hide_sub_form_button": true, // Do not show add sub form button
            "default_instances_count": 1, // Default sub form items to show, useful when the add sub form button is hidden.
            "hide_instance_remove_button": true, // Disable removing existing sub form rows by hiding the close button; This is useful along with hide sub form button enabled.
            "sub_form_table": {
                "name": "<table-name>" // sub form root table
            },
            "join_criteria_main_table_column": { // parent form root table join column
                "Field": "<column-name>"
            },
            "join_criteria_sub_table_column": { // sub form root table join column
                "Field": "<column-name>"
            },
            "sub_form_definition": {
                "inputs": [...], // sub form inputs defined here; Same as the parent form input conventions.
                "unique_keys": [“<column-name>”] // Array of unique key columns of the sub form root table can be specified here
}		
       
 }],
        "last_insert_id_key": "<column-name>", // specify this column if its value is returned as the lastInsertId value; Example, auto_increment key in mysql.
        "unique_keys":  ["<column-name>"] // Array of unique key columns of the main form root table can be specified here
	     "submit_button_label": <button-label>  // Default value is "Submit"
    }
   
}



## Examples of Forms in Cliosight's syntax ##
   
   
  

## JSON fields of a Report ##

## Examples of Reports in Cliosight's syntax ##

## Using reports' data for plotting graphs using various JavaScript libraries ##



## JSON fields of a Dashboard ##

## Examples of Dashboards in Cliosight's syntax ##
  
## Claiming trustworthiness 


## JSON fields of a Trigger ##

## Examples of commonly used SQL Triggers in Cliosight's syntax ##







