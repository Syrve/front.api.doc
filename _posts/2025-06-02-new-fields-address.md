---
title: Three new fields added to address
layout: default
tags: v9preview5 v9
---

Starting from `V9Preview5`, additional properties have appeared in [`IAddress`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Brd_IAddress.htm) for searching addresses in an external system: [`Title`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Brd_IAddress_Title.htm), [`Subtitle`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Brd_IAddress_Subtitle.htm), and [`Distance`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Brd_IAddress_Distance.htm).

- `Title` — main part of the address
- `Subtitle` — auxiliary part of the address
- `Distance` — distance to the address from the store

These fields are used for addresses in `Line1` format and are intended to visually facilitate searching for the required address in the "Add new address" search window.

Previously, the address search result was a single string ([`Line1`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Brd_IAddress_Line1.htm)), displayed on the screen in a single font. Now, the search result can be returned in two lines — the main part of the address (`Title`) and the auxiliary part (`Subtitle`). To improve readability, different fonts are used for their display: the text from `Title` will be displayed larger, and `Subtitle` will be smaller.

To switch to addresses in `Line1` format and further utilize the benefits of the new fields, you must ensure the following:
- the new address format is selected in the BackOffice settings (Administration -> Store Settings -> Delivery Address Format -> Use New Address Format)
- the `Resto.Front.Api.Delivery` plugin is installed and functioning correctly
- the `Resto.Front.Api.ExternalAddressService` plugin is installed and functioning correctly

Next, you need to create a delivery in the front-end and click on the "Address" field in the "Client" tab, which will open the "Add new address" window. When entering an address, the value of the `Title` field (if it is not `null`) will be displayed in the center, and the value of the `Subtitle` field will be displayed below in a smaller font.

If `Title` is `null`, the `Line1` field is displayed.

The `Distance` field is not explicitly displayed, but it is used implicitly when sorting search results: currently, they are sorted by increasing distance. The starting point for the distance calculation is the store's address.

Examples of creating an address with new fields can be found in the SDK project [SamplePlugin](https://github.com/syrve/front.api.sdk/tree/master/sample/v8) in the [`EditorTester.cs`](https://github.com/syrve/front.api.sdk/blob/7909d838587ad7d0408b9bbe5881d566055b5ffc/sample/v8/Resto.Front.Api.SamplePlugin/EditorTester.cs) class: in the [`CreateDelivery`](https://github.com/syrve/front.api.sdk/blob/7909d838587ad7d0408b9bbe5881d566055b5ffc/sample/v8/Resto.Front.Api.SamplePlugin/EditorTester.cs#L794) and [`ChangeDeliveryOrderTypeOnCourier`](https://github.com/syrve/front.api.sdk/blob/7909d838587ad7d0408b9bbe5881d566055b5ffc/sample/v8/Resto.Front.Api.SamplePlugin/EditorTester.cs#L983) methods.