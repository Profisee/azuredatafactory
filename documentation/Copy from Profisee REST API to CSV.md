Copy from Profisee REST API to CSV Format
=========================================

This article describes a solution template that you can use to copy
records from Profisee REST API to Azure Data Lake Storage Gen2 storage,
in CSV format.

About this solution template
----------------------------

This template retrieves records from Profisee REST API. It then copies
the records, in CSV format, to a file in an output container. The
template is designed to work with a folder structure consisting of
folders named for each entity within the output container. Create a
folder for each entity you wish to integrate with. CSV files for an
entity will get created to the profisee-output\\&lt;entity&gt; folder.

When the pipeline created by the template is run, it will create a
folder for the entity, if it doesn’t exist, and copy the file to that
folder. The file name is composed of the entity name and date/time in
UTC with the .csv extension.

For example:

-   profisee-output
    -   account
    -   customer
    -   product

<img src="./media/copyfrom_restapi_to_csv_1.png" style="width:3.80106in;height:2.44509in" />

How to use this solution template
---------------------------------

1.  Go to the **Copy from Profisee REST API to CSV** template.

    <img src="./media/copyfrom_restapi_to_csv_2.png" style="width:2.45399in;height:1.69459in" />

1.  Create a **New** or use an existing connection to the source
    Profisee REST API.

    <img src="./media/copyfrom_restapi_to_csv_3.png" style="width:4.376in;height:2.78549in" />

    Follow these steps if you need to create a new REST linked service.
    
    1.  Select “+ New" from the **REST** dropdown list.
    
        <img src="./media/copyfrom_restapi_to_csv_4.png" style="width:2.06135in;height:1.29089in" />
    
    2.  See [REST Linked Service](REST%20Linked%20Service.md) for information on setting up the REST linked service.

3.  Create a **New** or use an existing connection to the ADLS Gen2 sink
    data store that you are copying data to.

4.  Select **Use this template**.

5.  You will see a pipeline created as shown in the following example:

    <img src="./media/copyfrom_restapi_to_csv_6.png" style="width:3.69939in;height:2.41883in" />


Pipeline
--------

### Variables

1.  **OutputContainer:** The output container where you are copying the
    file to. It defaults to “profisee-output”. You can update to
    another name based on your environment.

2.  **EntityId:** The entity you are copying records for. Note, the
    entityId can be either the entity’s Name, UID, or InternalId
    value.

    <img src="./media/copyfrom_restapi_to_csv_8.png" style="width:4.04in;height:1.39674in" />

Copy Activity
-------------

### Source

1.  Dataset properties:

    1.  **entityId** - Uses the EntityId variable value.
    
    2.  **pageSize** - The page size to get.  Defaults to 1000 if not supplied.
    
    3.  **filter** - A filter to restrict the records returned.
        1.  \[&lt;attribute name&gt;\] &lt;operator&gt; &lt;value&gt;.
            -   Example: \[Color\] eq ‘BLU’.
        2.  The filter can include multi-level attributes (MLAs).
            -   Example: \[ProductSubCategory\]/\[ProductCategory\] eq '1'.
        3.  You can group attributes together using parenthesis and ANDs and ORs.
    
    4.  **attributes** - A comma separated list of entity attribute names to
        return.  The list can include multi-level attributes (MLAs). If
        blank, all attributes are returned. Note: the attribute list
        determines the result properties you will see in the **Mapping**
        tab.
        1.  MLAs are supported, using the ‘/’ to separate each part of the MLA path
        2.  Example: \[Color\],\[Class\],\[ProductSubCategory\],\[SellStartDate\],\[SellEndDate\],\[Weight\],\[ProductSubCategory\]/\[ProductCategory\]/\[ProductGroup\]
    
    5.  **orderBy** - A comma separated list of entity attribute names and direction to order the response
        1.  \[&lt;attribute name&gt;\] or \[&lt;attribute name&gt;\] asc - sorts attribute in ascending order
        2.  \[&lt;attribute name&gt;\] desc - sorts attribute in descending order
        3.  Example: \[ProductSubCategory\], \[SellStartDate\] desc
    
    6.  **dbaFormat** - The domain-based attribute (DBA) format to return.
        Provides an option to indicate how to return the DBA's Code and
        Name.  Note: a DBA is an attribute that points to, or references,
        another entity, called a domain entity. 
        1.  Code only (default) - Only return the code value.
            -   Example: 
                -   "Source System": "SF",
        2.  Code and Name simple properties.  The name property is returned as DBA.Name.
            -   Example: 
                -   "Source System": "SF",
                -   "Source System.Name": "Salesforce",
    
    7.  **recordCodes** – A comma separated list of record codes to restrict the records returned. 
    
    8.  You can find more information on these parameters on the Profisee REST API Swagger page. You can find it at https://&lt;host 
        name&gt;/Profisee/rest.
    
        <img src="./media/copyfrom_restapi_to_json_9.png" style="width:4.624in;height:2.12279in" />


### Sink

1.  Dataset properties

    1.  FolderName – A concatenation of the OutputContainer and the EntityId.
    2.  FileName – A concatenation of the EntityId and a timestamp.

    <img src="./media/copyfrom_restapi_to_csv_11.png" style="width:4.98747in;height:1.76in" />

### Mapping

Select **Mapping** tab to map the records result properties to the corresponding CSV column.

First click the **Import Schemas** button. You will be prompted to confirm the value of the pipeline variable for the EntityId. Click **OK**.

<img src="./media/copyfrom_restapi_to_csv_12.png" style="width:3.19951in;height:2.28221in" />

After a couple of seconds, you will see a list of mapping fields
listed, as shown in the following example.

<img src="./media/copyfrom_restapi_to_csv_13.png" style="width:6.55347in;height:4.51181in" />

Next, select **data** from the **Collection reference** drop down
list. The **data** property is the array of records.

<img src="./media/copyfrom_restapi_to_csv_14.png" style="width:3.70779in;height:0.45753in" />

Unselect the Include checkboxes for the pageNbr, pageSize,
resultCount, totalPages, totalRecords, and nextPage properties as we
do not want to copy them to the file.

<img src="./media/copyfrom_restapi_to_csv_15.png" style="width:5.29221in;height:1.43161in" />

After selecting the data collection reference, you need to select the
Type for each field you want to copy.

<img src="./media/copyfrom_restapi_to_csv_16.png" style="width:6.5in;height:1.51042in" />

Publish
-------

Once you are finished with all your changes, click **Publish All**.

<img src="./media/copyfrom_restapi_to_csv_17.png" style="width:1.36994in;height:0.29043in" />

Triggering
----------

1.  To run the pipeline now, select **Add Trigger** and select **Trigger
    now**. Press **OK** at the Pipeline run prompt.

2.  Select **Monitor** tab in the left navigation panel and wait for
    about 20 seconds. Click **Refresh** to get the updated run status.

3.  When the pipeline run completes successfully, you would see results
    like the following example:

    <img src="./media/copyfrom_restapi_to_csv_18.png" style="width:5.81595in;height:1.1023in" />

4.  You should also see the output file in the Container and Directory
    you entered.

    <img src="./media/copyfrom_restapi_to_csv_19.png" style="width:2.58282in;height:1.42087in" />

Next steps
----------

-   [Introduction to Azure Data
    Factory](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/data-factory/introduction.md)
