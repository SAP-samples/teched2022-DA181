# DA181 - SAP HANA Cloud: Dynamic Hybrid Extensions for Analytical Workloads

## Description

This repository contains the material for the SAP TechEd 2022 session called DA181 - SAP HANA Cloud: Dynamic Hybrid Extensions for Analytical Workloads 

## Overview

This session introduces attendees to extend their on-premise analytical workloads based on Calculation Views built within repository and schema or HDI containers. The attendees will be guided to model a simple Calculation View, select it from SAP HANA Cockpit in BTP and verify that the calculation view runtime a.k.a column views in SAP HANA Cloud. Attendees can utilize this column view with in Business Application Studio and extend utilizing the latest features offered by SAP HANA Cloud.

## Requirements

The requirements to follow the exercises in this repository are
   - SAP BTP trial / free tier user account
   - SAP HANA Cloud trial / free tier instance

   Details of getting access can be found from the following tutorial: [Jump Start Your SAP HANA Cloud, SAP HANA Database(free tier model or trial)](https://developers.sap.com/mission.hana-cloud-database-get-started.html)

Some basic understanding of the SAP BTP and SAP HANA Cloud Tooling
   - SAP HANA Cloud Central
   - SAP Business Application Studio 
   - SAP HANA Cockpit
   - SAP HANA Database Explorer
 
Some basic understanding of SAP HANA Platform Tooling
   - SAP HANA Studio (XSC)
   - SAP Web IDE (XSA)

The following execises provide 2 tracks that can be done together or seperated based on the individual needs of the participants. 
   - Calculation view based on database schema and repository (XSC) using SAP HANA Studio
   - Calculation view based on HDI containers and Git (XSA) using SAP Web IDE 

A single SAP HANA Platform will be provided and shared between participants. Detailed server connection info will be provided during the TechEd Live event.
To follow this exercise, outside the live event, the SAP HANA Platform will not be available and requires a SAP HANA Platform to be available acting as the source system. Also, in a normal corporate environment, the SAP HANA Platform will exist inside corporate firewalls and a cloud connector will be needed to create a network connection from SAP HANA Cloud in BTP to SAP HANA Platfrom on-premise. For simplicity of this workshop, we will bypass the configuration of the cloud connector during the live event.
   
## Exercises

- [Calculation view modeling with SAP HANA Studio](exercises/Steps_SAP_HANA_STUDIO/)
- [Calculation view modeling with SAP Web IDE](exercises/Steps_SAP_WEBIDE/)
- [Setup Replication from SAP HANA Platform to SAP HANA Cloud](exercises/ex2/)
- [Calcview Modeling with Business Application Studio](exercises/Steps_SAP_BAS/)
- [Modify Calculation view and trigger update to SAP HANA Cloud](exercises/Steps_SAP_BAS/)

## How to install and setup the SAP HANA Platform after TechEd live workshop
- Installation of [SAP HANA Clients](https://developers.sap.com/tutorials/hana-clients-install.html)
- Installation of [SAP HANA Platform, express edition](https://developers.sap.com/group.hxe-install-vm-xsa.html)

## How to install and configure cloud connector after the TechEd live workshop
- Installation of [SAP Connectivity Service, cloud connector](https://developers.sap.com/tutorials/hana-cloud-mission-extend-08.html)

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## Aditional Support and Learning Resources
 - Visit us in the [SAP Community](https://community.sap.com/topics/hana)
 
## License
Copyright (c) 2022 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
