**Use Case 01 – Build SQL database in Microsoft Fabric to query, report,
and share the data**

**Introduction**

This use case provides a hands-on lab experience for building and
managing a SQL database within **Microsoft Fabric**, a unified data
platform. It is designed for users who want to explore the capabilities
of Microsoft Fabric for data storage, querying, reporting, and sharing.
The lab walks through the complete lifecycle of a SQL database—from
environment setup and data import to advanced analytics and API
creation—using the AdventureWorks sample dataset. It emphasizes
practical application through step-by-step exercises that simulate
real-world data scenarios.

**Objectives**

- Set up a Microsoft Fabric trial environment and create a new
  workspace.

- Enable and configure SQL database capabilities within Fabric.

- Create and manage a SQL database using the Fabric portal.

- Load and explore sample data (AdventureWorksLT) in the SQL database.

- Write and execute T-SQL queries to create and manipulate database
  objects.

- Query data using the SQL Analytics Endpoint and understand OneLake
  integration.

- Generate and test a GraphQL API from the SQL database.

- Build and publish interactive reports using Power BI connected to the
  Fabric SQL database.

- Share data securely and manage user access and roles within the Fabric
  environment.

- Clean up resources after completing the lab.

## Exercise 1 – Set up your environment

### **Task-1: Start a Fabric Capacity Trial**

Follow these steps to start your Fabric capacity trial and become the
Capacity administrator of that trial.

1.  Open your browser and browse the **Microsoft Fabric Trial Page**
    +++https://app.fabric.microsoft.com/+++

2.  In the **Microsoft Fabric** window, enter your given credentials,
    and click on the **Submit** button.
     ![](./media/image1.png)

4.  Then, In the **Microsoft** window enter the password and click on
    the **Sign in** button 

    ![](./media/image2.png)

5.  In **Stay signed in?** window, click on the **Yes** button.

    ![](./media/image3.png)

6.  On **Fabric Home** page, click on the **Account manager** on the
    right side.

     ![](./media/image4.png)

6.  In the Account manager, select **Free trial**. If you don't
    see **Free trial** or **Start trial** or a **Trial status**, trials
    might be disabled for your tenant.

    > **Note:** If the Account manager already displays **Trial status**,
    > you may already have a **Power BI trial** or a **Fabric (Free)
    > trial** in progress. To test this out, attempt to use a Fabric
    > feature. For more information, see [**Start using
    > Fabric**](https://learn.microsoft.com/en-us/fabric/fundamentals/fabric-trial#other-ways-to-start-a-microsoft-fabric-trial).
    
     ![](./media/image5.png)

7.  If prompted, agree to the terms and select the appropriate Trial
    capacity region and then select **Activate**.

    ![](./media/image6.png)

8.  Once your trial capacity is ready, you receive a confirmation
    message. Select **Fabric Home Page** to begin working in Fabric.
    You're now the Capacity administrator for that trial capacity.

     ![](./media/image7.png)

9.  Open your Account manager again. Notice the heading for **Trial
    status**. Your Account manager keeps track of the number of days
    remaining in your trial.

    ![](./media/image8.png)

Congratulations. You now have a Fabric trial capacity that includes a
Power BI individual trial (if you didn't already have a Power
BI *paid* license) and a Fabric trial capacity.

### **Task-2: Create a New Fabric Workspace**

You can use an existing workspace or create a new Fabric workspace.  In
workspaces, you create collections of items such as lakehouses,
warehouses, and reports. You must be a member of the Admin or Member
roles for the workspace to create a SQL database.

To create a workspace:

1.  In the **Fabric** home page, select **+New workspace**.

    ![](./media/image9.png)

2.  In the **Create a workspace tab**, enter the following details and
    click on the **Apply** button.


     |   |    |
     |-----|-----|
     |Name|	+++SQLDatabaseXX+++(XX can be a unique number)|
     |Advanced	|Under License mode, select Trial|


    ![](./media/image10.png)
 
    ![](./media/image11.png)
 
    ![](./media/image12.png)

## Exercise 2 – Enable SQL Database in your Fabric 

### **Task-1: Enable SQL database for your tenant**

Follow these steps to enable SQL database for your tenant.

1.  Go to **settings** option in Fabric and select **Admin portal**.

    ![](./media/image13.png)

2.  Enable the **Users can create Fabric items** switch and click on
    **Apply**. 

    ![](./media/image14.png)

3.  Enable **SQL database (preview)** switch and click on Apply.

     ![](./media/image15.png)

## Exercise 3 – Create a SQL Database in the Fabric Portal

1.  In the Fabric Portal, click on **+ New Item**, search for **SQL
    databases**, and select **SQL database (preview) tile**

    ![](./media/image16.png)

2.  Provide a name for the **New Database** as **+++sqlbd+++**.
    Select **Create**.

     ![](./media/image17.png)

3.  When the new database is provisioned, on the **Home** page for the
    database, notice the **Explorer** pane showing database objects.

     ![](./media/image18.png)

4.  Under **Build your database**, three useful tiles can help you get
    your newly created database up and running.

- **Sample data** option lets you import a sample data into
  your **Empty** database.

&nbsp;

- **T-SQL** option gives you a web-editor that can be used to write
  T-SQL to create database object like schema, tables, views, and more.
  For users who are looking for code snippets to create objects, they
  can look for available samples in **Templates** drop down list at the
  top of the menu.

&nbsp;

- **Connection strings** option shows the SQL database connection string
  that is required when you want to connect using SQL Server Management
  Studio, the mssql extension with Visual Studio Code, or other external
  tools.

![](./media/image19.png)

## Exercise 4 - Load AdventureWorks sample data in your SQL database in Microsoft Fabric

1.  Once the new database is created, open the database's home page.
    Select **Sample Data**.

     ![](./media/image20.png)

2.  You'll see a **Loading Sample Data** notification.

   - Don't modify the database while the import is in process.

   ![](./media/image21.png)
    ![](./media/image22.png)

3.  Once complete, there's a notification. The object explorer also
    refreshes to show the new **SalesLT** schema. You're now ready to
    get started with the AdventureWorksLT sample database.

     ![](./media/image23.png)

4.  Expand the SalesLT schema in the **Object Explorer** to see the
    objects that were created. Select on any of the tables to quickly
    view the data.

    ![](./media/image24.png)

5.  For more options, like select **top 1000 rows** or to script an object
    out, right-click or select the context menu (...) of the object
    name.

    ![](./media/image25.png)
    
    ![](./media/image26.png)

## Exercise 5 – Create a table in SQL Database in Fabric

### **Task-1: Create a table with T-SQL Queries**

1.  Select the **New Query** button in the main ribbon.

    ![](./media/image27.png)

2.  Create the definition of your table in T-SQL with the help of this
    sample, in the query editor, copy and paste the following code.
    Select the entire text and click on *the **Run*** button to execute
    the query. After the query is executed, you will see the results.

    ```
    CREATE TABLE dbo.products ( 
    product_id INT IDENTITY(1000,1) PRIMARY KEY, 
    product_name VARCHAR(256), 
    create_date DATETIME2 
    )
    ``
   ![](./media/image28.png)
 
   ![](./media/image29.png)

3.  If the **Object Explorer** is already expanded to show tables, it
    will automatically refresh to show the new table upon create. If
    not, expand the tree to see the new table.

    ![](./media/image30.png)

## Exercise 6 – Query the SQL Analytics Endpoint of your SQL Database in Fabric

### **Task-1: Query the SQL Analytics Endpoint**

1.  Open an existing database that was loaded with the sample data.

2.  Expand the **Object Explorer** and make note of the tables in the
    database.

     ![](./media/image31.png)

3.  Select the replication menu at the top of the editor,
    select **Monitor Replication**.

     ![](./media/image32.png)

4.  A list containing the tables in the database will appear. If this is
    a new database, you'll want to wait until all of the tables have
    been replicated. There is a refresh button in the toolbar. If there
    are any problems replicating your data, it is displayed on this
    page.

    ![](./media/image33.png)

5.  Once your tables are replicated, close the **Monitor
    Replication** page.

6.  Select the SQL analytics endpoint from the dropdown in the SQL query
    editor.

    ![](./media/image34.png)
     You now see that the **Object Explorer** changed over to the warehouse
     experience.
     ![](./media/image35.png)

7.  Select Address table under the SalesLT database to see the data
    appear, reading directly from OneLake.

     ![](./media/image36.png)

8.  Select the context menu (...) for any table, and
    select **Properties** from the menu. Here you can see the OneLake
    information and ABFS file path.

    ![](./media/image37.png)
 
     ![](./media/image38.png)

9.  Close the **Properties** page and select the context menu (...) for
    one the tables again. Select **New Query** and **SELECT TOP 100**.

    ![](./media/image39.png)

10. **Run** the query to see the top 100 rows of data, queried from the
    SQL analytics endpoint, a copy of the database in OneLake.

    ![](./media/image40.png)
 
    ![](./media/image41.png)
 
    ![](./media/image42.png)

## Exercise 7 – Create GraphQL API from your SQL Database

To create an API for GraphQL:

1.  In the  **Fabric** page menu bar on the left side, select **sqldb**
    database .

    ![](./media/image43.png)

2.  Open the database where you want to create a GraphQL API and select
    **'New API for GraphQL'** from the toolbar ![](./media/image44.png)

3.  Enter the **Name API for GraphQL** as **+++sqlbd_api+++** and
    select **Create**.

    ![](./media/image45.png)

    At this point, the API is ready but it's not exposing any data. APIs for
    GraphQL are defined in a schema organized in terms of types and fields,
    in a strongly typed system. Fabric automatically generates the necessary
    GraphQL schema based on the data you choose to expose to GraphQL
    clients.

     ![](./media/image46.png)

4.   The **Choose data** screen allows you to search and choose the
    objects you want exposed in your GraphQL schema. Select the
    checkboxes next to the individual tables or stored procedures you
    want to expose in the API. To select all the objects in a folder,
    select the checkbox with the data source name at the top.

5.  Select **Load** to start the GraphQL schema generation process.

     ![](./media/image47.png)

6.  To add data to the API for GraphQL, click on **'Select data
    source'**.

    ![](./media/image48.png)

7.  On **Choose connectivity option** dialog box, select **Connect to
    Fabric data sources with single-on (SSo) authentication** and click
    on **Ok** button.

    ![](./media/image49.png)

8.  In the OneLake catalog tab, select the **sqldb of the SQL Analytics
    endpoint** and click on the **Connect**
    button
    ![](./media/image50.png)

10.  In the Choose data tab select Adress, SalesOrder, Custemer and
    Product tables and click on **Load** button

   ![](./media/image51.png)

10. The schema is generated, and you can start prototyping GraphQL
    queries (read, list) or mutations (create, update, delete) to
    interact with your data. The following image shows the **Schema
    explorer** with an API call template.

    ![](./media/image52.png)

Your API for GraphQL is now ready to accept connections and requests.
You can use the API editor to test and prototype GraphQL queries and the
Schema explorer to verify the data types and fields exposed in the API.

## Exercise 8: Create simple reports on your SQL database in Power BI

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL:
    +++https://powerbi.microsoft.com/en-us/desktop/+++ , then press
    the **Enter** button.

2.  Click on the **Download now** button.

     ![](./media/image53.png)

3.  In case, **This site is trying to open Microsoft Store** dialog box
    appears, then click on **Open** button.

     ![](./media/image54.png)

4.  Under **Power BI Desktop**, click on the **Get** button.

     ![](./media/image55.png)

5.  Now, click on the **Open** button.

     ![](./media/image56.png)

6.  Enter your **given** credentials and click on the **Next** button.

    ![](./media/image57.png)

7.  Enter the **Administrative password** from the **Resources** tab and
    click on the **Sign in** button.

     ![](./media/image58.png)

8.  In the **Home** tab, select the **OneLake catalog**

      ![](./media/image59.png)

9.  In the **OneLake catalog** tab, select **SQL database** and click on
    **Connect** button

    ![](./media/image60.png)

10. Enter your tenant credentials and click on the **Next** button.

     ![](./media/image61.png)

11. Enter the **password** from the **Resources** tab and click on
    the **Sign in** button.

     ![](./media/image62.png)

12. In the Navigator page, under **Display Options**, select the desired
    tables in your SQL database, then click on the **Load** button.

     ![](./media/image63.png)

13. Choose **DirectQuery** and click on Ok button

      ![](./media/image64.png)

     ![](./media/image65.png)

     ![](./media/image66.png)

14. In the **Power BI** page, under **Visualizations**, click on
    the **Table** icon 

     ![](./media/image67.png)

15. On the **Data** pane, expand the **SalesLT Customer table** and
    select the **columns for Company Name, Email Address, and First
    Name.**

     ![](./media/image68.png)

16. In the **Power BI** page, under **Visualizations**, click on
    the **Clustered** **Column chart** icon to add a **Column chart** to
    your report

     ![](./media/image69.png)

17. On the **Data** pane, expand **SalesLT SalesOrderDetail**  and check
    the box next to **OrderQty**. This creates a column chart and adds
    the field to the **X-axis**.

18. Check the box next to **UnitPrice**. This adds the field to
    the **Y-axis**.

19. Check the box next to **UnitPriceDiscount**. This adds the field to
    the **Legend**.

     ![](./media/image70.png)

20. Click on the **Publish** in the command bar.

     ![](./media/image71.png)

21. In **Microsoft Power BI Desktop** dialog box, click on
    the **Save** button.

     ![](./media/image72.png)

22. In **Publish to Power BI** dialog box, select the databse. Then,
    click on the **Select** button.

     ![](./media/image73.png)
 
     ![](./media/image74.png)

23.  You now have a Power BI report of your SQL database data. Review the
    reports for accuracy.

## Exercise 9: Share data and manage access to your SQL database in Microsoft Fabric

### Task 1:Add users and assign licenses at the same time

1.  Open a new tab on your browser and enter the following link in the
    address bar +++https://go.microsoft.com/fwlink/p/?linkid=2024339+++

2.  On the Microsoft 365 admin center page, select **Add a user**

    ![](./media/image75.png)

3.  In the **Add a user** tab, enter the user details and click on
    the **Next** button:

    - **First name:** +++sample+++

    - **Last name**:+++user1+++

    - **User name**:+++sampleuser1+++

    ![](./media/image76.png)

4.  In the **Add a user** tab, under the **Assign product
    licenses** pane, select all licenses and click on
    the **Next** button.

    ![](./media/image77.png)

5.  Click on the **Next** button.

     ![](./media/image78.png)

6.  Review the user details and click on the **Finish adding** button.

     ![](./media/image79.png)

7.  Copy the **Username** and **Password** and paste them on a notepad,
    as you need them in the upcoming task.

     ![](./media/image80.png)

8.  Open your SQL Fabric workspace containing the database in the Fabric
    portal.

### Task 2: Give users access to workspaces

1.  On the **Fabric SQLDatabase** home page, select **Mange access**

     ![](./media/image81.png)

2.  In the **Manage access** tab, click on the **+Add people or
    groups** button 

     ![](./media/image82.png)

3.  In the **Add people** tab, enter the name or email
    of **sampleuser1**, select the **Admin** role, and then
    click **Add.**

     ![](./media/image83.png)
 
      ![](./media/image84.png)

### Task 3: Share data and manage access to your SQL database in Microsoft Fabric

1.  In the list of items, or in an open item, select
    the **Share** button.
     ![](./media/image85.png)

     ![](./media/image86.png)

2.  The **Grant people access** dialog opens. Enter the names of the
    people as +++sample user1+++ that need access.

3.  Choose whether to notify recipients with an email and add a message.
    Select **Grant**.

      ![](./media/image87.png)

    **Note:** The dialog offers a few simple options to grant broad access
    to the SQL database for scenarios where a database has been created for
    a single user or purpose. We'll skip checking any of those boxes here.

     ![](./media/image88.png)

4.  Select the **sqldb database** from the list.
     ![](./media/image89.png)

5.  The users now have access to connect to the database but are unable
    to do anything yet. The users can be added to SQL roles by
    selecting **Manage SQL security** from the **Security** menu in the
    database editor.

     ![](./media/image90.png)

6.  Select the **db_datareader** role and then click on  **Manage
    Access**.

     ![](./media/image91.png)

7.  To add the users, enter the +++**sample user1**+++ to the role and
    select **Add.**

     ![](./media/image92.png)

8.  Click on **Save** button.

    ![](./media/image93.png)

9.  Select the **db_datawriter** role and then click on  **Manage
    Access**.

     ![](./media/image94.png)

10. To add the users, enter the +++**sample user1**+++ to the role and
    select **Add.**

     ![](./media/image95.png)

11. Click on **Save** button

     ![](./media/image96.png)

12. Now close the **Manage SQL security**

     ![](./media/image97.png)

13. Open your browser, navigate to the address bar, and type or paste
    the following URL: **+++https://app.fabric.microsoft.com/+++** then
    press the **Enter** button.

14. In the **Microsoft Fabric** window, enter **sample
    User1** credentials(which you have saved in Task 1), and click on
    the **Submit** button.

     ![](./media/image98.png)

15. Then, In the **Microsoft** window enter the **sample
    User1** password and click on the **Sign in** button

     ![](./media/image99.png)

16. Then, in the **Update your password** window, update **sample
    User1’s** password and click on the **Sign in** button.

    ![](./media/image100.png)

17. Select **Workspaces** and select **SQLDatabaseXX**

    ![](./media/image101.png)
 
     ![](./media/image102.png)

    ![](./media/image103.png)

The users now have access to read and write every table within the
database. They won't have rights on any other Fabric items in the
workspace unless they have also been granted. Instead of the broad
roles, consider that users could be granted rights on individual tables
to follow the principle of least privilege.

### Task 4: Clean up resources

1.  In the left navigation bar, select the icon for your workspace to
    view all of the items it contains.

     ![](./media/image104.png)

2.  In the menu on the top toolbar, select **Workspace settings**.

     ![](./media/image105.png)

3.  In the **General** section, select **Remove this workspace**.

     ![](./media/image106.png)

    ![](./media/image107.png)
    
    ![](./media/image108.png)

## Summary

This lab offers a comprehensive walkthrough of building a SQL database
in Microsoft Fabric, showcasing its capabilities for modern data
warehousing and analytics. Participants learn to set up their
environment, create and query databases, load sample data, and build
reports using Power BI. The lab also introduces advanced features like
GraphQL API generation and secure data sharing. By completing the
exercises, users gain practical experience in leveraging Microsoft
Fabric for end-to-end data management and reporting workflows.
