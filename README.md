![cliosight](https://media.licdn.com/dms/image/D4D22AQHLw_TPA7GCDw/feedshare-shrink_2048_1536/0/1697539569687?e=1700697600&v=beta&t=SgIwiRrspxrW3HAwkhKRUGMBxKrikm9ELU-Du-YIipQ)             
# Cliosight      
**Building the best applications & datasets**   
                 
Cliosight is a low-code platform that offers addition of various leading database servers for developing data-centric applications with SQL and JSON. This document is a bird's eye view of the major components of the platform, viz. datasources, forms, reports, dashboards, triggers and workflows.        

Please contact us at jigisha@cliosight.com or info@cliosight.com (Dikshit Baruah) for any questions.   
	    
Check out our [blog](https://medium.com/@cliosight) and connect with us over [Linkedin](https://www.linkedin.com/in/jigisha-aryya/).          

# Table of contents    

- [The Basic Architecture](#basic)
- [The building blocks of a Cliosight application](#app)       
	- [Form](#form)       
	- [Report](#report)        
	- [Dashboard](#dashboard)
	- [Automation](#automation)      
- [API](#api)
- [Free Tier Offerings](#freetier)
  
# The Basic Architecture <a name="basic"></a>      

![cliosight](https://miro.medium.com/v2/resize:fit:720/format:webp/1*ksvjxqFiHYyXAfC8_Zo9gA.png)        

- [SQL Interface for Structured Data](#sql)
	- [Support for Multiple Datasources](#datasources)
	- [Running Queries on a Datasource](#tables)
- [User Permissions and Access Control](#acl)
- [Advanced File Storage](#afiles)        
- [Leveraging AI Components](#ai)
- [Data Privacy and Security](#security)
- [Email Notification](#email)    

## SQL Interface for Structured Data <a name="sql"></a>
Our interface and APIs enable users to perform a wide range of analytical operations, encompassing both the in-house and user-owned relational and non-relational databases. As we continue to evolve, we plan to expand our compatibility to various cloud platform-specific databases and distributed ledgers.   

## Support for Multiple Datasources <a name="datasources"></a>        
Schemas created are segregated based on datasources. Users on the [free tier](#freetier) will have access to the in-built database only with a **shared connection pool**. They will be able to choose from a list of the top databases.         

SQL databases - MySQL, Postgres, Microsoft SQL Server, Google Cloud SQL and Amazon RDS.       
NoSQL databases - Cassandra, Mongodb, Amazon Dynamo DB, Google Big Query and Azure Cosmos DB.

Dev and enterprise tier users will be able to add multiple datasources to a single account. One other advantage is dedicated resources, ensuring faster data transfer and processing.      

![datasources](https://cdn-images-1.medium.com/max/800/1*L2ghFudqHTKOoQ0aIpU4qQ.png)  

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
Watch this video for creating a datasource.     
      
## Running Queries on a Datasource <a name="tables"></a>     
Admin users can execute create, update, delete and other SQL queries on a configured server instance with a database. It is a more convinient way to connect with multiple cloud databases compared to a conventional workbench because of the browser interface. Views are stored as reports that can be exported to other configured datasources.                 

## User Permissions and Access Control <a name="acl"></a>
Cliosight ensures that each component created within its platform incorporates fine-grained access control. Administrators can grant specific permissions to users, enabling controlled actions on UI and automation components. An example of this is access based on the geographic location of users.       

Types of users:     
1. **Admin** - Allowed to perform all actions within an account.          
2. **User** - Only view permission for assigned (non-public) forms, reports and dashboards.
3. Admin-Minor (WIP) - No access to datasources, tables, users/groups and automation components.
   
## Advanced File Storage <a name="afiles"></a>      
This platform provides extra functionalities for files uploaded through a form. The [free tier](#freetier) will allow a max of 4GB storage for the in-built database and files. Like other components, files are also equipped with access control methods. Text, CSV, spreadsheet, PDF, image, video and audio file formats will be supported for processing.    

Watch this video for creating a form for file upload and some uses of file reports.      

## Leveraging AI Components <a name="ai"></a>
To provide a better user experience through the use of AI, we have added code generators for the two types of syntaxes used within the platform.  
1. SQL query generator for tables and reports,     
2. JSON body generator for configuration.

One question that might arise is,   
**<i>Why is it not better to convert every part of the JSON to an instruction for an LLM to generate the underlying design and code in a preferred programming language?** </i>     
The answer to this is, even with meticulously crafted prompts, it is not always possible to describe the exact schema of a database and the fine details of each relation that might co-exist in a sophisticated application design. With tables, reports, forms, dashboards and automation code defined individually there is a simpler modular structure for the end goal. There is no need for the user to understand the code. The programming language becomes insignificant. Changes are done right in the config. Call it a shorthand for developing applications that comes with infrastructure. Having said that, the same framework can be generalized and extended to cover more of programming requirements like on the system side in the future.     

![ai](https://cdn-images-1.medium.com/max/800/1*dUJq2LUbfKAHZlW8WXaveg.png)     

Detailed explanation for using AI within Cliosight is given in this video.    

## Data Privacy and Security <a name="security"></a>       
Sharing private datasets created using our platform will have several security implications. In order to provide assurance to users, Cliosight is designed to have in place all the possible tools to make datasets compliant with the applicable regulations.    

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

## Email Notification <a name="email"></a>    
Emails are an integral part of any CRM application. Using our email notification system users will be able to send a max of 1000 emails to valid email ids for free. Visit the [automation](#automation) section to see how emails are sent using triggers and jobs in various business use cases.         

# The building blocks of a Cliosight application <a name="app"></a>       
 
![components](https://miro.medium.com/v2/resize:fit:720/format:webp/1*277E4iGxIAECACSyt2Y9RA.png)      

 [Form](#form)  &nbsp;&nbsp;|&nbsp;&nbsp;      [Report](#report)   &nbsp;&nbsp;|&nbsp;&nbsp;     [Dashboard](#dashboard)   &nbsp;&nbsp;|&nbsp;&nbsp;    [Trigger and Job](#trigger)   &nbsp;&nbsp;|&nbsp;&nbsp;  [Workflow](#workflow)    


# Form <a name="form"></a>    
   
- [JSON body of a Form with an example](#formjson) 
      
A form is the data input method to populate tables. It can have multiple sub-forms. It supports all basic input elements of a conventional HTML5 form.      

With the pre and post HTML JSON tags a form can function like a web page. This eliminates the need for a database when used within another application hosted on a cloud VM, a docker container or serverless infrastructure like Google App Engine, Azure App Service, and AWS Amplify. So we can have one or more Cliosight forms within an HTML file or several HTML code snippets within a form and other UI components discussed later. 
     
For instance, a simple [contact us](https://app.cliosight.com/app/forms/62/show?noNavbar=true) form can be used instead of the usual landing page. A form is complete, except for the limitation that it can have only one submit button.         

A dashboard which is similar to a UI container, enables any number of forms, reports and charts to be grouped together. This is explained in the [Dashboard](#dashboard) section. A report may also contain any number of nested reports and forms for drilling-down and editing or adding records as explained in the section for [Report](#report) creation.        

The format of the URL is https://app.cliosight.com/app/forms/52/show?noNavbar=true         

## JSON body of a Form <a name="formjson"></a>
    
Click ![here](https://github.com/cliosight/Docs/blob/main/form_json_format.css) to view the complete list of JSON tags that can be used within a form.   

## An Example of Using Forms - The Meeting Scheduler Application <a name="form_example"></a>        
For an application like a meeting scheduler portal, forms can be used to create contacts, groups and meeting requests. They can provide all the necessary information for an email that has to be sent out once a meeting is scheduled, updated or cancelled. Triggers and jobs explained later provides this capability.        
[Contact](https://app.cliosight.com/app/forms/35/show?noNavbar=true)    
[Group](https://app.cliosight.com/app/forms/34/show?noNavbar=true)    
[Meeting](https://app.cliosight.com/app/forms/52/show?noNavbar=true)  

### JSON tags of a Section of the Meeting Scheduler Form <a name="meeting_form"></a>     
The components are:   
1. Multiselect input from another table 
2. Drop down menu with hardcoded values   
3. Rules based on the values selected from the drop down  
4. Sub form within a form 
5. Multiple instances of aggregated values within the sub form

[Click here to view the JSON](https://github.com/cliosight/Docs/blob/main/meeting_form_json.json)        

Click here to understand the detailed use of each JSON tag in the forms.      

# Report <a name="report"></a>       
    
- [JSON body of a Report](#reportjson)     
- [Example of a Report](#report_example)     
- [Graphs and Charts with Reports](#graphs)
- [Using Reports in Jupyter Notebook](#jupyter)
- [Ensuring Trustworthiness](#trust)

  
While a form is the data input interface, a report is the output of data analysis with SQL or external Python code. It is equipped with a download option to save results in the CSV format. Results of a report can be accessed via Cliosight's API that can serve as a source of data for visualization applications.  

1. A report may contain filters and drill-down options through nested reports and forms. Just like a drop-down menu input field, filters in a report can either have hardcoded values or column values of another report or table. They can also have a multiselect option.
   
2. The content of a report is simply the result of a SQL query. Within an enterprise application's schema, there can be numerous SQL queries, leading to countless report and filter combinations.
          
A report can be embedded using a URL in the format https://app.cliosight.com/app/reports/29/show?noNavbar=true           

## JSON body of a Report <a name="reportjson"></a>      

The JSON tags of a report is given [here]().      
   
## Example of a Report - Contacts and Groups Report  <a name="report_example"></a> 
[Contacts & Groups](https://app.cliosight.com/app/reports/29/show?noNavbar=true) report in the meeting application shows all contacts along with the total number of groups for each.               

SQL Query for this report:      
``` sql
select min(c.id) as contact_id, min(gc.group_id) as group_id,
min(c.name) as Name, min(c.email) as Email, min(c.phone) Phone,
min(c.stage) as Stage, count(gc.id) as 'Total Groups' from `contacts` c 
left join `groups_contacts` gc on gc.contact_id = c.id
group by c.id
```
Click [here](https://github.com/cliosight/Docs/blob/main/meeting_report_section.json) to view the JSON body for this report.      

Detailed explanation for creating a report is given in this video.    

## Creating Graphs and Charts with JavaScript libraries and Reports <a name="graphs"></a>
Tabular data from reports can be used to plot graphs and charts using the standard Javascript or Python libraries for data visualization like Chart.js, Plotly, HighCharts, D3.js, C3.js, Google charts to name a few in Javascript. 

One such example is an area chart with Chart.js that depicts datasets from three different datasources, viz. in-built, containerized and fully-managed MySQL database instances across different cloud platforms. Another example is that of the common pie chart. We can also display live figures and stats by adding an iframe to an HTML or simply adding it to a dashboard. Visit the [API](#api) section for more.   

Below is the live demo:          
[Area chart with different datasources](https://cliosight.com/)      
[Pie chart](https://cliosight.com/)             
[Live stats report](https://app.cliosight.com/app/reports/85/show?noNavbar=true)           

Click here to watch a video explaining the Python and Javascript code.   

## Using Reports in Jupyter Notebook <a name="jupyter"></a>
Common Python packages can be used to train and test machine learning models. Data scientists and machine learning engineers prefer using free and open-source datasets from various data science platforms like Kaggle to experiment with their models. Datasets are typically downloaded as CSV files to store locally on the hard disk of their personal computers, cloud VM or serverless infrastructure where Jupyter is installed and accessed locally or remotely.     

Data is processed, split, or merged according to the requirements. Resultant datasets can be dumped as a CSV file or plotted on a graph using matplotlib et al.   

Alternatively, reports from different datasources in one or more Cliosight accounts can be used for such analysis and visualization tasks. The results of subsequent operations can be pushed back to connected datasources as new reports or as additional records for existing reports and tables. This makes creating, updating and sharing private datasets more secure for collaborative applications. Visit the [API](#api) section to know more.      

Detailed explanation for using reports in Python code is given in this video.      

## Ensuring Trustworthiness <a name="trust"></a> (WIP)    
It is possible to restrict data input into a table using the following features of a form:  
1. Disabling data import through CSV file upload.     
2. Disabling manual insert, update or delete query execution on the associated tables.
3. Ensuring that no other forms can insert data into those tables.
4. Disabling data export     

Reports created from restricted tables using one or more of the above methods can, however, serve as inputs to forms of other data sources from which reports can be generated if the fourth option is not applied. This ensures that the primary table and associated sub-form tables receive their inputs via the designated form interface only.
      
As a result, they  showcase a true representation of the data captured through the intended workflow. This approach helps maintain integrity and reliability, reinforcing the accuracy of subsequent analyses and insights derived through visualization. This real-world data can be used to generate artificial datasets using deep learning models like GANs (Generative adversarial networks) and VAEs (Variational autoencoders).    

# Dashboard <a name="dashboard"></a>        
      
- [JSON body of a Dashboard with example](#dashboardjson)
  
A dashboard is an aggregation of forms and multiple reports with global filters. It is basically a UI container element. It can have its own pre and post HTML code. The syntax enables users to place forms and reports one next to the other or in a sequential order. This makes it the easiest way to develop and host a micro CRM application, analytics dashboard, a SPA or a landing page.     

![Dashboard_aggregation](https://miro.medium.com/v2/resize:fit:1400/1*hZpjHig-UJXT8hwOkDgZPQ.png)              
  
#### Important features of a dashboard:     
1. Global filters need to be applied first as they take precedence over the report filters.
   
2. It is possible to configure the filtering criteria by specifying which field should be used for the join operation.
   
3. It can have its own CSS definition which will override the css of the constituent and nested forms and reports. This is however a configurable feature. 
     
![App](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*UXneRLM6cizrljW-C_D5eg.png)                
  
Just like the previous two types of widgets, a dashboard can be shared using a URL in a similar format.         
For instance, https://app.cliosight.com/app/dashboards/49/show?noNavbar=false    

## JSON body of a Dashboard  <a name="dashboardjson"></a>      

The JSON tags of a dashboard is given [here]().    

## Example of a Dashboard - Cliosight Meetings Portal <a name="dashboard_example"></a>   

[Cliosight Meetings](https://app.cliosight.com/app/dashboards/49/show?noNavbar=true) dashboard provides a quick view of the important stats, reports, charts and forms to test the application and present it in a desired way. Please note that it is possible to restrict access in a dashboard and its constituents upto the filter level.     

As explained earlier, the forms help in creating the three important entities of the schema, and the individual reports help in storing the results of an analysis such as the number of groups against each contact. A dashboard such as this one, will bring everything together for a unified view.        

```json
{
    "css_definition": "",
    "pre_html": "<div><br/><br/><h1 style=\"text-align: center; font-family: Raleway\">Cliosight Meetings</h1><br/><div class=\"pre-html-container align-items-center\"><img src=\"https://images.squarespace-cdn.com/content/v1/55a6ed19e4b0a3840f00af2f/1506720719545-CGYAQCFETD4430AHBQ3Z/board+meeting+graphic.png?format=2500w\" width=\"100%\" style=\"border: 2px solid grey\"></div><br/> <br/><iframe src=\"https://app.cliosight.com/app/reports/85/show?noNavbar=true\" width=\"100%\" height=\"400px\" style=\"background: #dee2e6! important; \"></iframe><br/></div>",

    "hideToolbar": false,

    "post_html": "<br/><h6 style=\"text-align: center\">Copyright Cliosight 2024. All rights reserved</h6><br/>",

    "dashlets": [
    ],
    "filter_menu": [{
        "label": "Group",
        "column": "id",
        "column_label": "Name",
        "report_id": 28,
        "name": "group_id",
        "reports": {
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

Detailed explanation for creating a dashboard is given in this video.    

# Automation <a name="automation"></a>       
  
- [JSON body of a Trigger with example](#trigger)
- [JSON body of a Job with example](#job)  
- [Sending Email Notification on Event](#trigger_email)        
- [JSON body of a Workflow with example](#workflow)             
- [Using Workflows in Jupyter Notebook](#python)

  
## JSON body of a Trigger <a name="trigger"></a>   
A trigger enables action on data and insights. Since we are dealing with structured data, it means executing the basic operations on table rows based on events. As explained later, when used along with jobs, it provides logic execution capability to the UI elements making them full-fledged cloud-hosted web applications.     
```json
{        
	"trigger_definition": {        
 		"trigger_type": "<crud-operation>",       
   		"trigger_entity": "<table-name>",  
     		"trigger_action_query": "<sql-query-as-a-formatted-string>",     
       		"label": "<name-for-the-trigger>"      
	 },      
  	 "datasourtce_id": 1     
    }        
```
Detailed explanation for creating and using a trigger is given in this video.      

## Example of a Trigger - Managing an SCD (Slowly Changing Dimension) <a name="scd"></a>

```json 
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

## Example of a Job - Managing free tier users of a SaaS platform <a name="job_example"></a>     

## Sending Email Notification on Event <a name="trigger_email"></a>    

## JSON body of a Workflow <a name="workflow"></a>

## Example of a Workflow - Executing daily HR operational tasks <a name="workflowexamples"></a>    

## Using Workflows in Jupyter Notebook for Machine Learning <a name="python"></a>     
Data analysis can provide pointers for fine-tuning an existing application or product design through hypothesis testing. It can also help in improving the performance of a machine learning model in production by training and testing with high quality datasets. 

Being able to code a workflow by combining tasks using a simple JSON configuration can speed up the design and development phases of an AI/ML project. This is made possible with the API framework that Cliosight has provided. Forms, charts, reports and dashboards can be created instantly through these APIs that return links to the widgets.         

# API <a name="api"></a>   
     

# Free Tier Offerings <a name="freetier"></a>   
    
|Category     |Free for 14 days|
|:----------------------|:-------------|
|Users    | 1    |
|Datasources       | 1
|Data and Files    |4 GB    |
|Widgets           |Unlimited|
|Automation        |Unlimited|
|Notification      |Limited (1000 free) |
|Customization           |Unlimited|
|Support    |Limited   |





