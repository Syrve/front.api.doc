---
title: Barcodes
layout: default
---

API V7 added mechanisms for working with product barcodes.
Now you can find a product by barcode using the method [`IOperationService.GetProductByBarcode`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetProductByBarcode.htm).  
You can also get a list of all barcodes that are used by a product.
To do this, a list of packages [`IProduct.BarcodeContainers`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Assortment_IProduct_BarcodeContainers.htm) has been added to the interface [`IProduct`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Assortment_IProduct.htm), from which you can find out barcodes using the property [`IBarcodeContainer.Barcode`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Assortment_IBarcodeContainer_Barcode.htm).