---
title: Creating a delivery with the Line1 address field
layout: default
tags: v8
---
The V8 API now allows you to create deliveries where the address, instead of the city, street, house, and building fields, is formed using the [Line1](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Brd_IAddress_Line1.htm) field.

`Line1` is the part of the address that specifies how to find the object on the map when geocoding.

Example: `London, Baker Street, 221B`

To generate an address, enable "`Use new address format`" in the "`Delivery address format`" section of the restaurant settings 
(`Administration -> Store Settings -> General Settings`)and select the mode below: `"Delivery destination", "apartment", "floor"...`
After this, restart Syrve POS and Syrve Office to apply the address display settings.

Specifics of request formation:

When creating a delivery, the address fields ([AddressDto](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Brd_AddressDto.htm)) ``StreetId и House`` must be filled in as follows:<br>
`StreetId = f8aaf1ec-8f46-907f-9357-e4c27bab5f78` - Empty street ID in the database (the same for all databases).<br>
`House = ""` - an empty string or string.Empty, but the absence of this field is not allowed.<br>

Example of address formation:

```cs
var address = new AddressDto
{
   Line1 = "London, Baker Street, 221B",
   StreetId = Guid.Parse("f8aaf1ec-8f46-907f-9357-e4c27bab5f78"),
   House = "",
   Flat = "4",
   Floor = "2",
   AdditionalInfo = "Address details",
   Doorphone = "4",
   Entrance = "1"
};
```

