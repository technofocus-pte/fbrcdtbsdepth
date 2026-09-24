---
lab:
  title: Use Case 3 – Build real-time analytics with Cosmos DB in Microsoft Fabric
  description: In this exercise, you will explore how Cosmos DB in Microsoft Fabric automatically mirrors your data to OneLake, allowing for cross-database querying. You will also learn how to Cosmos DB integrates with other services within Fabric, such as lakehouses, notebooks, power BI etc.
  duration: 180 minutes
  level: 300
  islab: true
  primarytopics:
    - Microsoft Fabric
    - Power BI
---

# Use Case 3 – Build real-time analytics with Cosmos DB in Microsoft Fabric 

## Introduction

This hands-on lab demonstrates how to create an operational data store,
implement streaming data pipelines, build cross-database analytics, and
deploy personalized recommendations using Reverse ETL patterns. Also,
learn how to build a complete real-time analytics solution using Cosmos
DB in Microsoft Fabric.

## Objectives

By the end of this session, learners will be able to:

- **Provision and configure** Cosmos DB in Microsoft Fabric as an
  operational data store

- **Implement real-time streaming** using Eventhouse and KQL for POS
  transaction data

- **Build cross-database analytics** leveraging Cosmos DB's automatic
  mirroring to OneLake

- **Create data warehouses** and perform ETL operations from streaming
  to structured data

- **Implement Reverse ETL** patterns to update operational systems with
  analytical insights

- **Deploy personalized recommendation models** using customer behavior
  data

## Exercise 1 – Set up the Environment

In this exercise, you set up the Microsoft Fabric environment needed to
complete the exercises in this lab. This includes creating a new Fabric
workspace, automated creation of a Fabric Warehouse, and loading data
into the Data Warehouse that will be used in later exercises.

### **Task-1: Create a New Fabric Workspace**

1.  Open a web browser and browse to +++https://app.fabric.microsoft.com/+++

2.  Sign in using the **email address** and click on **Submit**.

    ![A close up of a white and green object Description automatically
    generated](./media/image1.png)

3.  Enter **Password** to proceed with the sign in and click on **Sign
    in** button.

    ![A screenshot of a login box Description automatically
    generated](./media/image2.png)

4.  If prompted to stay signed in, select **Yes**.

    ![A screenshot of a computer Description automatically
    generated](./media/image3.png)

5.  You’ll be navigated to Microsoft Fabric Home Page. From the left
    navigation pane, select **Workspaces** and then select **+ New workspace**.

    ![A screenshot of a computer Description automatically
    generated](./media/image4.png)

6.  In the Create a workspace tab, enter the following details and click
    on the **Apply** button.

    - **Workspace name** -  +++Fourth Coffee Commerce - @lab.LabInstance.Id+++

    - **Advanced: Semantic Model storage format -** Small semantic model storage format

    - **Advanced: License mode –** Fabric capacity

    ![](./media/image5.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image6.png)

7.  You’ll be navigated to the workspace page.

    ![A screenshot of a computer Description automatically
    generated](./media/image7.png)

### **Task-2: Setup the Fabric Data Warehouse and a Fabric Lakehouse** 

1.  Click on **+ New item** on the workspace page.

    ![A screenshot of a computer Description automatically
    generated](./media/image8.png)

2.  From the flyout menu, search for +++lakehouse+++ and select the
    **lakehouse** tile.

    ![A screenshot of a computer Description automatically
    generated](./media/image9.png)

3.  Enter the lakehouse name as +++fc_commerce_lh+++

    ![A screenshot of a computer Description automatically
    generated](./media/image10.png)

4.  Once the lakehouse is created, select the **Upload Files** tile on
    the homepage.

    ![A screenshot of a computer Description automatically
    generated](./media/image11.png)

5.  Select the **files** **icon** on the upload files menu.

    ![A screenshot of a computer Description automatically
    generated](./media/image12.png)

6.  Navigate to this path-
    **C:\LabFiles\fbrcdtbsdepth-Fabric-Databases-Cloud-Slice\Lab
    Files\data\relational** and select all the files. Click on **Open**.

    ![](./media/image13.png)

7.  Click on **Upload**.

    ![A screenshot of a computer Description automatically
    generated](./media/image14.png)

8.  Once all files are uploaded, close the upload files menu.

    ![A screenshot of a computer Description automatically
    generated](./media/image15.png)

9.  Click on **Files** to verify if all the files are present.

    ![A screenshot of a computer Description automatically
    generated](./media/image16.png)

10. Select the **workspace** name from the left navigation menu to
    navigate to the workspace page.

    ![A screenshot of a computer Description automatically
    generated](./media/image17.png)

11. Create a **new item** in the workspace.

    ![A screenshot of a computer Description automatically
    generated](./media/image18.png)

12. Search for +++warehouse+++ and select the **warehouse** tile.

    ![A screenshot of a computer Description automatically
    generated](./media/image19.png)

13. Enter the warehouse name as +++fc_commerce_wh+++

    ![A screenshot of a login box Description automatically
    generated](./media/image20.png)

14. On the **get data** dropdown, select **ingest data with copy job**.

    ![](./media/image21.png)

15. With the default copy job name, click on **Create**.

    ![A screenshot of a computer Description automatically
    generated](./media/image22.png)

16. In the **choose data source** section, select **fc_commerce_lh**
    from **OneLake catalog.**

    ![A screenshot of a computer Description automatically
    generated](./media/image23.png)

17. In the Choose data section, select **Files** and make sure all the
    files are selected from the list. Once done, click on **Next** to
    proceed.

    ![](./media/image24.png)

18. Proceed with **Full copy** method and click on **Next**.

    ![A screenshot of a computer Description automatically
    generated](./media/image25.png)

19. Click on **Next** on **Map destination** section.

    ![A screenshot of a computer Description automatically
    generated](./media/image26.png)

20. Review and select **Save + run.**

    ![A screenshot of a computer Description automatically
    generated](./media/image27.png)

21. Wait until the **copy job status** is changed to **succeeded**.

    ![A screenshot of a computer Description automatically
    generated](./media/image28.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image29.png)

22. Navigate to **fc_commerce_wh** warehouse and expand the **dbo > Tables**. You will see all the tables successfully loaded in the
    warehouse. Click on any table to view its contents.

    ![A screenshot of a computer Description automatically
    generated](./media/image30.png)

## Exercise 2 – Provisioning Cosmos DB in Fabric (Operational Data Store)

In this exercise, you will create a Cosmos DB database in Microsoft
Fabric to serve as the operational data store for Fourth Coffee's
commerce application.

### **Task-1: Create the database and the containers**

1.  From the left navigation pane, select workspace icon as **Fourth Coffee Commerce - 0910** and again, select the **workspace** name.

    ![](./media/image31.png)

2.  From the top menu ribbon, select **+ New item**, a pane will open on
    the right side.

    ![](./media/image32.png)

3.  On the filter text box on the top right of the pane, type
    ‘**cosmos**’ to filter the list of items. Select **Cosmos DB
    database**.

    ![A screenshot of a computer Description automatically
    generated](./media/image33.png)

4.  Name the new database +++fc_commerce_cosmos+++ and select **Create**.

    ![](./media/image34.png)

5.  Once the Cosmos DB database has been created, it will open in a new
    tab in Fabric. On the left explorer pane, select **+ New
    container**.

    ![A screenshot of a computer Description automatically
    generated](./media/image35.png)

6.  In the New container pane that opens on the right side, provide the
    following details to create a new customers container:

    - **Container id**: +++customers+++

    - **Partition key**: +++/customerId+++

    Select **OK** to create the container.

    ![A screenshot of a computer Description automatically
    generated](./media/image36.png)

### **Task-2: Load Initial data**

You will now load initial data into the Cosmos DB container you just
created by uploading JSON file.

1.  On the left explorer pane, expand the **customers** container to
    open it then select **items**.

    ![](./media/image37.png)

2.  From the top menu ribbon, select **Upload item**.

    ![A screenshot of a computer Description automatically
    generated](./media/image38.png)

3.  On the **Upload item** pane that opens on the right side, select the
    folder icon to browse for the file to upload.

    ![A screenshot of a computer Description automatically
    generated](./media/image39.png)

4.  On the file picker dialog, navigate to the lab files folder in the C
    drive at **C:\LabFiles\fbrcdtbsdepth-Fabric-Databases-Cloud-Slice\Lab
    Files\data\nosql**,
    select the file **customers_container.json** and then
    select **Open**.

    ![A screenshot of a computer Description automatically
    generated](./media/image40.png)

5.  Back on the **Upload item** pane, select **Upload** to upload the
    file.

    ![A screenshot of a computer Description automatically
    generated](./media/image41.png)

6.  Once the upload is complete, you will see the items listed in the
    container.

  ![A screenshot of a computer Description automatically
  generated](./media/image42.png)

### **Task-3: Query the data** 

Let's perform some queries against the data you just uploaded to verify
that everything is working as expected.

1.  Select the **Items** tab inside the **customers** container. On the
    top menu ribbon, select **New SQL query**.

    ![A screenshot of a computer Description automatically
    generated](./media/image43.png)

2.  In the query editor that opens, enter the following query to
    retrieve the top ten high value customers based on their total
    loyalty points:

    ```
    SELECT TOP 10
    c.customerId,    
    c.name,    
    c.loyaltyPoints,    
    c.preferences.airport    
    FROM c  
    ORDER BY c.loyaltyPoints DESC
    ```
 
    ![A screenshot of a computer Description automatically
    generated](./media/image44.png)

3.  Select the **Execute Query** button on the top menu ribbon to run
    the query. You should see the results displayed below the query
    editor.

    ![A screenshot of a computer Description automatically
    generated](./media/image45.png)
 
    You can select the **View** dropdown on the top menu ribbon then
    select **Vertical** to change the results view to vertical for better
    readability.
 
    ![](./media/image46.png)

4.  Create another new SQL query in the same **customers** container to
    analyze customer recommendations.

    ![A screenshot of a computer Description automatically
    generated](./media/image47.png)

5.  Enter the following query in the query editor and execute it by
    clicking on the **Execute query** button.

    ```
    SELECT c.customerId,
    (SELECT VALUE COUNT(1) FROM r IN c.recommendations) AS recommendationSets,
    (SELECT VALUE COUNT(1) FROM r IN c.recommendations JOIN mi IN r.menuItems) AS totalRecommendedItems,
    (SELECT VALUE ROUND(AVG(r.score), 4) FROM r IN c.recommendations) AS avgRecScore,
    (SELECT VALUE MIN(r.expiresAt) FROM r IN c.recommendations) AS nextExpiryUtc
    FROM customers c
    ```
 
    This query uses correlated subqueries to count recommendation sets,
    total recommended items, the average recommendation score (rounded to
    four decimals), and the next recommendation expiry for each customer.
    
    Cosmos DB in Fabric supports rich querying capabilities including
    subqueries *(as demonstrated above)*, aggregate functions, scalar
    expressions, and more, enabling you to perform complex data analysis
    directly within the database.
 
    ![](./media/image48.png)

6.  Create another new SQL query in the same **customers** container.

    ![](./media/image49.png)

7.  In a new query editor, enter the following query to demonstrate
    scalar expressions. This query calculates a customer's membership
    tier based on their total loyalty points:

    ```
    SELECT c.customerId,
    c.name,
    c.loyaltyPoints,
        IIF(c.loyaltyPoints >= 1000, "Gold",
            IIF(c.loyaltyPoints >= 500, "Silver", "Bronze")) AS membershipTier
    FROM customers c
    ```
 
    Execute the query by clicking on the **Execute query** button.
 
    ![](./media/image50.png)

## Exercise 3 – Cross-Database Analytics (Cosmos DB to Data Warehouse)

In this exercise, you will explore how Cosmos DB in Microsoft Fabric
automatically mirrors your data to OneLake, allowing for cross-database
querying. You will also learn how to Cosmos DB integrates with other
services within Fabric, such as lakehouses, notebooks, power BI etc.

### **Task-1: Access Cosmos DB mirrored data in OneLake**

1.  First, check the status of replication for your Cosmos DB database
    in OneLake. In the **fc_commerce_cosmos** Cosmos DB, select
    the **Replication** tab from the top menu bar, then select **Monitor
    replication**.

    ![](./media/image51.png)

2.  This opens a pane on the right showing the replication status.

    ![A screenshot of a computer Description automatically
    generated](./media/image52.png)

3.  Close the replication status pane.

    ![](./media/image53.png)

4.  From the top menu bar select the **Cosmos DB** dropdown and then
    select **SQL Endpoint**.

    ![A screenshot of a computer Description automatically
    generated](./media/image54.png)

5.  This opens a new tab in Fabric to the SQL analytics endpoint page
    for your Cosmos DB database. In the left explorer pane,
    expand **Schemas > fc_commerce_cosmos >  Tables**.

    Here you will see the **customers container** you created in Exercise
    2, represented as a table.

    ![A screenshot of a computer Description automatically
    generated](./media/image55.png)

6.  From the top menu ribbon, select **New SQL query**. In the query
    editor that opens, enter the following query to analyze customer
    preferences by grouping customers by their favorite drink:

    ```
    SELECT
    JSON_VALUE(c.preferences, '$.favoriteDrink') AS drink,
    COUNT(1) AS customerCount
    FROM [fc_commerce_cosmos].[fc_commerce_cosmos].[customers] AS c
    GROUP BY JSON_VALUE(c.preferences, '$.favoriteDrink')
    ORDER BY COUNT(1) DESC
    ```
 
    ![A screenshot of a computer Description automatically
    generated](./media/image56.png)

7.  Select **Run** to execute the query. The results will show the most
    popular drinks ordered by customer count, with the most popular
    drinks appearing first.

    ![](./media/image57.png)
    
    The SQL analytics endpoint allows you to run T-SQL queries against
    your mirrored Cosmos DB data, making it easy to perform analytics
    without impacting your operational workloads.

### **Task-2: Cross-database querying Cosmos DB and Fabric Data Warehouse**

1.  In the same analytics endpoint tab, select **+ Warehouses** in the
    Explorer pane.

    ![](./media/image58.png)

2.  Then, select the **fc_commerce_wh** Data Warehouse you created in
    the Fabric Environment Setup.

    ![A screenshot of a computer Description automatically
    generated](./media/image59.png)

3.  Open a new SQL query editor, then enter the following query to
    analyze which favorite drinks drive the most revenue and loyalty
    engagement by combining data from both Cosmos DB and the Data
    Warehouse:

    ```
    SELECT
        JSON_VALUE(c.preferences, '$.favoriteDrink') AS FavoriteDrink,
        COUNT(DISTINCT c.customerId) AS TotalCustomers,
    
        AVG(
            TRY_CAST(c.loyaltyPoints AS decimal(18,2))
        ) AS AvgLoyaltyPoints,
    
        SUM(
            COALESCE(
                TRY_CAST(fs.TotalAmount AS decimal(18,2)),
                CAST(0 AS decimal(18,2))
            )
        ) AS TotalRevenue,
    
        SUM(
            COALESCE(
                TRY_CAST(fs.LoyaltyPointsEarned AS decimal(18,2)),
                CAST(0 AS decimal(18,2))
            )
        ) AS TotalPointsEarned,
    
        SUM(
            COALESCE(
                TRY_CAST(fs.LoyaltyPointsRedeemed AS decimal(18,2)),
                CAST(0 AS decimal(18,2))
            )
        ) AS TotalPointsRedeemed,
    
        AVG(
            TRY_CAST(fs.TotalQuantity AS decimal(18,2))
        ) AS AvgItemsPerOrder,
    
        COUNT(DISTINCT fs.TransactionId) AS TotalTransactions
    
    FROM [fc_commerce_cosmos].[fc_commerce_cosmos].[customers] AS c
    
    LEFT JOIN [fc_commerce_wh].[dbo].[DimCustomer] AS dc
        ON dc.CustomerId = c.customerId
    
    LEFT JOIN [fc_commerce_wh].[dbo].[FactSales] AS fs
        ON fs.CustomerKey = dc.CustomerKey
    
    WHERE JSON_VALUE(c.preferences, '$.favoriteDrink') IS NOT NULL
    
    GROUP BY JSON_VALUE(c.preferences, '$.favoriteDrink')
    
    ORDER BY TotalRevenue DESC;
    ```

4.  Select **Run** to execute the query. This cross-database query
    demonstrates the power of Fabric's unified analytics platform by
    seamlessly joining:

    - Customer preference data from Cosmos DB (customers container)

    - Sales transaction data from the Data Warehouse (FactSales table)

    - Menu item data from Cosmos DB (menuitems container)

    ![](./media/image150.png)

### **Task-3: Access mirrored data from a Fabric Lakehouse**

1.  Navigate back to the workspace by clicking on the **Workspaces**
    option from the left pane and select the workspace name again.

    ![](./media/image62.png)

2.  Select the existing lakehouse **fc_commerce_lh** you created in the
    Fabric Environment Setup.

    ![A screenshot of a computer Description automatically
    generated](./media/image63.png)

3.  In the lakehouse, select **Get data**, then select **New table shortcut**.

  ![](./media/image151.png)

4.  In the **New shortcut** dialog, select **Microsoft OneLake**.

    ![A screenshot of a computer Description automatically
    generated](./media/image65.png)

5.  In the next dialog, select the **fc_commerce_cosmos** database from
    the list of available OneLake data sources, then select **Next**.

    ![](./media/image66.png)

6.  Make sure **Passthrough Identity** is selected and click on **Connect**.

   ![](./media/image152.png)
   
8.  On the next page, expand **Tables** and then **fc_commerce_cosmos** schema, then select the
    mirrored **customers** container. Select **Next**.

    ![](./media/image67.png)

9.  On the final page, review the summary and select **Create** to
    create the table shortcuts in the lakehouse.

    ![A screenshot of a computer Description automatically
    generated](./media/image68.png)

10.  The data is now loaded. Once the data is in the lakehouse, you can
    use it in various Fabric services. For example, you can create a
    notebook to analyze the data.

     ![A screenshot of a computer Description automatically
    generated](./media/image69.png)

11.  From the top menu ribbon, select the **Analyse data with** dropdown, then
    select **Notebook > New notebook**.

![](./media/image153.png)

11. Hover below the existing code cell and click on the **+ Code** to add
    a new code cell.

    ![A screenshot of a computer Description automatically
    generated](./media/image71.png)

12. Enter the following code and run it:

    ```
      display(spark.sql("""
    SELECT
        get_json_object(c.preferences, '$.favoriteDrink') AS FavoriteDrink,
        COUNT(*) AS CustomerCount,
        AVG(CAST(c.loyaltyPoints AS float)) AS AvgLoyaltyPoints
    FROM customers AS c
    WHERE get_json_object(c.preferences, '$.favoriteDrink') IS NOT NULL
    GROUP BY get_json_object(c.preferences, '$.favoriteDrink')
    ORDER BY CustomerCount DESC
    """))
    ```
 
    This code uses Spark SQL to query the mirrored Cosmos DB data in the
    lakehouse, analyzing customer preferences by favorite drink.
    
    ![A screenshot of a computer Description automatically
    generated](./media/image72.png)

13. **Run** the code to see the output.

    ![A screenshot of a computer Description automatically
    generated](./media/image73.png)

## Exercise 4 – Real-Time Streaming of POS Events

In this exercise, you will ingest and query the streaming data and use
the Kusto Query Language (KQL) to analyze it.

### **Task-1: Create an Eventhouse and ingest the data**

1.  Navigate to **Fourth Coffee Commerce – 0910 workspace** by clicking
    on the workspace icon from the left navigation pane. Then, click on
    the workspace name again.

    ![A screenshot of a computer Description automatically
    generated](./media/image74.png)

2.  You will now create an Eventhouse to ingest and store the data.
    Navigate to your Fabric workspace and select **+ New item** from the
    top menu ribbon.

    ![A screenshot of a computer Description automatically
    generated](./media/image75.png)

3.  In the **New item** pane that opens on the right side, type
    +++eventhouse+++ in the filter text box on the top right of the pane
    to filter the list of items. Select **Eventhouse**.

    ![A screenshot of a computer Description automatically
    generated](./media/image76.png)

4.  Name the new Eventhouse +++fc_commerce_eventhouse+++ and
    select **Create**.

    ![A screenshot of a computer Description automatically
    generated](./media/image77.png)

5.  Once the Eventhouse has been created, it will open in a new tab in
    Fabric. Select **fc_commerce_eventhouse** under the KQL databases.

    ![A screenshot of a computer Description automatically
    generated](./media/image78.png)

6.  Navigate to **Get data** dropdown and choose **local file**.

    ![A screenshot of a computer Description automatically
    generated](./media/image79.png)

7.  In the Get data window, create a **new table**.

    ![A screenshot of a computer Description automatically
    generated](./media/image80.png)

8.  Name the new table as +++transactions_live+++ and click on **tick**
    **icon** beside the name.

    ![A screenshot of a computer Description automatically
    generated](./media/image81.png)

9.  Browse for file in the **LabFiles** folder in C: drive.

    Here’s the path: **C:\LabFiles\fbrcdtbsdepth-Fabric-Databases-Cloud-Slice\LabFiles\data\streaming**
    
    ![A screenshot of a computer Description automatically
    generated](./media/image82.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image83.png)

10. Once it is uploaded, click on **Next**.

    ![A screenshot of a computer Description automatically
    generated](./media/image84.png)

11. Inspect the data and click on **Finish**.

    ![A screenshot of a computer Description automatically
    generated](./media/image85.png)

12. This will take 15-20 seconds to load the status to **successfully ingested**. 
    Once it’s done, click on **Close**.

    ![A screenshot of a computer Description automatically
    generated](./media/image86.png)

13. Select the **transactions_live** table and preview the data. The
    data is now successfully ingested.

    ![A screenshot of a computer Description automatically
    generated](./media/image87.png)

### **Task-2: Verify live ingestion into Eventhouse and run KQL queries**

1.  In the database, with the **transactions_live** selected, select
    **Query with code** dropdown and select **Show any 100 records**
    from the dropdown.

    ![A screenshot of a computer Description automatically
    generated](./media/image88.png)

2.  In the new query editor tab, select **Run** to verify that data is
    being ingested into the Eventhouse in real-time. You’ll see 100
    records returned as the output.

    ![](./media/image89.png)

### **Task-3: Build Silver Layer for Analytics**

1.  Replace the existing query in the query editor with the following
    KQL code to create a Silver layer table that aggregates total sales
    by menu item:

    ```
    .create-or-alter function with (folder="Silver") vw_Pos_Sales() {
    transactions_live
    // best available event timestamp
    | extend EventTs = coalesce(
        todatetime(column_ifexists("timestamp", datetime(null))),
        todatetime(column_ifexists("EventEnqueuedUtcTime", datetime(null))),
        todatetime(column_ifexists("EventProcessedUtcTime", datetime(null))),
        ingestion_time()
    )
    // purchases only
    | where tostring(column_ifexists("transactionType","")) == "purchase"
    // project needed fields and make sure Items is an array (empty if missing)
    | project
        TransactionId = tostring(column_ifexists("transactionId","")),
        EventTs,
        CustomerId = tostring(column_ifexists("customerId","")),
        ShopId     = tostring(column_ifexists("shopId","")),
        PaymentMethod = tostring(column_ifexists("paymentMethod","")),
        Items = iif(isnull(column_ifexists("items", dynamic(null))), dynamic([]), column_ifexists("items", dynamic(null))),
        // optional top-level totals (may be null)
        TotalQuantity_t = toint(column_ifexists("totalQuantity", int(null))),
        TotalAmount_t   = todouble(column_ifexists("totalAmount",  real(null))),
        LoyaltyPointsEarned   = toint(coalesce(column_ifexists("loyaltyPointsEarned", int(null)), 0)),
        LoyaltyPointsRedeemed = toint(coalesce(column_ifexists("loyaltyPointsRedeemed", int(null)), 0))
    // explode items (works even if empty array)
    | mv-expand Items to typeof(dynamic)
    | extend
        qty  = toint(coalesce(Items.quantity, 0)),
        unit = todouble(coalesce(Items.unitPrice, 0.0))
    | extend
        lt   = todouble(coalesce(Items.totalPrice, qty * unit))
    // roll up per transaction
    | summarize
        TotalQuantity_i = sum(qty),
        TotalAmount_i   = sum(lt),
        EventTimestamp  = any(EventTs),
        CustomerId      = any(CustomerId),
        ShopId          = any(ShopId),
        PaymentMethod   = any(PaymentMethod),
        LoyaltyPointsEarned   = any(LoyaltyPointsEarned),
        LoyaltyPointsRedeemed = any(LoyaltyPointsRedeemed),
        TotalQuantity_t = any(TotalQuantity_t),
        TotalAmount_t   = any(TotalAmount_t)
    by TransactionId
    // prefer top-level totals when present; else use item-derived
    | extend
        TotalQuantity = iif(isnotnull(TotalQuantity_t), TotalQuantity_t, TotalQuantity_i),
        TotalAmount   = iif(isnotnull(TotalAmount_t),   TotalAmount_t,   TotalAmount_i),
        DateKey   = toint(format_datetime(EventTimestamp, "yyyyMMdd")),
        TimeKey   = toint(format_datetime(EventTimestamp, "HHmmss")),
        CreatedAt = EventTimestamp
    | project
        TransactionId, DateKey, TimeKey,
        CustomerId, ShopId,
        TotalQuantity, TotalAmount,
        PaymentMethod,
        LoyaltyPointsEarned, LoyaltyPointsRedeemed,
        CreatedAt
    }
    ```

    ```
    .create-or-alter function with (folder="Silver") vw_Pos_LineItems_Sales() {
    transactions_live
    | extend EventTs = coalesce(
        todatetime(column_ifexists("timestamp", datetime(null))),
        todatetime(column_ifexists("EventEnqueuedUtcTime", datetime(null))),
        todatetime(column_ifexists("EventProcessedUtcTime", datetime(null))),
        ingestion_time()
    )
    | where tostring(column_ifexists("transactionType","")) == "purchase"
    | extend Items = column_ifexists("items", dynamic(null))
    | where isnotnull(Items)
    | mv-expand Items to typeof(dynamic)
    | extend
        TransactionId  = tostring(column_ifexists("transactionId","")),
        EventTimestamp = EventTs,
        CustomerId     = tostring(column_ifexists("customerId","")),
        ShopId         = tostring(column_ifexists("shopId","")),
        MenuItemKey    = toint(coalesce(Items.menuItemKey, int(null))),
        MenuItemId     = tostring(Items.menuItemId),
        MenuItemName   = tostring(Items.name),
        Size           = tostring(coalesce(Items.size, "")),
        Quantity       = toint(coalesce(Items.quantity, 1)),
        UnitPrice      = todouble(coalesce(Items.unitPrice, 0.0))
    | extend LineTotal    = todouble(coalesce(Items.totalPrice, Quantity * UnitPrice)),
            PaymentMethod = tostring(column_ifexists("paymentMethod",""))
    | order by TransactionId asc, EventTimestamp asc, MenuItemId asc, Size asc
    | serialize
    | extend LineNumber = row_number(1, prev(TransactionId) != TransactionId)
    | extend
        DateKey  = toint(format_datetime(EventTimestamp, "yyyyMMdd")),
        TimeKey  = toint(format_datetime(EventTimestamp, "HHmmss")),
        CreatedAt = EventTs
    | project
        TransactionId, LineNumber,
        DateKey, TimeKey,
        // keep if you merge to Customer/Shop keys upstream; drop if not needed
        CustomerId, ShopId,
        MenuItemKey, MenuItemId, MenuItemName,
        Quantity, UnitPrice, LineTotal,
        PaymentMethod, Size, CreatedAt
    }
    ```
 
    ![](./media/image90.png)

2.  Execute each query one by one by clicking on the **Run** button.

    **Note: when you click on the query, it will get highlighted in blue
    and you’ll notice that only one section of the query is getting
    highlighted at a time. This way, you can run the query in two
    sections.**
 
    ![A screenshot of a computer Description automatically
    generated](./media/image91.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image92.png)

3.  Open a new query tab by clicking on the **+** icon.

    ![A screenshot of a computer Description automatically
    generated](./media/image93.png)

4.  You can now query the Silver layer views to perform analytics on the
    ingested streaming data. For example, to get total sales by menu
    item, use the following query:

    ```
    vw_Pos_LineItems_Sales()
    | summarize TotalSales = sum(LineTotal), TotalQuantity = sum(Quantity) by MenuItemId, MenuItemName
    | order by TotalSales desc
    ```
 
    ![A screenshot of a computer Description automatically
    generated](./media/image94.png)

5.  Select **Run** to execute the query and view the results.

    ![A screenshot of a computer Description automatically
    generated](./media/image95.png)

## Exercise 5 – Implement Reverse ETL and Build Personalization Model

In this exercise, you will work with Fabric Notebooks to extract and
transform data from the Eventhouse, update the user profiles in Cosmos
DB, and then use that data to build a personalization model.

### **Task-1: Create Data Warehouse Views**

1.  Navigate to **Fourth Coffee Commerce workspace** icon and
    select the Data Warehouse **fc_commerce_wh** where you want to
    create views.

    ![A screenshot of a computer Description automatically
    generated](./media/image96.png)

2.  Create a new SQL query by selecting the **New SQL Query** button in
    the warehouse page.

    ![A screenshot of a computer Description automatically
    generated](./media/image97.png)

3.  In the query window editor, paste the following SQL code to create
    views in the warehouse:

    ```
    CREATE OR ALTER VIEW dbo.vDimCustomerKey AS 
    SELECT CustomerId, CustomerKey, IsActive FROM dbo.DimCustomer
    GO

    CREATE OR ALTER VIEW dbo.vDimShopKey AS 
    SELECT ShopId, ShopKey, IsActive FROM dbo.DimShop
    GO

    CREATE OR ALTER VIEW dbo.vDimMenuItemKey AS
    SELECT menuItemId AS MenuItemId, menuItemKey AS MenuItemKey, isActive AS IsActive
    FROM dbo.DimMenuItem;

    CREATE OR ALTER VIEW dbo.vFactSalesMaxKey AS 
    SELECT
    MaxSalesKey = COALESCE(MAX(SalesKey), 0),
    ExistingTxnCount = COUNT(*)
    FROM dbo.FactSales
    GO
    ```
 
    ![A screenshot of a computer Description automatically
    generated](./media/image98.png)

4.  Highlight each command one at a time as shown in the image below and
    select **Run** to execute each command and create the views in the
    warehouse.

    ![A screenshot of a computer Description automatically
    generated](./media/image99.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image100.png)

5.  To verify if the view are created successfully with these queries,
    you can navigate to **dbo > Views.** As you have executed four
    queries for the views, hence four views have been created.

    ![A screenshot of a computer Description automatically
    generated](./media/image101.png)

### **Task-2: Perform Transformation with Fabric Notebooks**

1.  Navigate to the workspace page.

    ![A screenshot of a computer Description automatically
    generated](./media/image102.png)

2.  From the top menu ribbon, select **Import** and select **Notebook**, then **From this computer**.

    ![](./media/image103.png)

3.  In the Import status pane, select **Upload**.

    ![](./media/image104.png)

4.  In the file picker dialog, navigate to the location of this
    **LabFiles** folder on your computer, labeled **src**, then select
    both the **transform_transactions.ipynb** notebook file and
    the **build_personalization_model.ipynb** files, then
    select **Open** to upload them.

    ![](./media/image105.png)

5.  Once the notebooks have been uploaded, they will appear in the
    workspace content list. Select
    the **transform_transactions.ipynb** notebook to open it.

    ![](./media/image106.png)

6.  On the left side of the notebook, you'll see the Explorer pane,
    select **Add data items** and then select **OneLake catalog** from the dropdown.

    ![A screenshot of a computer Description automatically
    generated](./media/image107.png)

7.  Select **fc_commerce_lh** to add the Lakehouse as a data source to
    the notebook and click on **Connect**.

    ![](./media/image108.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image109.png)

8.  In the notebook, the first code cell only where you need to provide
    your Fabric environment details. Update the following variables with
    your specific information:

    ![A screenshot of a computer Description automatically
    generated](./media/image110.png)

    - **kustoCluster**: The Kusto cluster URL for your Eventhouse. You can
    find this in the Eventhouse you configured earlier
    named **fc_commerce_eventhouse** In the **Database Details** pane to
    the right, select **Copy Query URI**.

    ![A screenshot of a computer Description automatically
    generated](./media/image111.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image112.png)

    - **workspace_guid**: Copying the workspace GUID from the browser address bar when you have your Fabric
    workspace open (it is the first GUID in the URL, you can refer the below image).

    ![](./media/image113.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image114.png)

    - **WAREHOUSE**: Enter **fc_commerce_wh**. You can find this by
    navigating to your data warehouse in Fabric and copying the database
    name from the top of the page.

    ![A screenshot of a computer Description automatically
    generated](./media/image115.png)

    - **lakehouse_guid**: Copy the lakehouse GUID
    from the browser address bar when you have your lakehouse open (it is
    the second GUID in the URL, you can refer to the image below).

    ![A screenshot of a computer Description automatically
    generated](./media/image116.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image117.png)

    - **SERVER**: The SQL endpoint of your data warehouse. You can find this
    by navigating to your data warehouse in Fabric,
    selecting **Settings** from the top menu ribbon, and copying the SQL
    endpoint from there.

    ![A screenshot of a computer Description automatically
    generated](./media/image118.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image119.png)

9.  After updating the variables, run all the cells in the notebook
    sequentially to perform the data transformation and loading process.
    You can do this by selecting **Run All** from the top menu ribbon.

    ![A screenshot of a computer Description automatically
    generated](./media/image120.png)

10. Once the notebook has finished running, you should see two lakehouse
    urls printed at the end of the notebook execution. These URLs point
    to the Parquet files that were created in the lakehouse as part of
    the transformation process. You will use these files to load data
    into the data warehouse.

    ![A screenshot of a computer Description automatically
    generated](./media/image121.png)

### **Task-3: Load Transformed Data into Data Warehouse**

1.  Navigate to **Data warehouse** (**fc_commerce_wh**) tab from the top.

    ![A screenshot of a computer Description automatically
    generated](./media/image122.png)

2.  Open a new SQL query in your data warehouse by selecting the **New SQL Query** button in the warehouse page.

    ![A screenshot of a computer Description automatically
    generated](./media/image123.png)

3.  In the query window editor, paste the following SQL code to load the
    transformed data into the **FactSales** and **FactSalesLineItem**
    tables using the Parquet files created in the previous step.

    **Note**: **Make sure to replace the placeholder URLs with the actual
    URLs printed at the end of the notebook execution.**
    
    ```
    -- =============================
    -- COPY headers (FactSales)
    -- =============================
    COPY INTO dbo.FactSales
    (
        SalesKey             1,
        TransactionId        2,
        DateKey              3,
        TimeKey              4,
        CustomerKey          5,
        ShopKey              6,
        TotalQuantity        7,
        TotalAmount          8,
        PaymentMethod        9,
        LoyaltyPointsEarned 10,
        LoyaltyPointsRedeemed 11,
        CreatedAt           12
    )
    FROM '[lakehouse sales url]' 
    WITH (
        FILE_TYPE = 'PARQUET'
    );

    -- =============================
    -- COPY lines (FactSalesLineItems)
    -- =============================
    COPY INTO dbo.FactSalesLineItems
    (
        TransactionId  1,
        SalesKey       2,
        LineNumber     3,
        DateKey        4,
        TimeKey        5,
        MenuItemKey    6,
        Quantity       7,
        UnitPrice      8,
        LineTotal      9,
        PaymentMethod 10,
        Size          11,
        CreatedAt     12
    )
    FROM '[lakehouse items urls]' 
    WITH (
        FILE_TYPE = 'PARQUET'
    );
    ```

    ![A screenshot of a computer Description automatically
    generated](./media/image124.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image125.png)

4.  Select **Run** to execute the query and load the data into the warehouse.

    ![A screenshot of a computer Description automatically
    generated](./media/image126.png)

5.  Select the **New SQL query**.

6.  Verify the data in the warehouse by running a few SELECT queries
    against the FactSales and **FactSalesLineItems** tables.

    +++SELECT TOP 10 * FROM dbo.FactSales order by CreatedAt desc;+++
 
    +++SELECT TOP 10 * FROM dbo.FactSalesLineItem order by CreatedAt desc;+++
 
    ![A screenshot of a computer Description automatically
    generated](./media/image127.png)

7.  Highlight each query and click on **Run** to execute each query at a
    time. You’ll see the output of **top 10 records returned from FactSales and FactSalesLineItems.**

    ![A screenshot of a computer Description automatically
    generated](./media/image128.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image129.png)

### **Task-4: Reverse ETL and Build Personalization Model**

1.  Continue to the next notebook in the workspace
    named **build_personalization_model.ipynb** to perform Reverse ETL
    and build a personalization model using the transformed data.

    ![A screenshot of a computer Description automatically
    generated](./media/image130.png)

2.  In the notebook, locate the code cells where you need to provide
    your Cosmos DB and Data Warehouse details. Update the following
    variables with your specific information:

    ![A screenshot of a computer Description automatically
    generated](./media/image131.png)

    - **COSMOS_ENDPOINT**: The endpoint URL of your Cosmos DB account. You
    can find this in the Cosmos DB account settings under **Connection String**.

    ![A screenshot of a computer Description automatically
    generated](./media/image132.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image133.png)

    - **WAREHOUSE_SERVER**: The SQL endpoint of your data warehouse. You can
    find this by navigating to your data warehouse in Fabric,
    selecting **Settings** from the top menu ribbon, and copying the SQL
    endpoint from there. You can also use the same value you used in the
    previous notebook.

    ![A screenshot of a computer Description automatically
    generated](./media/image134.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image135.png)

3.  After updating the variables, run all the cells in the notebook
    sequentially to perform the Reverse ETL process and build the
    personalization model. You can do this by selecting **Run All** from
    the top menu ribbon.

    ![A screenshot of a computer Description automatically
    generated](./media/image136.png)
    
    **Note:** If you find any error in any of the cell, you can select
    ‘fix with copilot’ option appears when you hover the cell. You can use
    that to fix the cell and you can then run other cells manually one by
    one. This is required only when any of the cell run is stopped.
 
    ![A screenshot of a computer Description automatically
    generated](./media/image137.png)

4.  Once the notebook has finished running, it will have updated the
    user profiles in Cosmos DB with the latest transaction data and
    built a personalization model based on customer purchase patterns.

5.  Navigate to **Cosmos DB (fc_commerce_cosmos)** data.

    ![](./media/image138.png)

6.  Run this command to verify the updates in Cosmos DB by querying the
    customers collection to see the updated:

    +++SELECT TOP 10 * FROM c ORDER BY c.lastPurchaseDate DESC+++
 
    ![A screenshot of a computer Description automatically
    generated](./media/image139.png)

7.  Execute this query by clicking on the **Execute query** button.

    ![A screenshot of a computer Description automatically
    generated](./media/image140.png)

## Exercise 6 – Serve Personalized Recommendations from Cosmos DB

In this final exercise, you will enhance the demo application to serve
personalized menu item recommendations stored in Microsoft Fabric Cosmos
DB. This will allow customers to see AI-generated suggestions based on
their preferences and purchase history.

### **Task-1: Configure and Run the Demo Application**

1.  In the LabFiles folder, right-click on the folder and select **Show more options**.

    ![A screenshot of a computer Description automatically
    generated](./media/image141.png)

2.  Open the folder in **Visual Studio Code**s.

    ![](./media/image142.png)

3.  Open **New Terminal** in Visual Studio Code.

    ![A screenshot of a computer Description automatically
    generated](./media/image143.png)

4.  Set the Cosmos DB endpoint environment variable with your endpoint
    **Note: you can find this in your Fabric workspace under the Cosmos DB connection details:**

    ```
    $env:CosmosDb:Endpoint=<https://YOUR-FABRIC-COSMOS-ENDPOINT.ze6.sql.cosmos.fabric.microsoft.com:443/
    ```
    
    ![A screenshot of a computer Description automatically
    generated](./media/image144.png)

5.  Run the Application

    ```
    dotnet run --project src/app/FourthCoffee.Blazor/FourthCoffee.Blazor/FourthCoffee.Blazor.csproj
    ```
 
    ![A screenshot of a computer Description automatically
    generated](./media/image145.png)

6.  The app will start at +++http://localhost:5108+++ (or as displayed in
    the console).

    ![A screenshot of a computer Description automatically
    generated](./media/image146.png)

7.  Preview the application and search for any user such as +++Ava Garcia+++ and you'll see the details.

    ![A screenshot of a computer Description automatically
    generated](./media/image147.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image148.png)
    
    ![A screenshot of a computer Description automatically
    generated](./media/image149.png)

**Note: If you're unable to find the customer details. Navigate to VS Code and enter Ctrl+C to shut down the application. Then, first run the 'az login' command and then start the application again.**

## Conclusion

In this hands-on lab, you built a complete real-time analytics solution
using Microsoft Fabric and Cosmos DB—starting from environment setup and
data ingestion to cross-database analytics, real-time streaming, Reverse
ETL, and AI-driven personalization. You learned how operational data in
Cosmos DB is automatically mirrored to OneLake for unified analytics,
how to process streaming POS events using Eventhouse and KQL, how to
transform and load data into a Fabric Data Warehouse, and finally how to
enrich customer profiles and generate personalized recommendations. By
completing these exercises, you gained end-to-end experience in
designing, implementing, and consuming real-time insights in a modern
data architecture powered by Microsoft Fabric.
