# Power-BI-Solution-Template

This Repo contains a list of accelerators for partners and customers to quickly get set up with enterprise ready dashboards and solutions.

## Prerequisites

* An Azure Virtual Machine with minimum recommended size A3 or an on premise server with equivalent or greater techinical specifications.
  * PowerShell version 3 or greater must be available on the server.
  * The Active Directory PowerShell module must be available (This is installed alongside [Remote Server Administration Tools](https://www.microsoft.com/en-us/download/details.aspx?id=45520)).
* SQL Server Enterprise Edition with a SQL Server Database.
  * SQL Server Analysis Server in tabular mode is optional.
  * SQL Server Agent must be enabled.
* A Power BI Pro Subscription
* If using Salesforce as a data source:
  * An account for Informatica (an ETL tool used to replicate Salesforce data).
  * An on premise Informatica Agent.
* If using Dynamics CRM as a data source:
  * An account for Scribe Online (an ETL tool used to replicate Dynamics CRM data).
  * An on premise Scribe Agent.

## Step 1: Configure the INI file

A sample INI file has been provided (Scripts\sample.ini).

## Step 2: Run setup.ps1

1. Open a new PowerShell console running as Administrator.
2. Run the following PowerShell command to enable scripting for this session: `Set-ExecutionPolicy ByPass`
3. Navigate to the Solution Template directory containing setup.ps1.
4. Execute setup.ps1 with the following command: `.\setup.ps1`

## Step 3: Post Deployment

1. If using Informatica, create a new Task using the newly created connections and select only the following entities to pull:
  * account
  * lead
  * opportunity
  * OpportunityLineItem
  * OpportunityStage
  * product2
  * user
  * UserRole
2. If using Scribe, edit the newly created Scribe solution to only pull the following entities:
  * account
  * businessunit
  * lead
  * opportunity
  * opportunityproduct
  * product
  * systemuser
  * systemusermanagementmap
  * territory
3. Configure the SQL Server Agent Jobs
  1. Open the Properties of the newly created "Save credential" SQL Server Agent job edit the job at the "Encrypt" step.
  2. Edit the following line to include your Scribe password: `$pwd_plain = "put_password here, run, then set this back to empty string`
  3. Run the "Data load and processing" job to pull data from your source or create a schedule for when it should run.

## Step 4: Configure the Power BI Enterprise Gateway

Download and set up the Power BI Enterprise Gateway according to [the instructions here](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-enterprise/).

## Step 5: Setting up the dashboards using Power BI

1. Open PowerBIReport\SalesManagementReport.pbix and update the connection to SQL Server Database or SQL Server Analysis Services based on the deployed environment (this will repopulate the graphs inside Power BI).
2. Publish the desktop file to https://www.powerbi.com and configure the gateway connection (refer to the link in step 4).

Congratulations - you have successfully deployed the Solution Template!
