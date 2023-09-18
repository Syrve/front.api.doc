---
title: A link to the department to which the table belongs has been added to the ITable interface
layout: default
---

To the interface [`ITable`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Organization_Sections_ITable.htm) added a property [`RestaurantSection`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_Sections_ITable_RestaurantSection.htm), that returns a link to the restaurant branch. 

Previously, in order to find the branch of the restaurant to which the table belongs, it was necessary to request all branches using the method [`GetTerminalsGroupRestaurantSections`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetTerminalsGroupRestaurantSections.htm) and, by going through the tables in each department, find the desired one.
