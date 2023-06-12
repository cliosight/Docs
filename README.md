# Cliosight Documentation
A detailed explanation for creating and managing tables, forms, reports, dashboards and triggers. 

# Table of contents
1. [SQL interface for structured data](#sql)   
2. [Support for multiple data sources](#datasources)   
3. [File upload](#file)   
4. [User permissions and access control](#acl)   
5. [A Relational Database Example](#example)   
6. [Leveraging Automation and AI Components](#ai)   
7. [Schema details of the Example](#schema)   
8. [JSON body a Form](#form)    
9. Examples of Forms          
	1. [Customer Queries Form](#cq)     
	2. [Section of the Meetings Form](#meetings_form)      
10. [JSON body of a Report](#report)   
11. Examples of Reports in Cliosight          
	1. [All Meetings Report](#meetings_report)     
	2. [Group Meetings Report](#meetings_group)     
12. [Graphs and Charts with Cliosight Reports](#graphs)    
13. [JSON body a Reporting Dashboard](#dashboard)     
14. [Example of a Reporting Dashboard](#example_dashboard)     
15. [Claiming Trustworthiness](#trust)    
16. [JSON body of a Trigger](#trigger)  
17. Examples of Reports in Cliosight          
	1. [Managing an SCD (Slowly Changing Dimension)](#scd)     
	2. [Sending Email Notifications on Events](#trigger_email)    
18. [Email Notification](#email)   
19. [Workflows](#workflow)   

## SQL interface for structured data <a name="sql"></a>
Cliosight is a robust platform that offers support for various leading database servers, including MySQL, Postgres, and MS SQL. Additionally, it seamlessly integrates with popular cloud services such as AWS Dynamo DB, Azure Cosmos DB, and Google BigQuery. Our SQL interface empowers users to perform a wide range of analytical operations, encompassing both in-house and user-owned databases. As we continue to evolve, we plan to expand our compatibility to encompass emerging data sources, including distributed ledgers, in future releases.


## Support for multiple data sources <a name="datasources"></a>
Paid users of our platform will enjoy the flexibility of utilizing multiple data sources by saving configurations for each source. These configurations can be created for in-house databases within the account, user-owned virtual machines, or cloud database instances. On the other hand, users on the free tier will have access to a **single in-house MySQL database** with a **shared connection pool**.

One of the advantages of being a paid user is the significantly faster upload speed for large volumes of data. This is made possible by dedicated resources allocated specifically for paid accounts, ensuring efficient data transfer and processing.    

Example of a datasource definition:      
```json     
{
	"client_id": 2,
	"datasource_definition": {
    	"type": "mysql",
    	"inhouse": true,
    	"connectionLimit": 12,
    	"host": "localhost",
    	"port": 3306,
    	"user": "user1",
    	"password": "password1",
    	"database": "database1",
    	"multipleStatements": true,
    	"label": "Cliosight Mysql"
	}
}
```    

## File upload <a name="file"></a>
A form can be a way to attach files associated with an entity. A text, image, video or any other type of file uploaded through a form will be stored in the cloud storage such that a URL can be used to access that resource. 
   
## User permissions and access control <a name="acl"></a>
Cliosight ensures that each component created within its platform incorporates fine-grained access control. Administrators can grant specific permissions to users, enabling controlled actions such as data upload, viewing, and editing. An example of this control is restricting access to datasets and reports based on the geographic location of users. Furthermore, administrators can designate users with the ability to create and execute triggers and workflows for data and insights. This functionality proves especially useful in geographically targeted online marketing campaigns. Additionally, the same access restrictions apply to files stored in cloud storage. As for trial accounts, they are allocated a limited capacity for database and file storage.

## A relational database example <a name="example"></a>
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

One challenge to consider is that a contact might belong to multiple groups. When an email is sent to several groups and individual contacts, we need to ensure that there is no redundancy of email addresses, and recipients do not receive duplicate emails.

The modifications in the meetings table that will trigger an update email notification include:
1. Change in recipients (sent to specific invitees)
2. Change in date and time
3. Change in the meeting title
4. Adding or modifying a meeting note
5. Adding attachments
6. Cancelling a meeting

 ## Leveraging Automation and AI Components <a name="ai"></a>

While some meeting actions can be automated through triggers, there are other aspects that can provide a better user experience through the use of AI. 
1. SQL query generator   
2. JSON body generator     

These are the only two syntaxes used in Cliosight. Anyone with limited knowledge of SQL or wanting to build an application faster can seek help from the AI agent. 
    
As far this example is concerned, we can utilize another set of APIs in specific components within Cliosight. For example, the content of meeting emails, such as the subject and note, can be generated using AI tools to precisely highlight the purpose and topics to be covered in the discussion. Essential details like the exact location or the meeting link with passcode for joining online, especially in hybrid scenarios with both online and offline attendees, should be clearly stated by the organizer. An online meeting can be organized through some self-hosted video calling application or some commonly used enterprise collaboration software like Zoom, Google Meet and Microsoft Teams, that will return a link created dynamically through an API call.   

To enhance attendee interaction, the organizer may want to provide text materials, images, or videos relevant to the subject. Including a summary of the attachments in the email will attract their attention and motivate them to spend some time preparing for the meeting.

The concept of leveraging AI and automation can also be applied to sending follow-up emails, as mentioned in the possible enhancements section. The highlights field of a meeting can store a summary derived from the transcript or whiteboard, which are a type of meeting attachment. The same can also be entered manually later by the users who have edit access to the report listing all meeting details.   
Therefore, an email can also serve as an automated follow-up for attendees, stakeholders, or all invitees. To support this, we can introduce an `email_type` field for the email entity, which can hold the values `follow-up`, `creation`, `updation`, or `cancellation`. The application should also provide a simple interface, such as a form, for the organizer to edit the contents of the email before sending it out.

Creating a follow-up email is an optional feature that necessitates additional APIs for speech-to-text conversion in the video conferencing solution. However, note-taking functionality is already available in software like Etherpad. Application developers can consider integrating with open-source collaboration applications such as Jitsi Meet, which already includes Etherpad. To store the values back into the database, another API integration would be required.    

The forms created are given below:   
[Create a Contact](https://demo.cliosight.com/app/forms/35/show?noNavbar=true){:target="_blank"}    
[Create a Group](https://demo.cliosight.com/app/forms/34/show?noNavbar=true){:target="_blank"}    
[Schedule a Meeting](https://demo.cliosight.com/app/forms/52/show?noNavbar=true){:target="_blank"}   
    
## Schema details <a name="schema"></a>

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
     
   
## JSON body a Form <a name="form"></a>
```css
{   
   "datasource_id": <int-datasource-id>,   
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
            "default_instances_count": 1, // Default sub form items to show, useful when the add sub form button is hidden       
            "hide_instance_remove_button": true, // Disable removing existing sub form rows by hiding the close button; This is useful along with hide sub form button enabled     
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
                "inputs": [...], // sub form inputs defined here; Same as the parent form input conventions     
                "unique_keys": [“<column-name>”] // Array of unique key columns of the sub form root table can be specified here     
	}		     
    }],     
    "last_insert_id_key": "<column-name>", // specify this column if its value is returned as the lastInsertId value; Example, auto_increment key in mysql.     
    "unique_keys":  ["<column-name>"] // Array of unique key columns of the main form root table can be specified here      
    "submit_button_label": <button-label>  // Default value is "Submit"     
    }        
} 
```   
      

## Examples of Forms ##   
### Customer Queries <a name="cq"></a>     

![form_example](https://file.io/hUJZ7yyMbGFS)  
```
CREATE TABLE `customer_queries` ( `id` int NOT NULL AUTO_INCREMENT, `post_body` text, `email` varchar(255) DEFAULT NULL, `fullname` varchar(255) DEFAULT NULL, `subject` varchar(255) DEFAULT NULL, `soft_delete` tinyint(1) DEFAULT '0', PRIMARY KEY (`id`) )   
```   

```css
{
	"client_id": 2,    // Optional, added from backend according to the user id
	"datasource_id": 1, // Set to the datasource ID
	"table": {
    	"name": "customer_queries"  // The root table created in the datasource
	},
	"sub_form_definition": {
    	"is_public": {
        	"status": true  // Whether or not this form is accessible without login
    	},
    	"css_definition": "",  // The uglified CSS definition
        "inputs": [{  // start of the input fields of the form
        	"input_category": "field", // Since this is a text input field
        	"column": {
            	"Field": "subject"  // Column name in the database table schema
        	},
        	"input_type": "text",  // Input type for single line text 
        	"placeholder": "Message title",  // Optional; needed if input_label is not used
	        “Input_label”: “”, // Optional 
        	"validation": {
            	"isRequired": "1", // 1 for an error message if left blank. 0 otherwise.
            	"maxLength": 255   // According to the length of the field
        	}
    	     }, {  // Repeat the same for other fields
        	"input_category": "field",
        	"column": {
            	"Field": "post_body"
        	},
        	"input_type": "textarea",
        	"placeholder": "Your questions or comments",
        	"validation": {
            	"isRequired": "1", 
            	"maxLength": 255
        	}
    	     }, {
        	"cols": 6,    // Optional field to specify the columns in a row. 12 by default
        	"input_category": "field",
        	"column": {
            	"Field": "fullname"
        	},
        	"input_type": "text",
        	"placeholder": "Your full name",
        	"validation": {
            	"isRequired": "1",
            	"maxLength": 255
        	}
    	     }, {
        	"cols": 6,
        	"input_category": "field",
        	"column": {
            	"Field": "email"
        	},
        	"input_type": "text",
        	"placeholder": "Your email",
        	"validation": {
            	"isRequired": "1",
            	"maxLength": 255
        	}}],
    	"last_insert_key_id": "id", // Specify this column if its value is returned as the lastInsertId value; For example, auto_increment key in mysql
    	"unique_keys": ["id"], // Array of unique key columns of the main form root table can be specified here
    	"submit_button_label": "Send Message", // Default value is “Submit”
    	"label": "Contact Us" // Optional; default value is “Form”
	} 
}
```   

### A section of the Meetings form <a name="meetings_form"></a>     
Major components are:   
1. Multiselect input   
2. Drop down menu with hardcoded values   
3. Rules based on the selection made in the drop down menu  
4. Sub form within the main form 
5. Multiple instances of aggregated values   

![form_example_1](https://file.io/CGl3yMUZemLi)   
   ```css    
   {
	"datasource_id": 1,
	"table": {
    	"name": "meeting"
	},
	"sub_form_definition": {
    	"is_public": {
        	"status": true
    	},
         "css_definition": "@import url('https://fonts.googleapis.com/css?family=Raleway');.form{background: linear-gradient(45deg, rgba(0,0,0,0), rgba(29, 200, 205, 0));}.form{align-items: center; margin: 0%;}.form {font-family: Raleway; color: grey; }.btn{background: linear-gradient(45deg, rgba(29, 224, 153, 0.8), rgba(29, 200, 205, 0.8));border-radius: 30px; border: 0px solid white; height:50px; width:100%; margin: 0px; margin-bottom: 10px;}.btn:hover{background:grey; color: white; border: 2px solid grey; border-radius: 30px;}input[type=text] {box-sizing: border-box; border: 2px solid #ccc; height:50px !important;  border: 2px solid grey; }input[type=text]:focus {border: 2px solid #555;} .form-control {border: 2px solid grey; height: 50px; margin-bottom: 0px; background-color: white;} .select2-choices {border: 2px solid grey !important;  border-radius: 5px !important; box-shadow: 5px 10px inset white !important; height: 100px !important} p {background: linear-gradient(45deg, rgba(29, 224, 153, 0.8), rgba(29, 200, 205, 0.8));} .border {border: 2px solid green !important}",
	 "inputs": [{
        	"cols": 9,
        	"input_category": "multiselect",
        	"column": {
            	"Field": "contact_id"
        	},
        	"input_type": "",
        	"input_label": "Assign Contacts",
        	"table_select_table": {
            	"name": "contact"
        	},
        	"table_select_search_column": "email",
        	"table_select_join_column": "id",
        	"sub_form_button_label": "",
        	"sub_form_table": {
            	"name": "contact_meeting"
        	},
        	"join_criteria_main_table_column": {
            	"Field": "id"
        	},
        	"join_criteria_sub_table_column": {
            	"Field": "meeting_id"
        	}
    	},{
        	"cols": 4,
        	"input_category": "field",
        	"column": {
            	"Field": "purpose"
        	},
        	"input_type": "select",
        	"input_label": "Enter the meeting's purpose",
        	"placeholder": "Purpose",
        	"options": [{
            	"label": "Select Purpose",
            	"value": ""
        	}, {
            	"label": "Interview",
            	"value": "interview"
        	}, {
            	"label": "Consultation",
            	"value": "consultation"
        	}]
    	}, {
        	"rules": [{
            	"column": "purpose",
            	"value": "interview"
        	}],
        	"action": "show",
        	"show": false,
        	"cols": 9,
        	"input_category": "form",
        	"input_label": "Interview Details",
        	"sub_form_button_label": "Add Interview Details",
        	"hide_sub_form_button": false,
        	"default_instances_count": 1, 
        	"hide_instance_remove_button": false,
        	"sub_form_table": {
            	"name": "interview"
        	},
        	"join_criteria_main_table_column": {
            	"Field": "id"
        	},
        	"join_criteria_sub_table_column": {
            	"Field": "meeting_id"
        	},
        	"sub_form_definition": {
            	"inputs": [{
                	"cols": 6,
                	"input_category": "field",
                	"column": {
                    	"Field": "skill"
                	},
                	"input_type": "select",
                	"input_label": "Choose a skill",
                	"options": [{
                    	"label": "UI/UX",
                    	"value": "uiux"
                	}, {
                    	"label": "Full-stack dev",
                    	"value": "fullstack"
                	}],
                	"table_select_table": {
                    	"name": ""
                	},
                	"table_select_search_column": "",
                	"table_select_join_column": "",
                	"value": ""
            	}, {
                	"cols": 6,
                	"input_category": "field",
                	"column": {
                    	"Field": "experience"
                	},
                	"input_type": "select",
                	"input_label": "Choose the experience level",
                	"options": [{
                    	"label": "Entry-Level",
                    	"value": "entry"
                	}, {
                    	"label": "Mid-Level",
                    	"value": "mid"
                	}, {
                    	"label": "Senior-Level",
                    	"value": "senior"
                	}],
                	"table_select_table": {
                    	"name": ""
                	},
                	"table_select_search_column": "",
                	"table_select_join_column": "",
                	"value": ""
            	}],
            	"unique_keys": ["id"]
        	}
    	}, {
        	"rules": [{
            	"column": "purpose",
            	"value": "consultation"
        	}],
        	"action": "show",
        	"show": false,
        	"cols": 9,
        	"input_category": "form",
        	"input_label": "Consultation Details",
        	"sub_form_button_label": "Add Consultation Details",
        	"hide_sub_form_button": false,
        	"default_instances_count": 1,
        	"hide_instance_remove_button": false,
        	"sub_form_table": {
            	"name": "consultation"
        	},
        	"join_criteria_main_table_column": {
            	"Field": "id"
        	},
        	"join_criteria_sub_table_column": {
            	"Field": "meeting_id"
        	},
        	"sub_form_definition": {
            	"inputs": [{
                	"cols": 6,
                	"input_category": "field",
                	"column": {
                    	"Field": "module"
                	},
                	"input_type": "select",
                	"input_label": "Choose a module",
                	"options": [{
                    	"label": "Forms",
                    	"value": "forms"
                	}, {
                    	"label": "Reports",
                    	"value": "reports"
                	}],
                	"table_select_table": {
                    	"name": ""
                	},
                	"table_select_search_column": "",
                	"table_select_join_column": "",
                	"value": ""
            	}, {
                	"cols": 6,
                	"input_category": "field",
                	"column": {
                    	"Field": "category"
                	},
                	"input_type": "select",
                	"input_label": "Choose the category",
                	"options": [{
                    	"label": "Technical",
                    	"value": "technical"
                	}, {
                    	"label": "Functional",
                    	"value": "functional"
                	}],
                	"table_select_table": {
                    	"name": ""
                	},
                	"table_select_search_column": "",
                	"table_select_join_column": "",
                	"value": ""
            	}],
            	"unique_keys": ["id"]
        	}
    	}],
    	"last_insert_id_key": "id",
    	"unique_keys": ["meeting_code"],
    	"label": "Meetings",
    	"submit_button_label": "Save Meeting"
	}
}
```

## JSON body a Report <a name="report"></a>

## Examples of Reports ##
### All Meetings Report <a name="meetings_report"></a>

### Group Meetings Report <a name="meetings_group"></a>


## Creating Graphs and Charts with JavaScript libraries and Cliosight Reports <a name="graphs"></a>



## JSON body of a Reporting Dashboard  <a name="dashboard"></a>

## Example of a Reporting Dashboard <a name="example_dashboard"></a>
  
## Claiming Trustworthiness <a name="trust"></a>


## JSON body of a Trigger <a name="trigger"></a>

## Examples of commonly used SQL Triggers ##
### Managing an SCD (Slowly Changing Dimension) <a name="scd"></a>

### Sending Email Notifications on Events <a name="trigger_email"></a>

## Email Notification <a name="email"></a>

## Workflows <a name="workflow"></a>






