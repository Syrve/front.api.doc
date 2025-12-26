---
title: Extended data on units of measurement in Product and ChequeSale
layout: default
tags: v9preview2 v9

---

In the V9Preview2 API, the [`IProduct`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Assortment_IProduct.htm) 
, the `MeasuringUnitName` property has been replaced with the [`MeasuringUnit`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Assortment_IProduct_MeasuringUnit.htm) 
of type [`IMeasuringUnit`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Assortment_IMeasuringUnit.htm), 
which stores more information.

The [`ChequeSale`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Device_Tasks_ChequeSale.htm)
has been added the property [`MeasuringUnit`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Device_Tasks_ChequeSale_MeasuringUnit.htm) 
of type [`MeasuringUnitInfo`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Device_Tasks_MeasuringUnitInfo.htm). 
This property can be modified by plugins and therefore does not have an `Id` field to avoid confusion and not associate it with the actual `MeasureUnit`.