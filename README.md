# Cliosight Documentation
![cliosight](https://miro.medium.com/v2/resize:fit:720/format:webp/1*HsRqxET_JE0b8kRq3P4OOg.png)  
A detailed explanation of datasources, forms, reports, dashboards, triggers and workflows.   
Please contact us at jigisha@cliosight.com or info@cliosight.com.

# Table of contents
[SQL Interface for Structured Data](#sql)   
[Support for Multiple Data Sources](#datasources)   
[User Permissions and Access Control](#acl)   
[JSON body of a Form](#form)              
[JSON body of a Report](#report)     
[Graphs and Charts with Cliosight Reports](#graphs)    
[Using Reports in Jupyter Notebook](#jupyter)
[JSON body of a Reporting Dashboard](#dashboard)      
[Ensuring Trustworthiness](#trust)    
[JSON body of a Trigger](#trigger)    
[Email Notification](#email)   
[Workflows](#workflow) 
[Examples of Workflows](#workflowexamples)   
[Creating Workflows in Jupyter Notebook](#python)    

## SQL Interface for Structured Data <a name="sql"></a>
Cliosight is a robust platform that offers support for various leading database servers, including MySQL, Postgres, and Microsoft SQL server. Our SQL interface enables users to perform a wide range of analytical operations, encompassing both in-house and user-owned databases. As we continue to evolve, we plan to expand our compatibility to encompass emerging data sources, including cloud data storages and distributed ledgers, in future releases.


## Support for Multiple Data Sources <a name="datasources"></a>
Paid users of our platform will be able to add multiple data sources. These are added via configurations that can be created for the in-built databases or those on the user's cloud VM. Cloud database instances like Google Cloud SQL and Azure SQL Server are also supported. Schemas created are segregated based on these datasources. Users on the free tier will have access to **only one** in-built MySQL database with a **shared connection pool**.    

One other advantage of being a paid user is the significantly faster upload speed for large volumes of data. This is made possible by dedicating resources for paid accounts, ensuring efficient data transfer and processing. Data import option provided along with forms will allow uploading bulk data from CSV files. Data from a report can be downloaded or exported to other databases or data sources.       

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
   
## User Permissions and Access Control <a name="acl"></a>
Cliosight ensures that each component created within its platform incorporates fine-grained access control. Administrators can grant specific permissions to users, enabling controlled actions such as data upload, viewing, and editing. An example of this control is restricting access to forms and reports based on the geographic location of users. Executing CRUD SQL queries on the database tables directly is also restricted.      
Furthermore, administrators can designate users with the ability to create and execute triggers and workflows. This functionality proves especially useful in CRM operations like geographically targeted online marketing campaigns. Additionally, access restrictions apply to files stored within the account.     
     
## Leveraging Automation and AI Components <a name="ai"></a>

While some meeting actions can be automated through triggers, there are other aspects that can provide a better user experience through the use of AI.
1. SQL query generator  
2. JSON body generator
3. HTML/CSS code generator      

These are the only two types of syntaxes used in Cliosight for configuring the widgets - SQL and JSON. Apart from this, the "pre-html" and "post-html" tags allow users to embed an HTML inside a form for adding extra elements like images and videos or text and hyperlinks.      
As far as this example is concerned, we can utilize another set of AI related APIs in specific components within Cliosight for a more comprehensive and sophiticated application design.          
  
## JSON body of a Form <a name="form"></a>
A form can be embedded into another application with the help of an https URL. It can have sub-forms, charts, reports and HTML and supports all basic input elements. The URL format of a form is https://demo.cliosight.com/app/forms/serial-number/show?noNavbar=true     
Below are the JSON tags that can be used within a form.   
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
   "css_definition": "", // uglified css for the form      
        "inputs": [{      
            "cols": <values 1-12>, // Sets column width in the page; This is optional; default value is 12      
            "rules": [{ // show/hide current input based on the rules and action properties     
                "column": "<column-name>", // Matching column[‘Field’] value of a sibling input     
                "value": "<input-value>" // If value entered or selected matches the specified value     
            }],     
            "action": "show",    
            "show": true | false, // default action value is false    
            "input_category": "field | multiselect | form",  // form is for supporting sub-form definition;     
            "column": {    
                "Field": "<column-name>"  // column of the main table or the populated values via "multiselect" in input category     
            },
            "pre_html": "",    // HTML code snippet above the element  
            "post_html":"",    // HTML code snippet below the element      
            "input_type": "text | select | radio | checkbox | file | hidden",   // hidden columns can be used for categorizing the records   
            "placeholder": "", 
            "input_label": "", 
            "options": [{      // required for select tag options; also applies to multiselect checkboxes     
                "label": "",       
                "value": ""       
            }],      
            "table_select_table": “<table-name>”, // for single/multiselect dropdown where the options are from another table in database      
            "table_select_search_column": "<column-name>", // column name of the table values in multiselect      
            "table_select_join_column": "<column-name>", // the column name of the main table that is used to join with the search table     
            "validation": {       // Set of validation parameters that can restrict the inputs      
                "isRequired": "0 | 1",    // "1" if mandatory; throws an error if user does not enter a value but clicks submit     
                "maxLength": 255,         // characters for text and textarea input fields
                "minLength": 1,   
                "types": ["image/png", "video/mp4", ... ],    // for restricting the file formats in file upload  
                "min_size_in_kb": <min-file-size>,
		"max_size_in_kb": <max-file-size>,        
                "min_height_in_pixels":  <min_height>,    // min. resolution specification for images
		"min_width_in_pixels":  <min_width>,   
		"max_height_in_pixels":  <max_height>,    // max. resolution specification for images
		"max_width_in_pixels":  <max_width>
            },      
            "sub_form_button_label": "", // This is the add sub form button label      
            "hide_sub_form_button": true, // Do not show "add sub form" button      
            "default_instances_count": 1, // Default sub form items to show, useful when the add sub form button is hidden       
            "hide_instance_remove_button": true | false, // Disable removing existing sub form rows by hiding the close button; This is useful along with hide sub form button enabled     
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
    "unique_keys":  ["<column-name>"], // Array of unique key columns of the main form root table can be specified here      
    "submit_button_label": "<button-label>",  // Default value is "Submit" 
    "label": "<form-heading>"  // Default value is "Form"
    }        
} 
```   
      

## Example of Forms ##   
For an application like a meeting scheduling app, forms can be used to create contacts and groups of contacts. Along with that meeting scheduling form can also provide the necessary elements of the email notification that will be sent in various scenarios.   
Forms for this example:    
[Contact](https://app.cliosight.com/app/forms/35/show?noNavbar=true)    
[Group](https://app.cliosight.com/app/forms/34/show?noNavbar=true)    
[Meeting](https://app.cliosight.com/app/forms/52/show?noNavbar=true)    

### A Section of the Meeting Creation Form <a name="meetings_form"></a>     
The components are:   
1. Multiselect input from another table 
2. Drop down menu with hardcoded values   
3. Rules based on the values selected from the drop down  
4. Sub form within a form 
5. Multiple instances of aggregated values within the sub form   

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
                	}]
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
                	}]
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
                	}]
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
                	}]
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

## JSON body of a Report ## <a name="report"></a>  
While a form is the data input interface, a report is the output of data analysis. Both are equipped with bulk upload and download options. Additionally, results of a report can be accessed via Cliosight's API that can serve as a source of data for visualization applications.  
1. A report contains filters and drill-down options through nested forms and reports. Just like a drop-down menu in a form, filters in a report can either have hardcoded values or column values of another report or a table. 
2. The contents of a report is nothing but the result of a SQL query. For any schema in an enterprise application, we can have numerous queries and hence innumerable reports and filters.      
A report can be embedded using a URL in the format:    
https://app.cliosight.com/app/reports/serial-number/show?noNavbar=true      
For instance, https://app.cliosight.com/app/reports/68/show?noNavbar=true      

### Example of a Meetings Report ###

## Creating Graphs and Charts with JavaScript libraries and Reports <a name="graphs"></a>

## Using Report data in Jupyter Notebook <a name="jupyter"></a>
Testing machine learning models involve analysis with python using datasets from various sources like Kaggle that are downloaded as CSV files. The result in then plotted on a graph using python libraries like matplotlib, seaborn and others. Reports can serve as an input for such analysis and visualization tasks and the resultant datasets can be pushed to different datasources from the same source code. Forms, reports and dashboards can be created instantly through APIs that return a link to the widget.

## JSON body of a Reporting Dashboard  <a name="dashboard"></a>
A reporting dashboard is an aggregation of related reports with global filters. 
1. It works like a report in terms of the definition of the filter menu. Global filters should be applied first as they take precedence over the report filters.  
2. It is possible to configure the filtering criteria by specifying which field should be used for the join operation with the dashboard reports.
3. It can have its own css definition which will override the css of the individual reports and forms.   
Just like a form and a report, a dashboard can be shared using a URL in the format. For instance, https://app.cliosight.com/app/dashboards/47/show?noNavbar=true      

## Example of a Reporting Dashboard <a name="example_dashboard"></a>  


```css
{
    "css_definition": "#mc_dashboard_47 .bg-primary, #mc_dashboard_47 .btn-primary {background-image: linear-gradient(45deg, rgb(29, 224, 153), rgb(29, 200, 205));}",
    "reports": [{     // list of reports
        "id": "68",
        "cols": 6
    }, {
        "id": "37",
        "cols": 6
    }, {
        "id": "33",
        "cols": 12
    }],
    "filter_menu": [{ 
        "report_id": 28, // Report to be used for the filter menu
        "label": "Group",
        "column": "id", 
        "column_label": "Name",  // Column name in the report
        "name": "group_id",	// The column name in the reports in the dashboard that is common to the filter report
        "reports": {     // The reports in this dashboard where filters will be applied
            "37": {
                "report_column": "group_id",
                "name": "group_id",
                "label": "Group",
                "label_column": "Name"
            },
            "68": {
                "report_column": "group_id",
                "name": "group_id",
                "label": "Group",
                "label_column": "Name"
            }
        }
    }]
}
  ```
## Ensuring Trustworthiness <a name="trust"></a>
It is possible to disable uploading CSV file contents through the import data option in forms created for an in-built datasource. Reports created from these tables can however serve as inputs to tables of other datasources from which reports can be generated. This ensures that the primary table and associated sub-form tables receive their inputs via the designated form interface only. As a result, any reports or charts generated from these tables will showcase a true representation of the data captured through the intended workflow. This approach helps maintain integrity and reliability of the data, reinforcing the accuracy of subsequent analyses and insights derived through the visualization widgets.

## JSON body of a Cliosight Trigger <a name="trigger"></a>

## Example of a Triggers ##
### Managing an SCD (Slowly Changing Dimension) <a name="scd"></a>

```css 
{
    "client_id": 2,
    "trigger_definition": {
        "trigger_type": "insert",
        "trigger_entity": "contacts",
        "trigger_action": "insert",
        "trigger_action_entity": "contacts_scd",
        "trigger_action_columns": [{
            "column": "email",
            "value": "{{new_row.email}}"
        }, {
            "column": "stage",
            "value": "{{new_row.stage}}"
        }, {
            "column": "start_datetime",
            "value": "{{current_datetime}}"
        }],
        "label": "New Contact stage scd"
    },
    "datasource_id": 1
}
```
## Email Notification <a name="email"></a>

### Sending Email Notifications on Events <a name="trigger_email"></a>

## Workflows <a name="workflow"></a>

## Examples of Workflows <a name="workflowexamples"></a>    

## Creating Workflows and Widgets in Jupyter Notebook <a name="python"></a>






