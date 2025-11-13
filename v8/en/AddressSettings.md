---

title: Getting delivery address settings
layout: default
tags: v8
---
The V8 API now allows you to receive address settings to correctly handle delivery creation.

For this purpose, the [AddressShowTypeSettings](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Organization_IRestaurant.htm) property was added to the [IRestaurant](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Organization_IRestaurant.htm) restaurant settings.

How to get new delivery address settings:

```cs
IRestaurant restaurant = PluginContext.Operations.GetHostRestaurant();
AddressShowTypeSettings addressShowTypeSettings = restaurant.AddressShowTypeSettings;
```

``addressShowTypeSettings`` will have the following fields:

- [``UseNewFormat``](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Settings_AddressShowTypeSettings_UseNewFormat.htm) - whether to use the shipping address format with the Line1 and Line2 fields.

- [``UseLiveSearch``](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Settings_AddressShowTypeSettings_UseLiveSearch.htm) - whether to use live search (DaData) to add delivery addresses to Syrve POS.

- [``AddressShowType``](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Settings_AddressShowTypeSettings_AddressShowType.htm) - what type of delivery address display is used in Syrve POS and Syrve Office?

Address display types:

LEGACY - Address format with the fields "City", "Street", "House", "Building"

CITY - Address format with the fields "Where to deliver" (Line1), "Entrance", "Floor", etc.

INTERNATIONAL - UK address style with Line1 and Line2

NOPOSTCODE - UAE address style.



This functionality can be useful so that when creating a delivery in Syrve POS, the plugin can determine whether it is allowed to use the Line1 and Line2 fields to create the delivery address or not.
