## Deprecation Notice

This public repository is read-only and no longer maintained.

![](https://img.shields.io/badge/STATUS-NOT%20CURRENTLY%20MAINTAINED-red.svg?longCache=true&style=flat)

---
[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2022-DA181)](https://api.reuse.software/info/github.com/SAP-samples/teched2022-DA181)

# DA181 - SAP HANA Cloud: Dynamic Hybrid Extensions for Analytical Workloads

## Description

This repository contains the material for the SAP TechEd 2022 session called DA181 - SAP HANA Cloud: Dynamic Hybrid Extensions for Analytical Workloads

## Overview

This session introduces attendees to extend their on-premise analytical workloads \built as repository or HDI containers based calculation views. The attendees will be guided on how to model a simple Calculation View using SAP HANA Studio or SAP WebIDE, select it from SAP HANA Cockpit in BTP, and verify the calculation view runtime a.k.a column views in SAP HANA Cloud. The replicated calculation view can be extended and utilize the latest features offered by SAP HANA Cloud.

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

The following exercises provide 2 tracks that can be done together or separated based on the individual needs of the participants.
   - Calculation view based on database schema and repository (XSC) using SAP HANA Studio
   - Calculation view based on HDI containers and Git (XSA) using SAP Web IDE

A single SAP HANA Platform will be provided and shared between participants. Detailed server connection info will be provided during the TechEd Live event.
To follow this exercise, outside the live event, the SAP HANA Platform will not be available and requires a SAP HANA Platform to be available acting as the source system. Also, in a normal corporate environment, the SAP HANA Platform will exist inside corporate firewalls and a cloud connector will be needed to create a network connection from SAP HANA Cloud in BTP to SAP HANA Platform on-premise. For simplicity of this workshop, we will bypass the configuration of the cloud connector during the live event.

## Exercises

- [Exercise 1 - Data Preparation (optional for post TechEd event participants)](exercises/Exercise_1_Setup/)
- [Exercise 2 - Calculation view modeling with SAP HANA Studio](exercises/Exercise_2_SAP_HANA_STUDIO/)
- [Exercise 3 - Calculation view modeling with SAP Web IDE](exercises/Exercise_3_SAP_WEBIDE/)
- [Exercise 4 - Setup Replication from SAP HANA Platform to SAP HANA Cloud](exercises/Exercise_4_Replicate_Calcview/)
- [Exercise 5 - Calculation view modeling with Business Application Studio](exercises/Exercise_5_SAP_BAS/)
- [Exercise 6 - Modify Calculation view and trigger update to SAP HANA Cloud](exercises/Exercise_6_Modify/)

## How to install and setup the SAP HANA Platform after TechEd live workshop
- Installation of [SAP HANA Clients](https://developers.sap.com/tutorials/hana-clients-install.html)
- Installation of [SAP HANA Studio](https://help.sap.com/docs/SAP_HANA_PLATFORM/a2a49126a5c546a9864aae22c05c3d0e/c13cec2b7c2c46bdb0762ce5f1410260.html)
- Installation of [SAP HANA Platform, express edition](https://developers.sap.com/group.hxe-install-vm-xsa.html) or using the CAL image of SAP HANA 2.0 SPS06 Rev63 available at http://cal.sap.com

## How to install and configure cloud connector after the TechEd live workshop
- Installation of [SAP Connectivity Service, cloud connector](https://developers.sap.com/tutorials/hana-cloud-mission-extend-08.html)

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## Aditional Support and Learning Resources
 - Visit us in the [SAP Community](https://community.sap.com/topics/hana)

## License
Copyright (c) 2022 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
