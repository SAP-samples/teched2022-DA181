# Steps in SAP HANA Studio

## Prerequisites

- a running SAP HANA Studio with a connection to a running SAP HANA Database
  
- database user SYSTEM is used in the connection

</br>
</br>

> <mark> If you have no running SAP HANA Studio then you can skip this section. To import the tables that are needed for the other sections without using SAP HANA Studio, please follow the steps outlined in "Import_tables_DatabaseExplorer". </mark>


In the following steps two calculation views are created from scratch that will later be replicated to SAP HANA Cloud. These models use data of up to three tables. Therefore, these tables are imported first. Afterwards authorizations are granted to use the tables in calculation view modeling

> In the following database user SYSTEM is used. Typically dedicated users with the respective authorizations would be used in real-world scenarios. By using database user SYSTEM we skip these authorization steps to focus on the replication part which is the focus of this session

</br>


## Import tables required by the calculation views and assign required authorizations

### Extract zip file that contains tables

On your computer, extract the TechEd2022_tables.zip file. You will need the location of the extracted folder that contains folder "index" later.


### Import tables

- In the File menu, choose "Import":

  ![import tables](./screenshots/import.png)

- Select "Catalog Objects" and press "Next":

  ![select catalog objects](./screenshots/selectCatalogObjects.png)

- Choose option "Import catalog objects from current client" and use the "Browse" button to navigate to the extracted folder that contains the folder "index". Select this folder.

  > Do not select the folder "index" itself but the folder containing it.

- select all three tables by clicking at them with the left mouse button

- choose "Add" to select them for import 

- press "Next"

- select the option to include data but keep the other options unselected:

  ![include data in import](./screenshots/includeData.png)

- press "Finish"

The tables have now been imported with data.

### Assign authorizations to use the tables for calculation view modeling

- open a SQL terminal by right-clicking on your connection with database user "SYSTEM" and choosing "Open SQL Console"

  ![open SQL console](./screenshots/openSQLConsole.png)

- In the opened SQL console, type:

  ```SQL
  GRANT SELECT ON SCHEMA BASKETANALYSISDATA TO _SYS_REPO WITH GRANT OPTION;
  GRANT SELECT ON SCHEMA BASKETANALYSISDATA TO "_SYS_DI#SYS_XS_HANA_BROKER"."_SYS_DI_OO_DEFAULTS" WITH GRANT OPTION;
  GRANT SELECT ON SCHEMA BASKETANALYSISDATA TO "SYS_XS_HANA_BROKER"."RT_DEFAULTS";
  ```

- mark all statements with your mouse and execute the highlighted statements by pressing the "Execute" button on the top-right:

  ![execute SQL statement](./screenshots/execute.png)

  The first line allows using the tables in schema BASKETANALYSISDATA for calculation view modeling in SAP HANA Studio. The other two lines allow using the tables in every Web IDE project.

  > Typically, not all projects should have the authorization to use certain tables and a more fine-granular authorization setup is used. How this can be achieved with HDI development can be found e.g., in this [blog](https://blogs.sap.com/2018/12/11/how-to-use-objects-contained-in-a-schema-outside-of-your-web-ide-full-stack-project-in-sap-hana-service/).

The required tables have been imported and the authorizations assigned. In the next steps we will create calculation view models.

## Create calculation view models

We will create two calculation view models. The first combines the two tables in a Union node. The second one joins the tables to enable a basket analysis of the data.

### Create package BASKETANALYSIS

A package helps structuring your development objects. 

- To create a package, right-click on "Content", select "New" and "Package"

  ![new package](./screenshots/newPackage.png)


  - Name it "BasketAnalysis" and choose "OK":

  ![name package](./screenshots/namePackage.png)

### Create calculation view COMBINESOURCESFORBASKETANALYSIS

This calculation view will combine different data source using a Union node.

- Right-click on the new package and choose "New" and "Calculation View"
- Name the calculation view "COMBINESOURCESFORBASKETANALYSIS" and press "Finish":

  ![name COMBINESOURCESFORBASKETANALYSIS](./screenshots/COMBINESOURCESFORBASKETANALYSIS.png)

  A graphical editor opens.

- Select the Union node icon and place it by a left-click on the modeling area:

  ![place Union](./screenshots/Union.png)

- Use the Add Objects button:

  ![add objects](./screenshots/addObjects.png)

- Search for tables with "TLOG" in their name, select "TLOGF_C" and "TLOGF_O" and press "OK":

  ![select tables](./screenshots/selectTables.png)

- In the Details of the Union node that opens when double-clicking on the node, press icon "Auto Map By Name" to combine columns with same names in one output column

  ![auto map by name](./screenshots/autoMapByName.png)


- Feed the output of the Union node into the Aggregation node by selecting the "Create Connection" icon and dragging the connection to the Aggregation node:

  ![connect nodes](./screenshots/connectNodes.png)

- To feed the columns of the "Aggregation" node to the node "Semantics":
    - double-click on node Aggregation to open the Details
    - click on the first column
    - hold the shift-key and click on the last column
    - finally right-click on any column and select "Add To Output". 
    
    All columns should now show an orange dot. If not, select the individual column and click on the grey dot to switch it to orange

    ![select output columns](./screenshots/selectOutputColumns.png)


#### Switch off Analytic Privilege check

- In node "Semantics" under "View Properties" select the empty option for "Apply Privileges":

  ![no analytic privilege check](./screenshots/noAP_1.png)

  > If the analytic privilege check was not switched off an analytic privilege would be required to report on the calculation view. For details, see e.g., this [blog](https://blogs.sap.com/2022/09/23/sap-hana-cloud-analytic-privileges-a-step-by-step-guide/)

- Press "Save and Activate" to create the database object of the calculation view

  ![save and activate](./screenshots/saveAndActivate.png)


The calculation view database object has now been generated and can be used.

</br>

### Design calculation View BASKETANALYIS_CALCULATE

This calculation view will prepare the data so they can be used for basked analyses.


#### Create Calculation View BASKETANALYSIS_CALCULATE

- Right-click on package "BasketAnalysis" and choose "New" and then "Calculation View"

- Name the view "BASKETANALYSIS_CALCULATE" and press "Finish"

  ![name BASKETANALYSIS_CALCULATE](./screenshots/BASKETANALYSIS_CALCULATE.png)

#### Place nodes to modeling area

- Add 2 Projection Nodes, 2 Aggregation Nodes and 1 Join node to the modeling area. To do so click on the respective icon and then at an unoccupied space in the modeling area. Try to arrange the nodes roughly like in the screenshot below:

![node layout](./screenshots/nodeLayout.png)

#### Rename nodes

- Rename the nodes as in the screenshot with the connected nodes below. To rename a node right-click on the name and choose "Rename": 

  ![rename nodes](./screenshots/renameNodes.png)

  In particular use the following names:
    - cleanse
    - extractRelevantReceipts
    - getDistinctRelevantProductIDs
    - removeSelectedProduct


#### Connect nodes

- connect the nodes with 6 lines as shown below. To start a connection click on "Create Connection" and drop the line on the target node like you did for  calculation view "COMBINESOURCESFORBASKETANALYSIS"

![connect nodes](./screenshots/connectAllNodes.png)


#### Add data source

- Add the calculation view COMBINESOURCESFORBASKETANALYSIS that was created in an earlier step as a data source to node "cleanse". To do so, choose "Add Objects" of node "cleanse", search for "COMBINESOURCESFORBASKETANALYSIS" and click "OK". 

![add objects](./screenshots/addObjects_2.png)


#### Map columns to the output of the individual nodes

- In Details of node "cleanse", only feed columns "OrderID" and "ProductName" into the next node by clicking on their dot. To open the Details, double-click on node "cleanse":
 
  ![select columns in cleanse](./screenshots/selectColumns.png)

- In Details of node "extractRelevantReceipts", select both columns in the same way to feed them into the next node:

  ![select columns in extractRelevantReceipts](./screenshots/columnsExtractRelevantReceipts.png)

- For node "getDistinctRelevantProductIDs" only select "OrderID":

  ![select columns in getDistinctRelevantProductIDs](./screenshots/columnsGetDistinctRelevantProductIDs.png)

- select both columns in node "removeSelectedProduct":

  ![select columns in removeSelectedProduct](./screenshots/columnsRemoveSelectedProduct.png)

- In the Details of node "Join_1": drag a connection between both columns "OrderID". Select the line and choose JoinType "Inner" and Cardinality "n..m". Finally select "ProductName" comming from node "removeSelectedProduct" and "OrderID" comming from node "getDistinctRelevantProductIDs":

  ![join node setup](./screenshots/joinNode.png)

- In node "Aggregation", choose both columns:

  ![aggregation node](./screenshots/aggregationNode.png)

#### Add an input parameter

- Double-click on node "Semantics", then on tab "Parameters/Variables" and click on the green plus-sign to choose "Create Input Parameter":

  ![add input parameter](./screenshots/addInputParameter.png)

- Name the input parameter "selectedProduct" and choose "NVARCHAR" as Data Type, and length "300", confirm with "OK":

  ![input parameter details](./screenshots/inputParameterDetails.png)

#### Add count distinct column

- In node "Semantics", right-click on "Calculated Columns" and choose "New Counter":

  ![new counter](./screenshots/newCounter.png)

- name the counter "countFrequency" and add the column "OrderID". Finally confirm with "OK":

  ![counter definition](./screenshots/counterDefinition.png)

#### Switch off Analytic Privilege check

- In node "Semantics" under "View Properties" select the empty option for "Apply Privileges":

  ![no analytic privilege check](./screenshots/noAP.png)



#### Add filters to the nodes

- Double-click on node "extractRelevantReceipts", then on "Expression" and enter:

  `"ProductName"='$$selectedProduct$$'`

  ![filter extractRelevantReceipts](./screenshots/filterExtractRelevantReceipts.png)

- Confirm with "OK" 

This filter is used to remove all records that contain a different value in column "ProductName" than the value that is entered for the input parameter "selectedProduct" during query execution. 

- Double-click on node "removeSelectedProduct", then on "Expression" and enter:
  
  `"ProductName"!='$$selectedProduct$$'`

  ![filter removeSelectedProduct](./screenshots/filterRemoveSelectedProduct.png)

This filter will remove records with values in column "ProductName" that correspond to the value that is entered for the input parameter.

#### Save and activate calculation view

- press the "Save and Activate" icon:
  
  ![save and activate](./screenshots/saveAndActivate.png)


The calculation view database object has now been generated and can be used.