<h1>Cliosight      
**Building the best data-driven applications and presentations**     
</h1>

# Technical Documentation               
![cliosight](https://miro.medium.com/v2/resize:fit:720/format:webp/1*HsRqxET_JE0b8kRq3P4OOg.png)  
A detailed explanation of datasources, forms, reports, dashboards, triggers and workflows within Cliosight.      
Please contact us at jigisha@cliosight.com or info@cliosight.com (Dikshit Baruah) for any questions.     

# Table of contents

- [SQL Interface for Structured Data](#sql)
- [Support for Multiple Data Sources](#datasources)
- [User Permissions and Access Control](#acl)
- [JSON body of a Form](#form)
- [JSON body of a Report](#report)
- [Graphs and Charts with Reports](#graphs)
- [Using Reports in Jupyter Notebook](#jupyter)
- [Ensuring Trustworthiness](#trust)
- [Data Privacy and Security](#security)
- [JSON body of a Dashboard](#dashboard)
- [JSON body of a Trigger](#trigger)     
- [JSON body of a Job](#job)       
- [Email Notification](#email)
- [JSON body of a Workflow](#workflow)
- [Example of a Workflow](#workflowexamples)
- [Creating Workflows and Widgets in Jupyter Notebook](#python)        


## SQL Interface for Structured Data <a name="sql"></a>
Cliosight is a robust platform that offers addition of various leading database servers, including MySQL, Postgres, and Microsoft SQL. Our interface and APIs enable users to perform a wide range of analytical operations, encompassing both the in-house and user-owned databases. As we continue to evolve, we plan to expand our compatibility to cloud data storages and distributed ledgers in future releases.   

![datastorages](https://miro.medium.com/v2/resize:fit:720/format:webp/1*r42HqHgUxlRpbFLr1PZtLg.png)    


## Support for Multiple Data Sources <a name="datasources"></a>
Paid users of our platform will be able to add multiple datasources to a single account. Configurations can be created for the in-built databases or those on the user's cloud VM. Cloud database instances like Google Cloud SQL are also supported. Schemas created are segregated based on these datasources. Users on the free tier will have access to the in-built MySQL database only with a **shared connection pool**.    

![databases](https://miro.medium.com/v2/resize:fit:720/format:webp/1*MQ_QTZ0CGrNoiIe3y8ePgA.png)

One other advantage of being a paid user is the significantly faster upload speed for large volumes of data. This is made possible by dedicating resources, ensuring efficient transfer and processing. Data import option provided with forms will allow uploading bulk data from CSV files. Data from a report can be downloaded or exported to other datasources.       

Example of a datasource definition:      
```json     
{
    "client_id": 2,
    "datasource_definition": {
        "type": "mysql",
        "connectionLimit": 10,
        "host": "34.23.219.145",
        "port": 3306,
        "user": "cliosight",
        "password": "Password123",
        "database": "cliosight",
        "multipleStatements": true,
        "label": "My Website Database (Shared Bandwidth)"
    }
}
```    
   
## User Permissions and Access Control <a name="acl"></a>
Cliosight ensures that each component created within its platform incorporates fine-grained access control. Administrators can grant specific permissions to users, enabling controlled actions such as data upload, viewing, and editing. An example of this control is restricting access to forms and reports based on the geographic location of users. Furthermore, administrators can designate users with the ability to create and execute triggers and workflows.     
Types of users:     
1. Admin - all possible actions within an account.          
2. Non-admin - View permission for assigned (non-public) forms and view permission assigned for reports and dashboards.
3. Admin-minor - All permissions for forms, reports, dashboards, triggers and workflows. No access to datasources, users/groups and tables.     
     
## Leveraging Automation and AI Components <a name="ai"></a>

While some meeting actions can be automated through triggers, there are other aspects that can provide a better user experience through the use of AI.
1. SQL query generator  
2. JSON body generator
3. HTML/CSS code generator      

SQL and JSON are the only two types of syntaxes used in Cliosight for configuring widgets. Apart from this, the "pre-html" and "post-html" tags allow users to embed an HTML inside a form, report or dashboard for adding extra elements like images, videos, text or hyperlinks. This makes it easy for users to host any of these UI elements like a conventional web page.      

![elements](https://miro.medium.com/v2/resize:fit:720/format:webp/1*9VNmQUsN851BWedd64bfJA.png)  
![workflow](https://miro.medium.com/v2/resize:fit:720/format:webp/1*83DJIW4Rkxy_-JkzYIeRmQ.png)
  
## JSON body of a Form <a name="form"></a>
A form can be created and shared independently or embedded into another application or web page. It can have sub-forms and reports. It supports all basic input elements of a conventional HTML5 form. 
With the pre and post-HTML JSON tags, a form can function like a web page except for the limitation that it can have only one submit button. With a dashboard widget that is similar to a UI container, any number of forms and reports can be grouped together. This eliminates the need for a database when embeded within apps and web pages hosted using cloud serverless infrastructure like Google App Engine, Azure App Service, or AWS Amplify.      
The format of the URL is **https://app.cliosight.com/app/forms/52/show?noNavbar=true**       
    
Below are the JSON tags that can be used within a form.   
```css
{   
   "datasource_id": <int-datasource-id>,      // Mandatory; to identify the datasource   
   "table": {    
        "name": "<main-table-name>" // Mandatory; This is the root table of the form      
    },       
   "sub_form_definition": {      
        "is_public": {       
            "status": <boolean> // Default is false; Indicates if the form is available without login      
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
            "input_category": "field | multiselect | form",  // Mandatory; form is for supporting sub-form definition;     
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
    "last_insert_id_key": "<column-name>", // Specify this column if its value is returned as the lastInsertId value; Example, auto_increment key in mysql.     
    "unique_keys":  ["<column-name>"], // Array of unique key columns of the main form root table can be specified here      
    "submit_button_label": "<button-label>",  // Default value is "Submit" 
    "label": "<form-heading>"  // Default value is "Form"
    }        
} 
```   
      
## Example of Using Forms in Applications ##   
For an application like a meeting scheduling portal, forms can be used to create contacts, groups and meeting requests. Forms can provide all the necessary elements for an email notification that has to be sent once it is scheduled, updated or cancelled.       
[Contact](https://app.cliosight.com/app/forms/35/show?noNavbar=true)    
[Group](https://app.cliosight.com/app/forms/34/show?noNavbar=true)    
[Meeting](https://app.cliosight.com/app/forms/52/show?noNavbar=true)     

### JSON tags for a Section of the Meeting Creation Form <a name="meetings_form"></a>     
The components are:   
1. Multiselect input from another table 
2. Drop down menu with hardcoded values   
3. Rules based on the values selected from the drop down  
4. Sub form within a form 
5. Multiple instances of aggregated values within the sub form

[Click here to view the JSON](https://github.com/cliosight/Docs/blob/main/meeting_form_json.json)        


## JSON body of a Report <a name="report"></a> 
While a form is the data input interface, a report is the output of data analysis with SQL or external Python code. It is equipped with a download option to save results in the CSV format. Results of a report can be accessed via Cliosight's API that can serve as a source of data for visualization applications.  
1. A report may contain filters and drill-down options through nested forms and reports. Just like a drop-down field in a form, filters in a report can either have hardcoded values or column values of another report or table. They can also have multiselect values.        
2. The content of a report is simply the result of a SQL query. Within an enterprise application's schema, there can be numerous queries, leading to countless report and filter combinations.          
A report can be embedded using a URL in the format given below:           
**https://app.cliosight.com/app/reports/29/show?noNavbar=true** 

### Example of a Report ###   
Contacts and Groups report in the meeting management portal shows all contacts with relevant details along with the total number of groups that they are a part of.    
[Contacts & Groups](https://app.cliosight.com/app/reports/29/show?noNavbar=true)         

SQL Query for this report:      
``` sql
select min(c.id) as contact_id, min(gc.group_id) as group_id,
min(c.name) as Name, min(c.email) as Email, min(c.phone) Phone,
min(c.stage) as Stage, count(gc.id) as 'Total Groups' from `contacts` c 
left join `groups_contacts` gc on gc.contact_id = c.id
group by c.id
```

```css
{
    "datasource_id": "1",
    "is_public": {
        "status": false
    },
 "css_definition": "@import url('https://fonts.googleapis.com/css?family=Raleway');.form{background: linear-gradient(45deg, rgba(0,0,0,0), rgba(29, 200, 205, 0));}.form{align-items: center; margin: 0%; border: 2px solid white !important; }.form {font-family: Raleway; color: grey; }.btn{background: linear-gradient(45deg, rgba(29, 224, 153, 0.8), rgba(29, 200, 205, 0.8));border-radius: 30px; border: 0px solid white; height:40px; width: 150px; margin: 10px;}.btn:hover{background:grey; color: white; border: 2px solid grey; border-radius: 30px;}input[type=text] {box-sizing: border-box; border: 2px solid #ccc; height:40px !important;  border: 2px solid grey; }input[type=text]:focus {border: 2px solid #555;} .form-control {border: 2px solid grey; height: 50px; margin-bottom: -10px;} .select2-choices {border: 2px solid grey !important;  border-radius: 5px !important; box-shadow: 5px 10px inset white !important;} p {background: linear-gradient(45deg, rgba(29, 224, 153, 0.8), rgba(29, 200, 205, 0.8));} .border {border: 1px solid green !important} textarea {margin-bottom: -20px !important; height: 6em !important} .select2-container .select2-default {height: 30px !important; font-size: 16px !important; box-shadow: 5px 10px inset white !important; vertical-align: center; } .select2-chosen {font-size: 14px !important}  .control-label {font-size: 14px !important; margin:2px !important}",
    "columns": {
        "group_id": {
            "hidden": true
        },
        "contact_id": {
            "hidden": true
        },
        "Name": {
            "links": [{
                "type": "form",
                "id": "35",
                "args": [{
                    "report_column": "contact_id",
                    "name": "id"
                }, {
                    "report_column": "Email",
                    "name": "email"
                }],
                "label": "Edit Contact"
            }]
        },
        "Total Groups": {
            "text-align": "right",
            "dropdown-menu-align": "right",
            "links": [{
                "type": "report",
                "id": "31",
                "args": [{
                    "report_column": "contact_id",
                    "name": "contact_id"
                }],
                "label": "View Groups"
            }]
        }
    },
    "filter_menu": [{
        "column": "Name"
    }, {
        "column": "Email",
        "report_id": 49,
        "name": "email"
    }, {
        "column": "Phone"
    }, {
        "column": "Total Groups"
    }],
    "report_links": [{  
        "type": "form",
        "id": "35",
        "label": "Add a Contact"
    }]
}
```     

## Creating Graphs and Charts with JavaScript libraries and Reports <a name="graphs"></a>
Tabular data from reports can be used to plot graphs and charts using any standard Javascript or Python libraries for data visualization, Chart.js, Plotly, HighCharts, D3.js, C3.js, Google charts to name a few in Javascript. One such example is an area chart with Chart.js that depicts datasets from three different data sources, viz. the in-house MySQL, a remote MySQL and a fully-managed MySQL database instance accross different cloud platforms. Another example is that of the common pie chart. We can also display live figures and stats by adding simple client-side Javascript to a form or an HTML.   
Below are the mentioned examples:      
Area chart with different datasources      
Pie chart     
Live stats on a landing page      

## Using Reports in Jupyter Notebook <a name="jupyter"></a>
Basic python coding can be used to test and train machine learning models using free and open-source datasets from various platforms, such as Kaggle. These are typically downloaded as CSV files and stored locally on the VM where Jupyter is installed or accessed remotely. They are processed, split, or merged according to the requirements. The results of analysis tasks are plotted on a graph using libraries like matplotlib and seaborn. Alternatively, reports from different datasources in one or more Cliosight accounts can be used for such data analysis and visualization. The results of subsequent operations can be pushed back to various datasources as new reports or as additional records for existing reports. This makes creation, updation and sharing of private datasets more secure for collaborative applications.      

## Ensuring Trustworthiness <a name="trust"></a> (WIP)    
It is possible to restrict data input into a table using the following features of a form:  
1. Disabling data import through CSV.     
2. Disabling manual insert, update or delete query execution on the associated tables.
3. Ensuring that no other forms can insert data into those tables.
4. Disabling data export     

Reports created from restricted tables using one of more of the above methods, can however serve as inputs to forms of other datasources from which reports can be generated, if the fourth option is not applied. This ensures that the primary table and associated sub-form tables receive their inputs via the designated form interface only. As a result, they  showcase a true representation of the data captured through the intended workflow. This approach helps maintain integrity and reliability, reinforcing the accuracy of subsequent analyses and insights derived through visualization. This real-world data can be used to generate synthetic datasets using deep learning models like GANs (Generative adversarial networks) and VAEs (Variational autoencoders (VAEs).     

## Data Privacy and Security <a name="security"></a>       
Sharing private datasets online has several security implications. In order to provide assurance to users, Cliosight is designed to have in place all the possible tools to make the datasets compliant with the applicable regulations.    
Some of the in-built features are:    
1. [Role-based Access Control](#acl)
2. Secure protocols for data and file transfer like HTTPS and SFTP only.
3. Automatic removal of personal information from data.     
4. Masking sensitive information with queries.      
5. Logs for tracking access and updates made on a table.
   
EU and USA:   
[GDPR vs. HIPPA compliance](https://www.onetrust.com/blog/hipaa-vs-gdpr-compliance/)      
India:     
[The PDP Bill](https://www.meity.gov.in/writereaddata/files/The%20Digital%20Personal%20Data%20Potection%20Bill%2C%202022_0.pdf)     
[The EHR Standards](https://main.mohfw.gov.in/sites/default/files/17739294021483341357.pdf)      

## JSON body of a Dashboard  <a name="dashboard"></a>
A dashboard is an aggregation of forms and multiple reports with global filters.  

Important points to remember while creating a dashboard:     
1. Global filters should be applied first as they take precedence over the report filters.  
2. It is possible to configure the filtering criteria by specifying which field should be used for the join operation amongst the reports.
3. It can have its own css definition which will override the css of the individual reports, nested forms within reports and embedded forms.   
Just like a form and a report, a dashboard can be shared using a URL in a similar format. For instance, 
https://app.cliosight.com/app/dashboards/47/show?noNavbar=true

A dashboard in Cliosight is basically a UI container for forms and reports. It can have its own pre and post HTML. The syntax enables users to place forms and reports one next to the other or in a sequential order. This makes it the easiest way to host a CRM dashboard, a simple web application or a regular landing page for a product or service.    
    
Below is an example.     
[Meeting portal](https://app.cliosight.com/app/forms/42/show?noNavbar=true)   

## Example of a Dashboard <a name="example_dashboard"></a>  

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

## JSON body of a Trigger <a name="trigger"></a>   
A trigger is the simplest way to taking action on data and insights. Since we are dealing with structured data, it means executing the basic CRUD operations on table rows based on events. As explained later, when used along with background jobs, it provides processing ability to UI elements making them full-fledged cloud-hosted web applications.     

```css
{        
	"trigger_definition": {        
 		"trigger_type": "<crud-operation>",  // insert/delete/update      
   		"trigger_entity": "<table-name>",  
     		"trigger_action_query": "<sql-query-as-a-formatted-string>",     
       		"label": "<name-for-the-trigger>"      
	 },      
  	 "datasourtce_id": 1     
    }        
```
## Example of a Trigger ##
### Managing an SCD (Slowly Changing Dimension) <a name="scd"></a>

```css 
{
    "client_id": 2,
    "trigger_definition": {
        "trigger_type": "insert",
        "trigger_entity": "`contacts`",
        "trigger_action_query": "insert into contacts_scd (email, stage, start_datetime) values('{{new_row.email}}', '{{new_row.stage}}', '{{current_datetime}}')",
        "label": "New Contact stage scd"
    },
    "datasource_id": 1
}
```

## JSON body of a Job <a name="job"></a>   

## Email Notification <a name="email"></a>    

### Sending Email Notifications on Events <a name="trigger_email"></a>

## JSON body of a Workflow <a name="workflow"></a>

## Example of a Workflow <a name="workflowexamples"></a>    

## Creating Workflows and Widgets in Jupyter Notebook <a name="python"></a>     
Data analysis can provide pointers for fine-tuning an existing application or product design through hypothesis testing. It can also help in improving the performance of a machine learning model in the production environment by training and testing with datasets of highest quality. Being able to code a workflow by combining tasks using a simple JSON configuration can speed up the design and development phases of an AI/ML project. This is made possible with the framework that Cliosight has provided. Forms, pages, reports and dashboards can be created instantly through APIs that return a link to the widget.  







