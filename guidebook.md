# Low-code Travel Request Application

## Goal

In this session, you will get hands-on experience in creating a Travel Request application using the ServiceNow platform. We will cover concepts such as Building Tables, Record Producers, Integrations and Workflows within App Engine Studio - our low-code application development interface for developers of any level of experience.

## Background

![relative](images/nowairline.png)

As the Travel and Expense manager at ACME Corporation , you currently have very little visibility and control in the travel plans of your employees. Any travel plans now are requested via email from an employee to their manager, and you only get notified once the expense claims come in. The CFO has hence given you the task of better managing the costs associated with travel of your employees, and making sure that their travels are justifiable and reasonable. With that in mind, you have decided to take things in your own hands and build a Travel Request application leveraging the ServiceNow platform.

# Exercise 1: Creating tables for our travel request application

## Introduction

In the first section, we will create two tables. Our first table will be used to capture the Travel Requests coming in from employees, and our second table will be used to store all the major airports and cities that can be traveled to. We will assume that all Travel Requests will only be for air travel!

## Let's start

1. Click **All**, then search and click **App Engine Studio**

    ![relative](images/openaes.png)

1. Click **Create app** on the top right of the screen

    ![relative](images/createapp.png)

1. On the Create App page, name the app "Travel request", and for description, enter "Track travel requests from employees."

    ![relative](images/appname.png)

1. Click **Continue**

1. Leave the default roles - *admin* and *user*, and click **Continue**

1. Click **Go to app dashboard**

    > What you've just done is create a scoped application. Scope uniquely identifies every application file, not just within a single ServiceNow instance, but in every instance around the world. Why is this important?
    >- Scope protects an application, its files, and its data from conflicts with other applications.
    >- Scope determines which parts of an application are available for use by other applications in ServiceNow.
    >- Scope allows developers to configure which parts of their application can be acted on by other applications.
    >- Scope prevents work done in the main ServiceNow browser window (not in Studio) from becoming part of an application's files.
    >- Witout Scope, it will be very difficult to govern new applications!

## Create a Travel Request table

We will now create a table to capture the travel requests.

1. Under **Data**, click **Add**

    ![relative](images/addtable1.png)

1. Click **Create a table**

1. Click **Get started**

1. On the *Add Data* page, click **Create from an existing table**

1. Click **Continue**

    ![relative](images/existingtable.png)

1. On the next page, search and select **Task**

    ![relative](images/selecttask.png)

    >The task table is one of the core tables provided on the platform. Any table that extends task can take advantage of task-specific functionality such as SLAs and Approvals. This speeds up the overall process and ease of building logic.

1. Click **Continue**

1. For Table label, enter **Travel request**. Table name should be auto-populated.

1. Check **Auto number**

1. For Prefix, enter **TRVREQ**

    ![relative](images/tabledetails.png)

1. Click **Continue**

1. Allow all access for *admin* and **Create** and **Read** access for *user*, then click **Continue**

    ![relative](images/tabacl.png)

1. Click **Edit table**

1. If presented with the **GETTING STARTED** pop-up, close it.

1. You should now be on tha *Table Builder* interface. Click **Add new field**, and add the following fields:

    Column label | Type
    -------------- | --------------
    Departure date | Date
    Estimated airfare | Decimal 
    Reason for travel | Choice (Dropdown with --None--) : Internal meeting, Customer meeting, Training

1. Your screen should look like this

    ![relative](images/newfields1.png)

1. Click **Save**

    At this point, we could also capture the Origin and Destination via a String field so that the users can enter free text, but for more consistency, let's create a **Airports** table so that users can select these locations (like how you would select on any airline reservation website)

## Create an Airport table

1. Click the **App Home** tab to return to the main view

    ![relative](images/apphome.png)

1. Under **Data**, click **Add**

    ![relative](images/addairport.png)

1. Click **Create a table**

1. Click **Get started**

1. This time, select **Upload a spreadsheet**

    ![relative](images/uploadss.png)

1. Click **Continue**

1. Download this file: [airports.xlsx](downloads/airports.xlsx "download")

1. Upload the downloaded file to the upload box. You should see the following screen once the upload is successful

    ![relative](images/uploadcomplete.png)

1. *IMPORTANT*: Make sure to check the **Import spreadsheet data** box.

1. Click **Continue**

1. After a short loading time, you should land on the page that says: "Great! Here's the info we brought over from your spreadsheet"

    ![relative](images/mapexcel.png)

1. Scroll through the list to see all the fields that will be created. Notice that you can change the data **Type** if necessary, but we can leave everything as String fields for now

1. Click **Continue**

1. Under **Table label**, enter **Airport**. **Table name** will be automatically populate, leave it as it is

1. Check **Auto number**, then under **Prefix**, enter **AIRPORT**

    ![relative](images/filledupairport.png)

1. Click **Continue**

1. In the roles page, check **All** for *admin*, and only **Read** for *user*

    ![relative](images/airportrole.png)

1. Click **Continue**

1. Click **Edit table**

1. On the table editor, toggle **Display** on the **Name** row. This is what users will search for when selecting airports

    ![relative](images/displayname.png)

1. Click **Save** on the top right of the screen

1. Click **Preview**

1. A new tab should open and show you the list of airports you have imported. 
  
    ![relative](images/airportlist.png)

Great, you now have a table to store the list of Airports around the world!

## Completing the Travel request table

1. If the **Travel request** tab is still open, click to navigate to it

    ![relative](images/returntreq.png)

1. If not, return to **App Home** and open the **Travel request** table

1. Click **Add new field**, and add the following fields:

    Column label | Type
    -------------- | --------------
    Travel from | Reference (Airport)
    Travel to | Reference (Airport) 

    ![relative](images/reference1.png)

1. It should look like the following once you've added the two new fields

    ![relative](images/tftt.png)

1. Click **Save**

    > For simplicity, we are not adding additional fields like Return date, Daily estimated expenses, etc. You can always choose to add those fields if you want to.

## Styling the backend form

1. Still in the table designer, click **Form views**

    ![relative](images/formviews.png)

1. Remove the following fields from the screen by clicking the **X** icon

    - Assigned to
    - Configuration item
    - Priority
    - Parent
    - Active

    ![relative](images/removefields.png)

1.  These fields are inherited from the task table, and are not needed for the purposes of our application.

1. From the left sidebar, search and drag in the following fields onto the form views

    - Reason for travel
    - Departure date
    - Travel from
    - Travel to
    - Estimated airfare
    - Opened by

1. When completed, your form view should look similar to this

    ![relative](images/editedform.png)

1. Click **Save**

Congratulations, you have completed Exercise 1 and now have a complete way to store the Travel requests from your employees.

# Exercise 2: Creating a form on the Portal

For this exercise, we will focus on exposing your newly created table on one of the employee portals so that your employees can easily access and create a travel request for themselves.

1. Click the **App Home** tab to return to the main view

1. Click **Add** under **Experience**

    ![relative](images/addexp.png)

1. On the following screen, click **Record Producer**

    ![relative](images/selectrp.png) 

1. Click **Get started**

1. On the **ADD EXPERIENCE** screen, enter *Raise a travel request* for **Name**, and *Capture employee travel requests* for description

    ![relative](images/namerp.png)
    
1. Click **Continue**

1. Click **Edit record producer**

1. Under **Description**, enter the following text: "Use this form to raise a travel request for all international flights. Approval will be routed to your manager."

1. (Optional) Add an image if you wish (You can use anything you find online)

    ![relative](images/basicinforp.png)

1. On the left sidebar, click **Destination**

1. We will define where this form will route requests to. Search and select **Travel request**. This is the table we first created.

    ![relative](images/destination.png)

1. On the left sidebar, click **Location**. We will define which portal this form will be located in. A form can belong to multiple portals on ServiceNow

1. On the main area, click **Browse**

    ![relative](images/browse.png)

1. On the next screen, look for **Service Catalog** under the **Available** section and double click on it (Your list might appear different from the screenshot below)

    ![relative](images/selectservicecat.png)

1. After double clicking, ensure that **Service Catalog** now appears in the **Selected** section

1. On the bottom right, click **Save selections**

1. Click **Browse** under **Categories**

1. This time, do the same as before, and select **Can We Help You?** from the **Available** list

    ![relative](images/canwehelp.png)

1. On the left sidebar, select **Questions**

1. Click on the dropdown arrow next to **Insert new question**, and click **Single column container**

    ![relative](images/scc.png)

1. In the pop-up box, enter **Information** under **Title**

1. Click **Submit**

1. Repeat the top 3 steps again, but this time select **Two column container** and enter **Locations**

1. Your screen should look like this
   
    ![relative](images/questionscolumn.png)

1. Expand the **Information** section, click the **Insert** icon, and select **New question**

    ![relative](images/newquesiton.png)

1. In the next form, fill it out as below

    Name | Selection
    -------------- | --------------
    Question type | Choice
    Question subtype | Dropdown (fixed values)
    Map to a specific field on the table | **Checked**
    Tabel field | Reason for travel
    Question label | What is the reason for travel?
    Mandatory | **Checked**

    ![relative](images/filledp1.png)
    ![relative](images/reasonpt2.png)

1. Click **Choices**

1. Check **Include none choice**

1. Under **Available Choices**, add the 3 reasons you added during table creation: Internal meeting, Customer meeting, Training

    ![relative](images/canpreview.png)

1. On the bottom right, click **Insert Question**

    >For the purposes of time in this lab, we will skip adding the following fields to our form: **Departure Date** and **Estimated airfare**. Feel free to take up the challenge and add those fields into your form if you have the time.

1. Expand the **Dates** section

1. On the left column, add a **New question**

    ![relative](images/datequestion.png)

1. Fill out the form as follows

    Name | Selection
    -------------- | --------------
    Question type | Choice
    Question subtype | Record reference
    Map to a specific field on the table | **Checked**
    Tabel field | Travel from
    Question label | Where are you departing from?
    Mandatory | **Checked**

1. Click the **Additional details** tab

1. Under **Source table**, search and select **Airport** (This is the table you imported from the spreadsheet)

1. On the bottom right, click **Insert Question**

1. In the main screen, follow the steps above once more for **Travel to**

    Name | Selection
    -------------- | --------------
    Question type | Choice
    Question subtype | Record reference
    Map to a specific field on the table | **Checked**
    Tabel field | Travel to
    Question label | Where are you traveling to?
    Mandatory | **Checked**

1. Your form should now look like this

    ![relative](images/clickpreview.png)

1. (Optional) You can preview how your form will look like by clicking on the **Preview** button on the top right
    
    ![relative](images/previewform.png)

1. If you're previewing the form, close it by clicking the cross icon on the top right

1. On the left sidebar, click **Review and submit**

1. Click the **Submit** button