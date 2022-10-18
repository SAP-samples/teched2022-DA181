# Exercise 2 - Calculation view modeling in SAP HANA Studio


> If you don't have SAP HANA Studio already installed and configured then you can skip this section. Please skip to the next [exercise](/exercises/Exercise_3_SAP_WEBIDE/) 


In the following steps two calculation views are created from scratch that will later be replicated to SAP HANA Cloud.

</br>

## Create calculation view models

We will create two calculation view models. The first combines the two tables in a Union node. The second one joins the tables to enable a basket analysis of the data.

### Packages
Packages help structuring your development objects. We have already created a package for you with name "TECHEDUSERXX". You can find it under in your system connection under "Content":

![package to work in](./images/ownPackage.png)

<details><summary><mark>If you are following this tutorial after the live event, please create this package first</mark></summary>
<p>

#### Create package TECHEDUSERXX

- To create a package, right-click on "Content", select "New" and "Package"

  ![new package](./images/newPackage.png)


  - Name it "TECHEDUSERXX" and choose "OK":

  ![name package](./images/namePackage.png)

</details>
<p>

### Create calculation view COMBINESOURCESFORBASKETANALYSIS

This calculation view will combine different data source using a Union node.

- Right-click on your package "TECHEDUSERXX" and choose "New" and "Calculation View"
- Name the calculation view "COMBINESOURCESFORBASKETANALYSIS" and press "Finish":

  ![name COMBINESOURCESFORBASKETANALYSIS](./images/COMBINESOURCESFORBASKETANALYSIS.png)

  A graphical editor opens.

- Select the Union node icon and place it by a left-click on the modeling area:

  ![place Union](./images/Union.png)

- Use the Add Objects button:

  ![add objects](./images/addObjects.png)

- Search for tables with "TLOG" in their name, select "TLOGF_C" and "TLOGF_O" and press "OK":

  ![select tables](./images/selectTables.png)

- In the Details of the Union node that opens when double-clicking on the node, press icon "Auto Map By Name" to combine columns with same names in one output column

  ![auto map by name](./images/autoMapByName.png)


- Feed the output of the Union node into the Aggregation node by selecting the "Create Connection" icon and dragging the connection to the Aggregation node:

  ![connect nodes](./images/connectNodes.png)

- To feed the columns of the "Aggregation" node to the node "Semantics":
    - double-click on node Aggregation to open the Details
    - click on the first column
    - hold the shift-key and click on the last column
    - finally right-click on any column and select "Add To Output". 
    
    All columns should now show an orange dot. If not, select the individual column and click on the grey dot to switch it to orange

    ![select output columns](./images/selectOutputColumns.png)


#### Switch off Analytic Privilege check

- In node "Semantics" under "View Properties" select the empty option for "Apply Privileges":

  ![no analytic privilege check](./images/noAP_1.png)

  > If the analytic privilege check was not switched off an analytic privilege would be required to report on the calculation view. For details, see e.g., this [blog](https://blogs.sap.com/2022/09/23/sap-hana-cloud-analytic-privileges-a-step-by-step-guide/)

- Press "Save and Activate" to create the database object of the calculation view

  ![save and activate](./images/saveAndActivate.png)


The calculation view database object has now been generated and can be used.

</br>

### Design calculation View BASKETANALYIS_CALCULATE

This calculation view will prepare the data so they can be used for basked analyses.


#### Create Calculation View BASKETANALYSIS_CALCULATE

- Right-click on package "TECHEDUSERXX" and choose "New" and then "Calculation View"

- Name the view "BASKETANALYSIS_CALCULATE" and press "Finish"

  ![name BASKETANALYSIS_CALCULATE](./images/BASKETANALYSIS_CALCULATE.png)

#### Place nodes to modeling area

- Add 2 Projection Nodes, 2 Aggregation Nodes and 1 Join node to the modeling area. To do so click on the respective icon and then at an unoccupied space in the modeling area. Try to arrange the nodes roughly like in the screenshot below:

![node layout](./images/nodeLayout.png)

#### Rename nodes

- Rename the nodes as in the screenshot with the connected nodes below. To rename a node right-click on the name and choose "Rename": 

  ![rename nodes](./images/renameNodes.png)

  In particular use the following names:
    - cleanse
    - extractRelevantReceipts
    - getDistinctRelevantProductIDs
    - removeSelectedProduct


#### Connect nodes

- connect the nodes with 6 lines as shown below. To start a connection click on "Create Connection" and drop the line on the target node like you did for  calculation view "COMBINESOURCESFORBASKETANALYSIS"

![connect nodes](./images/connectAllNodes.png)


#### Add data source

- Add the calculation view COMBINESOURCESFORBASKETANALYSIS that was created in an earlier step as a data source to node "cleanse". To do so, choose "Add Objects" of node "cleanse", search for "COMBINESOURCESFORBASKETANALYSIS" and click "OK". 

![add objects](./images/addObjects_2.png)


#### Map columns to the output of the individual nodes

- In Details of node "cleanse", only feed columns "OrderID" and "ProductName" into the next node by clicking on their dot. To open the Details, double-click on node "cleanse":
 
  ![select columns in cleanse](./images/selectColumns.png)

- In Details of node "extractRelevantReceipts", select both columns in the same way to feed them into the next node:

  ![select columns in extractRelevantReceipts](./images/columnsExtractRelevantReceipts.png)

- For node "getDistinctRelevantProductIDs" only select "OrderID":

  ![select columns in getDistinctRelevantProductIDs](./images/columnsGetDistinctRelevantProductIDs.png)

- select both columns in node "removeSelectedProduct":

  ![select columns in removeSelectedProduct](./images/columnsRemoveSelectedProduct.png)

- In the Details of node "Join_1": drag a connection between both columns "OrderID". Select the line and choose JoinType "Inner" and Cardinality "n..m". Finally select "ProductName" comming from node "removeSelectedProduct" and "OrderID" comming from node "getDistinctRelevantProductIDs":

  ![join node setup](./images/joinNode.png)

- In node "Aggregation", choose both columns:

  ![aggregation node](./images/aggregationNode.png)

#### Add an input parameter

- Double-click on node "Semantics", then on tab "Parameters/Variables" and click on the green plus-sign to choose "Create Input Parameter":

  ![add input parameter](./images/addInputParameter.png)

- Name the input parameter "selectedProduct" and choose "NVARCHAR" as Data Type, and length "300", confirm with "OK":

  ![input parameter details](./images/inputParameterDetails.png)

#### Add count distinct column

- Double-click on node "Aggregation", then on Output pane select "Calculated Columns" folder and right mouse clike to select "New Counter":

  ![new counter](./images/newCounter.png)

- name the counter "countFrequency" and add the column "OrderID". Finally confirm with "OK":

  ![counter definition](./images/counterDefinition.png)

#### Switch off Analytic Privilege check

- In node "Semantics" under "View Properties" select the empty option for "Apply Privileges":

  ![no analytic privilege check](./images/noAP.png)



#### Add filters to the nodes

- Double-click on node "extractRelevantReceipts", then on "Expression" and enter:

  `"ProductName"='$$selectedProduct$$'`

  ![filter extractRelevantReceipts](./images/filterExtractRelevantReceipts.png)

- Confirm with "OK" 

This filter is used to remove all records that contain a different value in column "ProductName" than the value that is entered for the input parameter "selectedProduct" during query execution. 

- Double-click on node "removeSelectedProduct", then on "Expression" and enter:
  
  `"ProductName"!='$$selectedProduct$$'`

  ![filter removeSelectedProduct](./images/filterRemoveSelectedProduct.png)

This filter will remove records with values in column "ProductName" that correspond to the value that is entered for the input parameter.

#### Save and activate calculation view

- press the "Save and Activate" icon:
  
  ![save and activate](./images/saveAndActivate.png)


The calculation view database object has now been generated and can be used.

## Summary
You have now created a calculation view model using the base tables imported using SAP HANA Studio.

Continue to - [Exercise 3 - Calculation view modeling in SAP Web IDE](/exercises/Exercise_3_SAP_WEBIDE)
