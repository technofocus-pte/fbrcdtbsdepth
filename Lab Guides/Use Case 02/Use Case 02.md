# Use Case 02 – Build a Supply Chain Disruption Response App using SQL database in Microsoft Fabric

**Introduction**

In today's dynamic global economy, supply chain disruptions can
significantly impact business operations. To address these challenges,
Microsoft Fabric offers a powerful platform for building responsive,
data-driven applications. This tutorial demonstrates how to leverage
Microsoft Fabric's SQL capabilities to build a Supply Chain Disruption
Response App. Through a series of guided exercises, users will learn to
create and manage SQL databases, ingest and analyze data, and visualize
insights using integrated tools like Power BI and Fabric Notebooks.

**Objective**

- Create and configure a SQL database within Microsoft Fabric.

- Ingest and manipulate supply chain data using T-SQL and data
  pipelines.

- Perform advanced data analysis and create views for reporting and
  visualization.

- Utilize the SQL analytics endpoint and GraphQL API to build
  interactive applications.

- Develop and share insightful Power BI reports based on real-time
  supply chain data.

## ­Exercise 1 – Create a New Fabric Workspace

The objective is to create a new Fabric workspace. By the end,
participants will confidently set up, manage, and collaborate within
their own Fabric workspaces.

### **Task 1: Create a New Fabric Workspace**

You can use an existing workspace or create a new Fabric workspace.  In
workspaces, you create collections of items such as lakehouses,
warehouses, and reports. You must be a member of the Admin or Member
roles for the workspace to create a SQL database.

To create a workspace:

1.  In the **Fabric** home page, select **+New workspace**.

    ![](./media/image1.png)

2.  In the **Create a workspace tab**, enter the following details and
    click on the **Apply** button.

    |   |   |
    |---|---|
    |Name|	+++Supply Chain Analytics WorkspaceXX+++(XX can be a unique number)|
    |Advanced	|Under License mode, select Trial|


     ![](./media/image2.png)
 
    ![](./media/image3.png)
    ![](./media/image4.png)

## Exercise 2 – Create a SQL Database in Microsoft Fabric 

1.  In the Fabric Portal, click on **+ New Item**, search for **SQL
    databases**, and select **SQL database (preview) tile.**

     ![](./media/image5.png)

2.  Provide a name for the **New Database** as
    **+++supply_chain_analytics_database+++.** Select **Create** button.

    ![](./media/image6.png)

    ![](./media/image7.png)

## Exercise 3 – Ingest sample data and create objects and data 

### **Task-1: Open the Query Editor in the Fabric Portal**

1.  Once the new database is created, open the database's home page.
    Select **Sample Data**.

    ![](./media/image8.png)

2.  Check the Notifications area to ensure the import is complete before
    you proceed.

     ![](./media/image9.png)

3.  Notifications show you when the import of the sample data is
    complete. Your SQL database in Fabric now contains
    the SalesLT schema and associated tables.

     ![](./media/image10.png)

### **Task-2: Insert data using Transact-SQL**

The following steps use a T-SQL script to create a schema, table, and
data for the simulated data for supply chain analysis.

1.  Select the **New Query** button in the toolbar of the SQL database
    to create a new query.

    ![](./media/image11.png)

2.  Paste the following script in the Query area and select **Run** to
    execute it. The following T-SQL script

    a.  Creates a schema named **SupplyChain.**

    b.  Creates a table named **SupplyChain.Warehouse**.

    c.  Populates the **SupplyChain.Warehouse** table with some randomly
        created product data from **SalesLT.Product**.

        SQL
        
        /* Create the Tutorial Schema called SupplyChain for all tutorial objects */
        CREATE SCHEMA SupplyChain;
        GO
        
        /* Create a Warehouse table in the Tutorial Schema
        NOTE: This table is just a set of INT's as Keys,  
        tertiary tables will be added later
        */
        
        CREATE TABLE SupplyChain.Warehouse (
          ProductID INT PRIMARY KEY  -- ProductID to link to Products and Sales tables
        , ComponentID INT -- Component Identifier, for this tutorial we assume one per product, would normalize into more tables
        , SupplierID INT -- Supplier Identifier, would normalize into more tables
        , SupplierLocationID INT -- Supplier Location Identifier, would normalize into more tables
        , QuantityOnHand INT); -- Current amount of components in warehouse
        GO
        
        /* Insert data from the Products table into the Warehouse table. Generate other data for this tutorial */
        INSERT INTO SupplyChain.Warehouse (ProductID, ComponentID, SupplierID, SupplierLocationID, QuantityOnHand)
        SELECT p.ProductID,
            ABS(CHECKSUM(NEWID())) % 10 + 1 AS ComponentID,
            ABS(CHECKSUM(NEWID())) % 10 + 1 AS SupplierID,
            ABS(CHECKSUM(NEWID())) % 10 + 1 AS SupplierLocationID,
            ABS(CHECKSUM(NEWID())) % 100 + 1 AS QuantityOnHand
        FROM [SalesLT].[Product] AS p;
        GO
        

      ![](./media/image12.png)
     
      ![](./media/image13.png)

    Your SQL database in Fabric database now includes Warehouse information.
    You'll use this data in a later step in this tutorial.

3.  You can select these tables in the **Explorer** pane, and the table
    data is displayed – no need to write a query to see it.

      ![](./media/image14.png)

### **Task-3: Insert data using a Microsoft Fabric Pipeline**

Another way you can import data into and export data out of your SQL
database in Fabric is to use a Microsoft Fabric Data Pipeline. Data
pipelines offer an alternative to using commands, instead using a
graphical user interface. A data pipeline is a logical grouping of
activities that together perform a data ingestion task. Pipelines allow
you to manage extract, transform, and load (ETL) activities instead of
managing each one individually.

Microsoft Fabric Pipelines can contain a Dataflow. **Dataflow
Gen2** uses a Power Query interface that allows you to perform
transformations and other operations on the data. You'll use this
interface to bring in data from the *Northwind Traders* company, which
Contoso partners with. They're currently using the same suppliers, so
you'll import their data and show the names of these suppliers using a
view that you'll create in another step in this tutorial.

To get started, open the SQL database view of the sample database in the
Fabric portal.

1.  Select the **Get Data** button from the menu bar and Select **New
    Dataflow Gen2**.

    ![](./media/image15.png)
 
    ![](./media/image16.png)

2.  In the Power Query view, select the **Get Data** button. This starts
    a guided process rather than jumping to a particular data area.

    ![](./media/image17.png)

3.  In the search box of the **Choose Data Source**, view
    type **+++odata+++** and Select **OData** from the **New
    sources** results.

    ![](./media/image18.png)

4.  In the URL text box of the **Connect to data source** view, type the
    text: **+++https://services.odata.org/v4/northwind/northwind.svc/+++** for
    the Open Data feed of the **Northwind sample** database. Select
    the **Next** button to continue.

     ![](./media/image19.png)

5.  In the **Data Destination** section, make sure to check that the
    **SQL database** is connected.
    ![](./media/image20.png)

6.  Select the **Publish** button to start the data transfer.

     ![](./media/image21.png)

7.  You're returned to your Workspace view, where you can find the new
    Dataflow item.

    ![](./media/image22.png)
    ![](./media/image23.png)

8.  Select **supply_chain_analytics_database** SQL databse

      ![](./media/image24.png)

9.  Refresh the database by clicking on the three
    dots **(...)** beside **supply_chain_analytics_database ,** then
    navigate and click on **Refresh**.

     ![](./media/image25.png)

10. In the **Explorer**, expand the **dbo** schema to display the new
    table named **Suppliers**

      ![](./media/image26.png)

11. The data is now ingested into your database. You can now create a
    query that combines the data from the Suppliers table using this
    tertiary table. You'll do this later in our tutorial.

## Exercise 4 – Query the database 

### **Task-1: Transact-SQL Queries**

You can type Transact-SQL (T-SQL) statements in a query window.

1.  In ribbon of the database in the Fabric portal, select the **New
    Query** button.

      ![](./media/image27.png)

2.  Copy the following T-SQL script and paste it in the query window.
    This sample script performs a simple TOP 10 query, and creates a
    view based on a simple analytical T-SQL query. Select
    the **Run** button in the toolbar to execute the T-SQL query.
    ```
    -- Show the top 10 selling items 
    SELECT TOP 10
        [P].[ProductID],
        [P].[Name],
        SUM([SOD].[OrderQty]) AS TotalQuantitySold
    FROM [SalesLT].[Product] AS P
    INNER JOIN [SalesLT].[SalesOrderDetail] AS SOD ON [P].[ProductID] = [SOD].[ProductID]
    GROUP BY [P].[ProductID], [P].[Name]
    ORDER BY TotalQuantitySold DESC;
    GO
    
     /* Create View that will be used in the SQL GraphQL Endpoint */
    CREATE VIEW SupplyChain.vProductsbySuppliers AS
    SELECT COUNT(a.ProductID) AS ProductCount
    , a.SupplierLocationID
    , b.CompanyName
    FROM SupplyChain.Warehouse AS a
    INNER JOIN dbo.Suppliers AS b ON a.SupplierID = b.SupplierID
    GROUP BY a.SupplierLocationID, b.CompanyName;
    GO
    ```
    ![](./media/image28.png)
 
    ![](./media/image29.png)


### **Task-2: Performance Monitoring in SQL database in Fabric**

As your queries run in your SQL database in Fabric, the system collects
performance metrics to display in the **Performance Dashboard**. You can
use the Performance Dashboard to view database performance metrics, to
identify performance bottlenecks, and find solutions to performance
issues.

1.  On the **Home** toolbar in the Query with the SQL query editor,
    select **Performance summary**.

     ![](./media/image31.png)

2.  The entire performance summary is displayed such as CPU consumption,
    Allocated size, User connections etc.

    ![](./media/image32.png)

3.  To check the performance of Automatic Indexing, click on **View
    More** in the Automatic Index section.

    ![](./media/image33.png)

4.  In the Fabric portal, the **Automatic Index** tab shows a history
    and status of automatically created indexes

     ![](./media/image34.png)

### **Task-3: Backups in SQL database in Fabric** 

SQL database in Fabric automatically takes backups for you, and you can
see these backups in the properties that you access through the database
view of the Fabric portal.

1.  Click on the **Database editor**

    ![](./media/image35.png)

2.  Select the **Settings** icon in the toolbar.

    ![](./media/image36.png)

3.  Select the **Restore points** page. This view shows the recent point
    in time backups that have been taken on your database.

     ![](./media/image37.png)

4.  Click on the Close

    ![](./media/image38.png)

5.  Now, click on **Supply Chain Analytics Workspace** on the left-sided
    navigation menu.

     ![](./media/image39.png)

## Exercise 5 – Use the SQL analytics endpoint to query data 

### **Task-1: Query the data with the SQL analytics endpoint** 

You can query any of the mirrored data in the SQL analytics endpoint
using standard Transact-SQL statements that are compatible with a Fabric
warehouse. 

1.  You can access this mirrored data by selecting the SQL analytics
    endpoint in your Workspace view.

    ![](./media/image40.png)

     ![](./media/image41.png)

2.  On the **WideWorldImporters** page, go to the **Home** tab,
    select **SQL** from the drop down, and click on **New SQL query**.

      ![](./media/image42.png)
     
      ![](./media/image43.png)

3.  In this step, create a view over the mirrored data, and then create
    a report to show the results.

    Ensure you're in the SQL analytics endpoint, and then open a new Query
    window using the icon bar that depicts a paper with the
    letters **SQL** and paste the following Transact-SQL Code and
    select **Run** to execute it.

     **SQL**
    ```
    CREATE VIEW SupplyChain.vProductsBySupplier AS
    -- View for total products  each supplier
    SELECT sod.ProductID
    , sup.CompanyName
    , SUM(sod.OrderQty) AS TotalOrderQty
    FROM SalesLT.SalesOrderHeader AS soh
    INNER JOIN SalesLT.SalesOrderDetail AS sod 
        ON soh.SalesOrderID = sod.SalesOrderID
        INNER JOIN SupplyChain.Warehouse AS sc 
            ON sod.ProductID = sc.ProductID
            INNER JOIN dbo.Suppliers AS sup 
                ON sc.SupplierID = sup.SupplierID
    GROUP BY sup.CompanyName, sod.ProductID;
    GO
    CREATE VIEW SupplyChain.vSalesByDate AS
    -- Product Sales by date and month
    SELECT YEAR(OrderDate) AS SalesYear
    , MONTH(OrderDate) AS SalesMonth
    , ProductID
    , SUM(OrderQty) AS TotalQuantity
    FROM SalesLT.SalesOrderDetail AS SOD
    INNER JOIN SalesLT.SalesOrderHeader AS SOH 
        ON SOD.SalesOrderID = SOH.SalesOrderID
    GROUP BY YEAR(OrderDate), MONTH(OrderDate), ProductID;
    GO
    CREATE VIEW SupplyChain.vTotalProductsByVendorLocation AS
    -- View for total products by each supplier by location
    SELECT wh.SupplierLocationID AS 'Location'
    , vpbs.CompanyName AS 'Supplier'
    , SUM(vpbs.TotalOrderQty) AS 'TotalQuantityPurchased'
    FROM SupplyChain.vProductsBySupplier AS vpbs
    INNER JOIN SupplyChain.Warehouse AS wh
        ON vpbs.ProductID = wh.ProductID
    GROUP BY wh.SupplierLocationID, vpbs.CompanyName;
    GO   
    ```
   ![](./media/image44.png)
 
   ![](./media/image45.png)

4.  This T-SQL query creates three new SQL views,
    named **SupplyChain.vProductsBySupplier**, **SupplyChain.vSalesByDate**,
    and **SupplyChain.vTotalProductsByVendorLocation**

     ![](./media/image46.png)

You can now use these views in analytics and reporting. You will create
a report using these views in the further steps.

## Exercise 6 – Create and share visualizations.

### **Task-1: Find the connection strings to the SQL database.**

1.  Click on **supply_chain_analytics_database** database in the
    left-sided navigation bar.

     ![](./media/image47.png)

2.  To get your server and database name, open your SQL database in
    Fabric portal view and select the **Settings** button in the icon
    bar.

    ![](./media/image48.png)

3.  Select **Connection Strings** and you'll see a long string that
    starts with **Data Source...** From there, select the text between
    the characters **tcp:** through the characters **,1433**. Ensure
    that you select the entire set of characters there and nothing more
    for the server name.

     ![](./media/image49.png)

4.  For the database name, select all the characters between the
    characters **Initial Catalog=** and **;MultipleActiveResultSets**.

     ![](./media/image50.png)

You can now use these SQL strings in your connection area for tools such
as Power BI or SQL Server Management Studio. For Visual Studio Code with
the mssql extension, you can paste the entire connection string in the
first text box where you make a database connection, so you don't have
to select only the server and database names.

### **Task-2: Power BI visualization creation**

As you work with the SQL analytics endpoint, it creates a Data model of
the assets. This is an abstracted view of your data and how it's
displayed and the relationship between entities. Some of the defaults
the system takes might not be as you desire, so you'll now change one
portion of the data model for this SQL analytics endpoint to have a
specific outcome.

1.  Click on **supply_chain_analytics_database** SQL analytics endpoint
    in the left-sided navigation bar.

     ![](./media/image51.png)

2.  In your SQL analytics endpoint view, select the **Model
    layouts** button in the ribbon.

    ![](./media/image52.png)
 
     ![](./media/image53.png)

3.  From the resulting view, zoom in and scroll over until you see
    the **vTotalProductsByVendorLocation** object. Select it.

     ![](./media/image54.png)

4.  In the properties, select the **Location** field, and expand
    the **Advanced** properties section. You might need to scroll to
    find it. Set the value of **Summarize by** to **None**. This ensures
    that when the field is used, it's a discrete number, not a
    mathematical summarization of that number.

     ![](./media/image55.png)

### **Task-3: Create a report**

Here, the task is to create a report based on the views you created in
the SQL analytics endpoint in previous steps.

1.  Inside the SQL analytics endpoint view, select
    the **Reporting** button in the menu bar and then the **New
    report** button in the ribbon.

     ![](./media/image56.png)

2.  From the **New report with all available data** that appears, select
    the **Continue** button.

    ![](./media/image57.png)
 
    ![](./media/image58.png)

3.  Now the Power BI canvas appears
    ![](./media/image59.png)

4.  Expand the **vTotalProductsByVendor **data object. Select each of
    the fields you see there. The report takes a moment to gather the
    results to a text view. You can size this text box if desired.

      ![](./media/image60.png)
 
     ![](./media/image61.png)

5.  Select in a blank area of the report canvas, and then
    select **Location** in the **Data** fields area.

    ![](./media/image62.png)
 
     ![](./media/image63.png)

6.  Select a value in the box you just created – notice how the first
    selection of values follows the selection you make in the second
    box. Select that same value again to clear the selection.

     ![](./media/image64.png)

7.  Select in a blank area of the reporting canvas, and then select
    the **Supplier** field.

    ![](./media/image65.png)
 
    ![](./media/image66.png)

8.  Once again, you can select the name of a supplier and the first
    selection shows the results of just that supplier.

     ![](./media/image67.png)

### **Task-4: Save the Power BI item for sharing** 

You can save and share your report with other people in your
organization.

1.  Select the **Save** button in the icon box.

     ![](./media/image68.png)

2.  Name the report +++**suppliers_by_location_report**+++ and ensure
    you select the correct Workspace for this tutorial. Select **Save**
    button

    ![](./media/image69.png)
 
    ![](./media/image70.png)

3.  Select the **Share** button in the icon bar to share the report with
    people in your organization who have access to the proper data
    elements.

      ![](./media/image71.png)

4.  Enter the user +++sample user1+++, which you created in Use Case 1,
    and select **Send** button.

     ![](./media/image72.png)
   
     ![](./media/image73.png)

## Exercise 7 – Perform data analysis using Microsoft Fabric Notebooks

### **Task-1: Data analysis with T-SQL notebooks**

1.  Click on **Supply Chain Analytics Workspace** workspace in the
    left-sided navigation bar.

    ![](./media/image74.png)

2.  In the **Fabric** page, select **+New item**. Then, select
    **Notebook** tile.

    ![](./media/image75.png)

3.  In the icon bar, change the environment from **PySpark
    (Python)** to **T-SQL**.

     ![](./media/image76.png)

4.  In each code cell, there is a drop-down list for the code language.
    In the first cell in the Notebook, change the code language
    from **PySpark (Python)** to **T-SQL**.

     ![](./media/image77.png)

5.  Select the **+ Warehouses** button.

     ![](./media/image78.png)

6.  Select the **SQL analytics endpoint** object that is named
    **supply_chain_analytics_database**. Select **Confirm**.

    ![](./media/image79.png)

    ![](./media/image80.png)

7.  Expand the database, expand **Schemas**. Expand
    the **SupplyChain** schema. Expand **Views**, and locate the SQL
    view named **vProductsBySupplier**.

    ![](./media/image81.png)

8.  Select the ellipses next to that view. and select the option that
    says **SELECT TOP 100**.

     ![](./media/image82.png)

9.  This creates a cell with T-SQL code that has the statements
    pre-populated for you. Select the **Run Cell** button for the cell
    to run the query and return the results.

     ![](./media/image83.png)
 
     ![](./media/image84.png)

10. In the results, you can see not only the data requested, but buttons
    that allow you to view charts, save the data as another table,
    download, and more.
    ![](./media/image85.png)

11. To the side of the results you can see a new pane with quick
    **inspection** of the data elements, showing minimum and maximum
    values, missing data, and unique counts of the data returned.

    ![](./media/image86.png)
 
    ![](./media/image87.png)

12. Hovering between the code cells shows you a menu to add another
    cell. Select the **+ Markdown** button.

     ![](./media/image88.png)
 
      ![](./media/image89.png)

13. This places a text-based field where you can add information.
    Styling for the text is available in the icon bar, or you can select
    the \</\> button to work with Markdown directly. The result of the
    formatting show as a preview of the formatted text.

    ![](./media/image90.png)
    ![](./media/image9189.png)

14. Select the **Save As** icon in the ribbon.

     ![](./media/image92.png)

15. Enter the text **+++products_by_suppliers_notebook+++**. Ensure you set
    the location to your tutorial Workspace. Select the **Save** button
    to save the notebook.

     ![](./media/image93.png)
 
     ![](./media/image94.png)

## Exercise 8: Setup and Configure the GraphQL API

To create the API for GraphQL that you'll use for an application:

1.  Click on **Supply Chain Analytics Workspace** workspace in the
    left-sided navigation bar.

     ![](./media/image74.png)

2.  In the **Fabric** page, select **+New item**. Then, select **API for
    GraphQL** tile.

      ![](./media/image95.png)

3.  Enter the text **supplier_impact_gql** for the **Name** for your
    item and select **Create**.

     ![](./media/image96.png)

4.  Select **data source** card displayed to add the data for GraphQL.

     ![](./media/image97.png)

5.  On **Choose connectivity option** dialog box, select **Connect to
    Fabric data sources with single-on (SSo) authentication** and click
    on **Ok** button.

     ![](./media/image98.png)

6.  In the OneLake catalog tab, select the
    **supply_chain_analytics_database** and click on the '**Connect'**
    button

      ![](./media/image99.png)

7.  You are presented with a **Choose Data** panel. Scroll until you
    find ***SupplyChain.vProductsBySuppliers***, the view you created
    earlier in this tutorial Select it and click on **Load** button.

      ![](./media/image100.png)

8.  In the Query1 panel, replace the text you see there with the
    following GraphQL query string:

  +++query { vProductsbySuppliers(filter: { SupplierLocationID: { eq: 7 } }) { items { CompanyName SupplierLocationID ProductCount } } }+++

9.  Select the **Run **button in the Query1 window. The results of the
    GraphQL query are returned to the Results window in JSON format.

  ![](./media/image101.png)
 
   ![](./media/image102.png)

10. Select the **Copy** **endpoint** button in the ribbon.

    ![](./media/image103.png)

11. Select the **Copy button** when the Copy link panel appears. Store
    this string in a notepad or other location to be used in the sample
    application for this tutorial.

     ![](./media/image104.png)
      ![](./media/image105.png)
**Your API for GraphQL is now ready to accept connections and
requests. **

## Exercise 9 – Clean up resources

1.  In the left navigation bar, select the icon for your workspace to
    view all of the items it contains.

    ![](./media/image106.png)

2. In the menu on the top toolbar, select **Workspace settings**.

    ![](./media/image107.png)

3. In the **General** section, select **Remove this workspace**.

    ![](./media/image108.png)
    
    ![](./media/image109.png)

**Summary**

This usecase provides a comprehensive, hands-on guide to building a
Supply Chain Disruption Response App using Microsoft Fabric's SQL
database features. It walks users through creating a workspace, setting
up a SQL database, ingesting sample and external data, and performing
data transformations. Users also learn to create analytical views,
monitor performance, and visualize data using Power BI. The tutorial
concludes with the setup of a GraphQL API for application integration
and guidance on cleaning up resources. By completing this tutorial,
users gain practical experience in managing and analyzing supply chain
data within a unified analytics environment.


