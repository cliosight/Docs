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
   	- [Support for Multiple Statements](#multiplestatements)
- [User Permissions and Access Control](#acl)
- [Advanced File Storage](#afiles)        
- [Leveraging AI Components](#ai)
- [Data Privacy and Security](#security)
- [Email Notification](#email)    

## SQL Interface for Structured Data <a name="sql"></a>
Our web interface with APIs enable users to perform a wide range of analytical operations, encompassing both the in-house and user-owned databases. As we continue to evolve, we plan to expand our compatibility to various cloud platform-specific databases, data warehouses and distributed ledgers.   

## Support for Multiple Datasources <a name="datasources"></a>        
1. Schemas created are segregated based on datasources.       
2. Users on the [free tier](#freetier) will have access to the in-built MySQL database only with a shared connection pool.         
3. Dev and enterprise users will be able to choose from a list of top databases for adding multiple datasources. One other advantage is dedicated resources.
                  
**SQL** - MySQL, Postgres, Microsoft SQL Server, Google Cloud SQL and Amazon RDS.       
**NoSQL** - Cassandra, Mongodb, Amazon Dynamo DB and Azure Cosmos DB.      
**Data warehouse** - Google Big Query   
      
## Running Queries on a Datasource <a name="tables"></a>     
Admin users can execute CRUD SQL queries and connect multiple cloud data storage products with our easy-to-use browser interface.          
There are two types of configurations:  
1. Shared Bandwidth (for trial users using the in-built database)    
2. Dedicated connections (for dev and enterprise users using both the in-built and external datasources)      

![datasources](https://raw.githubusercontent.com/cliosight/Docs/main/1%20_IhJ2hsx9USQOrgt_8I2PQ.png)  

## Support for Multiple Statements <a name="multiplestatements"></a>     
Execution of multiple SQL queries is possible in the admin console and for report creation. In reports however, only the final `select` statement is used for populating reports. The preceeding queries in a report SQL query block can be used for fetching real-time and dynamic intermediate values for substituting variable terms in the final query.
   
## User Permissions and Access Control <a name="acl"></a>
Cliosight ensures that each component created within its platform incorporates fine-grained access control. An example of this is, access based on the geographic location of users.       

Types of users:     
1. **Admin** - Allowed to perform all actions within an account.          
2. **User** - Only view permission for assigned (non-public) forms, reports and dashboards.
3. **Admin-Minor** - No access to datasources, tables, users/groups and automation components.
   
## Advanced File Storage <a name="afiles"></a>      
This platform provides extra functionalities for files uploaded through a form. The [free tier](#freetier) will allow a max of 4GB storage for the in-built database and files. Like other components, files are also equipped with access control methods.      

## Leveraging AI Components <a name="ai"></a>
To provide a better user experience through the use of AI, we have added code generators for the two types of syntaxes used within the platform.  
1. SQL query generator for tables and reports,     
2. JSON body generator for configuration.

One question that might arise is,   
**<i>Why is it not better to convert every part of the JSON to an instruction for an LLM to generate the underlying design and code in a preferred programming language?** </i>     
The answer to this is, even with meticulously crafted prompts, it is not always possible to describe the exact schema of a database and the fine details of each relation that might co-exist in a sophisticated application design. With UI and automation code defined individually there is a simpler modular structure for the end goal. Changes can be done right in the config because it interprets exactly the way a user wants. Having said that, the same framework can be generalized to cover more of programming requirements like on the system side in the future.     

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
Using our email notifications, users will be able to send a max of 100 emails to valid email ids for free. Visit the [automation](#automation) section to see how emails are sent using triggers and jobs in various business use cases.         

# The building blocks of a Cliosight application <a name="app"></a>       

 [Form](#form)  &nbsp;&nbsp;|&nbsp;&nbsp;      [Report](#report)   &nbsp;&nbsp;|&nbsp;&nbsp;     [Dashboard](#dashboard)   &nbsp;&nbsp;|&nbsp;&nbsp;    [Trigger and Job](#trigger)   &nbsp;&nbsp;|&nbsp;&nbsp;  [Workflow](#workflow)    


# Form <a name="form"></a>    
A form is the data input method for populating tables. It can have multiple sub-forms. It supports all basic input elements of a conventional HTML5 form. With the pre and post HTML JSON tags a form can function like a web page. A form is complete, except for the limitation that it can have only one submit button.        
## An Example of Using Forms - The Meeting Scheduler Application <a name="form_example"></a>        
For an application like a meeting scheduler portal, forms can be used to create contacts, groups and meeting requests. Data collected with forms can be used to [automate](#automation) tasks like sending out emails.          

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

# Report <a name="report"></a>          
  
While a form is the data input interface, a report is the output of data analysis with SQL or external code. Results of a report can be accessed via Cliosight's API or exported to other datasources with [jobs](#jobs).        
   
The content of a report is simply the result of a SQL query. Within an enterprise application's schema, there can be numerous SQL queries, leading to countless report and filter combinations.      
   
## Example of a Report - Contacts and Groups Report  <a name="report_example"></a> 
[Contacts & Groups](https://app.cliosight.com/app/reports/29/show?noNavbar=true) report in the meeting application shows all contacts along with the total number of groups for each created using multiple statements.                   

SQL Query for this report:      
``` sql
select count(*) as count from (select min(c.id) as contact_id, min(gc.group_id) as group_id, min(c.name) as Name, \         
min(c.email) as Email, min(c.phone) as Phone, min(c.stage) as Stage, \       
count(gc.id) as 'Total Groups' from `contacts` c  \        
left join `groups_contacts` gc on gc.contact_id = c.id   \
where ({{term}} is null or c.name like concat('%',{{term}},'%') or c.phone like concat('%',{{term}},'%'))    \
and (c.name = {{Name}} or {{Name}} is null)  \
and (c.phone = {{Phone}} or {{Phone}} is null)   \
group by c.id) abc;       

select min(c.id) as contact_id, min(gc.group_id) as group_id, min(c.name) as Name, \
min(c.email) as Email, min(c.phone) as Phone, min(c.stage) as Stage, \      
count(gc.id) as 'Total Groups' from `contacts` c  \     
left join `groups_contacts` gc on gc.contact_id = c.id  \
where ({{term}} is null or c.name like concat('%',{{term}},'%') or c.phone like concat('%',{{term}},'%'))  \
and (c.name = {{Name}} or {{Name}} is null)  \
and (c.phone = {{Phone}} or {{Phone}} is null)  \
group by c.id limit {{startIndex}}, {{pageSize}};
```

JSON for this report (without CSS and post HTML):         
```json
{
    "datasource_id": "1",
    "multipleStatements": true,
    "is_public": {
        "status": true
    },
    "post_html": "",
    "css_definition": "",
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
Tabular data from reports can be used to plot graphs and charts using the standard Javascript libraries for data visualization like Chart.js, HighCharts, D3.js, C3.js to name a few. One such example is a line chart that depicts datasets from three different datasources, viz. in-built, containerized and fully-managed MySQL database instances across different cloud platforms. We can also display live figures and stats by adding an iframe to an HTML or by simply using it within a dashboard. 
     
[Chart with different datasources](https://chart.cliosight.com)                      
[Live stats report](https://app.cliosight.com/app/reports/85/show?noNavbar=true)    

SQL query for the report:     
```sql
select concat(count(*), ' Meetings Scheduled') from `meetings` as count       
union all   
select concat(count(*), ' Contacts Created') from `contacts` as count    
union all    
select concat(count(*), ' Groups Organized') from `groups` as count    
union all    
select concat(count(*), ' Interviews Recorded') from `interviews` as count    
```
Corresponding JSON (without CSS definition):    
```json
{
    "datasource_id": "1",
    "pre_html": "<img src=\"https://images.squarespace-cdn.com/content/v1/55a6ed19e4b0a3840f00af2f/1506720719545-CGYAQCFETD4430AHBQ3Z/board+meeting+graphic.png?format=2500w\" width=\"100%\"></div><div><br/><h1>Our numbers tell the story</h1>",
    "is_public": {
        "status": true
    },
    "css_definition": "",
    "post_html": "<a href=\"https://app.cliosight.com/app/reports/85/show?noNavbar=true\" target=\"_blank\">Share this report</a></div>",
    "columns": {}
}
```

## Using Reports in Jupyter Notebook <a name="jupyter"></a>
Public datasets are typically downloaded as CSV files to store locally on the hard disk of personal computers, cloud VM or storage attached to serverless infrastructure. Jupyter is installed in these devices and accessed locally or remotely. Data is processed, split, or merged according to the requirements of a data science project. Resultant datasets can be dumped as a CSV file or plotted on a graph using matplotlib, seaborn et al.   

Alternatively, reports from different datasources in one or more Cliosight accounts can be used for this purpose. The results of subsequent operations can be pushed back to connected datasources as new reports or as additional records for existing reports or tables. This makes creating, updating and sharing private datasets more secure for collaborative applications. Visit the [API](#api) section to know more.      

## Ensuring Trustworthiness <a name="trust"></a>           
It is possible to restrict data input into a table using the following features of a form:  
1. Disabling data import through CSV file upload.     
2. Disabling manual insert, update or delete query execution on the associated tables.
3. Ensuring that no other forms or endpoints can insert data into these tables.
4. Disabling data export     

Reports created from restricted tables using one or more of the above methods can serve other datasources from which reports can be generated. This is however possible only if the fourth option is not applied.         
It ensures that the primary table and associated sub-form tables receive their inputs through the designated form interface or endpoint only. This real-world data can be used for generating artificial datasets using GANs (Generative adversarial networks) or VAEs (Variational autoencoders).    

# Dashboard <a name="dashboard"></a>        
  
A dashboard is an aggregation of HTML/CSS code snippets, forms and reports. Dashboards can have their own pre and post HTML. Access can be controlled upto the report filter level. This makes it the easiest way to develop and host an analytics dashboard, a SPA or a landing page.                                    

## Example of a Dashboard - Cliosight Meetings Portal <a name="dashboard_example"></a>   

[Cliosight Meetings](https://app.cliosight.com/app/dashboards/49/show?noNavbar=true)                    

```json
{
    "is_public": {
        "status": true
    },
    "css_definition": "",

    "pre_html": "<h1>Cliosight Meetings</h1>",

    "hideToolbar": true,

    "post_html": "<h6>Copyright Cliosight 2024. All rights reserved<br/><br/>Built with <a href=\"https://cliosight.com\">Cliosight</a></h6><br/>",
    "widget_groups": [{
        "widgets": [{
                "html": "",
                "cols": 1,
                "type": "html"
            }, {
                "id": "85",
                "cols": 10
            }, {
                "html": "",
                "cols": 1,
                "type": "html"
            }, {
                "id": "88",
                "cols": 12
            },
            {
                "id": "29",
                "cols": 12,
                "removeReportCss": false
            },
            {
                "html": "",
                "cols": 2,
                "type": "html"
            },
            {
                "type": "form",
                "id": "52",
                "cols": 8,
		"removeFormCss": false
            },
            {
		"type": "html",   
                "html": "",
                "cols": 2
            }
        ]
    }, {
        "widgets": [{
            "id": "68",
            "cols": 6
        }, {
            "id": "37",
            "cols": 6
        }],
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
    }]
}
```
# Automation <a name="automation"></a>   
Simple UI like forms and dashboards are useful. However for developing a full-fledged cloud hosted CRM application, automating tasks is mandatory. Our syntax provides a shorthand for one-click deployment of automation code.    
Provision for custom Javascript and Python code will be provided in the next release.     

## Sending Email Notification on Event <a name="trigger_email"></a>        
Sending notifications on events can be configured like any other backend component.   

## Trigger <a name="trigger"></a>   
A trigger enables action on data and insights. Since we are dealing with structured data, it means executing the basic operations on table rows based on events.  
    
## Example of a Trigger - Managing an SCD (Slowly Changing Dimension) <a name="scd"></a>    
### Existing Contact update action (refer to the Contacts form in Forms section)  
```json 
{
    "client_id": 2,
    "trigger_definition": {
        "trigger_type": "update",
        "trigger_entity": "`contacts`",
        "trigger_condition_query": "select ('{{old_row.stage}}' <> '{{new_row.stage}}') as 'condition'",
        "trigger_action_query": "insert into contacts_scd (email, stage, start_datetime) values('{{new_row.email}}', '{{new_row.stage}}', '{{current_datetime}}')",
        "label": "Existing Contact Stage SCD"
    },
    "datasource_id": 1
}
```
## Job <a name="jobs"></a>   
A job executes SQL queries at intervals for performing an ETL operation.   

## Example of a Job - Managing free tier users of a SaaS platform <a name="job_example"></a>     
Let's consider a simple application that notifies trial users of a SaaS product - Meeting Scheduler Portal. As an admin, I need to send notifications everyday through email.     

## Workflow <a name="workflow"></a>     
A workflow in Cliosight is an aggregation of interconnected jobs and triggers. Since it can only be configured for a single datasource, fetching data from others has to be carried out through other utility jobs.       

## Example of a Workflow - Executing daily sales operational tasks <a name="workflowexamples"></a>        
Consider the sales funnel that comprises sending promotional emails. We need to contact existing or potential customers on certain events at regular intervals.   

## Using Workflows in Jupyter Notebook for Machine Learning <a name="python"></a>     
Data analysis can provide pointers for fine-tuning an existing application or product design through hypothesis testing. It can also help in improving the performance of a machine learning model in production with high quality datasets. 

Being able to code a workflow by combining data management tasks using a simple JSON configuration can speed up the design and development phases of an AI/ML project. This is made possible with data pipelines. UI and automation components can be created instantly in external applications with our API.    

# API <a name="api"></a>    

## Fetch Data from a Report           
       
```python
import requests
import json

url = "https://app.cliosight.com/node/reports/70/data"

payload = json.dumps({
  "startIndex": 0,
  "pageSize": 500  
})
headers = {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImNsaWVudElkIjoyLCJ1c2VyRW1haWwiOiJjbGlvc3VpdGUuYXBwQGdtYWlsLmNvbSIsImlhdCI6MTcwNDU1NzE0NSwiZXhwIjoxNzA0NjQzNTQ1fQ.4oo2u_Xk6h75NrA_O8SuKGC90Rq6gbbsEaDzfOlryHg'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```
## With 'Agent' filter              
```python
import requests
import json

url = "https://app.cliosight.com/node/reports/70/data?tableRow={\"Agent\":\"Best Bricks\"}"

payload = json.dumps({
  "startIndex": 0,
  "pageSize": 500
})
headers = {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImNsaWVudElkIjoyLCJ1c2VyRW1haWwiOiJjbGlvc3VpdGUuYXBwQGdtYWlsLmNvbSIsImlhdCI6MTcwNDU1NzE0NSwiZXhwIjoxNzA0NjQzNTQ1fQ.4oo2u_Xk6h75NrA_O8SuKGC90Rq6gbbsEaDzfOlryHg'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)

```
## Create a new Report "Contacts Named Jigisha" for a Cliosight user and group               
```python
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:121.0) Gecko/20100101 Firefox/121.0',
    'Accept': 'application/json, text/plain, */*',
    'Accept-Language': 'en-US,en;q=0.5',
    'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImNsaWVudElkIjoyLCJ1c2VyRW1haWwiOiJjbGlvc3VpdGUuYXBwQGdtYWlsLmNvbSIsImlhdCI6MTcwNDQ0MzE4OCwiZXhwIjoxNzA0NTI5NTg4fQ.Bfn_u7WdwReEoKHJpXAtcD6XaYyzTn2nC_CK3Gyr3Lg',
    'Content-Type': 'application/json',
    'Connection': 'keep-alive',
    'Referer': 'https://app.cliosight.com/app/reports',
    'Sec-Fetch-Dest': 'empty',
    'Sec-Fetch-Mode': 'cors',
    'Sec-Fetch-Site': 'same-origin',
}

json_data = {
    'report_definition': {
        'config': '{ "columns": {}\n}',
        'query': "select * from `contacts` c where c.name like '%Jigisha%'",
        'label': 'Contacts Named Jigisha',
    },
    'mc_users': [
        '2',
        '6',
    ],
    'mc_groups': [
        '5',
    ],
    'datasource_id': '1',
}

response = requests.post('https://app.cliosight.com/node/reports/create', headers=headers, json=json_data)            
```

# Free Tier Offerings <a name="freetier"></a>   
    
|Category     |Free for 14 days|
|:----------------------|:-------------|
|Users    | 1    |
|Datasources       | 1
|Data and Files    |4 GB    |
|Widgets           |Unlimited|
|Automation        |Limited|
|Notification      |Limited (100 free) |
|Customization           |None|
|Support    |Limited   |

