---
title: Added methods for working with external courier services
layout: default
---

From V7 in delivery [`IDeliveryOrder`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm) a new field appears [`IDeliveryOrder.ExternalCourierServiceData`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_ExternalCourierServiceData.htm), as well as methods for assigning an external courier service [`PluginContext.Operations.ChangeDeliveryExternalCourierService`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryExternalCourierService.htm) and external courier [`PluginContext.Operations.ChangeDeliveryExternalCourier`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryExternalCourier.htm).

### Usage example

```cs
private void EcsExample()
{
    var delivery = ...;
    var credentials = PluginContext.Operations.AuthenticateByPin(pin);
    
    // ID received from external system
    Guid ecsId = ...;
    // System name
    string ecsName = "Service Name";

    PluginContext.Operations.ChangeDeliveryExternalCourierService(ecsId, ecsName, delivery, credentials);
    delivery = PluginContext.Operations.GetDeliveryOrderById(delivery.Id);

    string courierName = "John Johnson";
    string courierPhone = "+380971111011"; // courier phone number, in international format
    string courierComment = "Cash payment";

    PluginContext.Operations.ChangeDeliveryExternalCourier(courierName, courierPhone, courierComment, delivery, credentials)
}
```
