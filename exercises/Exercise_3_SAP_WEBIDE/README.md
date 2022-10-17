# Steps in SAP Web IDE

## Prerequisites

- a running SAP Web IDE that is connected to the same SAP HANA database that was used for the SAP HANA Studio steps
- a Web IDE user that is allowed to develop in a XSA space

Creating the same calculation views in SAP Web IDE would look very similar to creating them in SAP HANA Studio. Therefore, we migrated the views using the [xs-migration assistant](https://help.sap.com/docs/SAP_HANA_PLATFORM/58d81eb4c9bc4899ba972c9fe7a1a115/5775fac4200441589c12a5421d0bcb1e.html) and provide you the migrated calculation views in a [zip file](/exercises/resources/TECHED_2022.zip).

## Import migrated calculation views

- Click on "Workspace" so that it becomes highlighted

- In Menu "File", choose "Import", "File or Project"

    ![import zip file](./images/importZip.png)

- Use the Browse button to search for your local file "TECHED_2022.zip" (you can download it from the "resources" section), click on "Open" and confirm with "OK"

    ![import dialog zip file](./images/importDialogZip.png)

- Right-click on folder "db" and choose "Build", and "Build" again

    ![build project](./images/buildProject.png)

- If you get an error message saying that no space is defined, press the "Configure Settings" button and select a space

- If no builder has been installed before, install the builder by pressing "Install Builder" (you won't see this button, if a Builder has already been installed)

    ![space setting](./images/spaceSetting.png)

    - Wait for the progess status for installing the builder to finish in the lower right corner:

        ![progress  install builder](./images/installBuilderProgressBar.png)

    - you should see a success message for the builder:

        ![builder successfully installed](./images/builderInstallSuccess.png)

- Press "Save", and right-click again on folder "db" to start the build process

The calculation views are deployed to your database.

## Summary
You have now imported a calculation view model migrated for XSA

Continue to - [Exercise 4 - Excercise 4 ](exercises/Exercise_4_Replicate_Calcview)