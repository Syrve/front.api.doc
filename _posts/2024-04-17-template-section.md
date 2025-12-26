---
title: Options for extending check templates from plugins
layout: default
tags: v9 v9preview2

---

The V9Preview2 API now includes the ability to modify the printed `XDocument` via [`BeforeFormatDocumentHandler`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_RegisterBeforeFormatDocumentHandler.htm) and [`AfterFormatDocumentHandler`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_RegisterAfterFormatDocumentHandler.htm).

The markup itself can contain the `<section>` tag, which is used to logically divide the document into blocks. The tag has a mandatory `name` attribute and an optional `data` attribute

```
<section name="section_name" data="section_data" />
```
The `name` attribute defines a specific section. It can also be used to define the type of document (guest bill, payment receipt, service check, etc.).

The optional `data` attribute can contain additional information required by the plugin. For example, you can pass `orderId`.

The `BeforeFormatDocument` subscription is designed to add additional information to the check that is not in the data model but may be in the plugin's data model.
Or to execute business logic that depends on the plugin's data model and is not available in razor templates and DocumentService.

##### Example 1.
[Resto.Front.Api.SamplePlugin.PrintTester.cs](https://github.com/syrve/front.api.sdk/blob/master/sample/v9preview2/Resto.Front.Api.SamplePlugin/PrintTester.cs)


##### Example 2.

Add a vehicle number to the delivery note. The vehicle number is missing from the ITemplateRootModel data model and DocumentService. Let's assume that this information is available in a certain plugin. To implement the task using this plugin, we add the `<section>` tag to the delivery note template. 
```
<doc>
  ...
  ...
  <section name="plate_number" data="@orderId">
  ...
  ...
</doc>
```
In the plugin code: here is an example of code that adds the vehicle number to a section named “plate_number”.
```
public XDocument BeforeFormatDocument(XDocument doc)
{
    XElement section = doc
        .Elements(“section”)
        .FirstOrDefault(e => e.Attribute(“name”) != null && (string)e.Attribute(‘name’) == “plate_number”);
  
    if (section != null)
    {
        XElement newElement = new XElement(“pair”,
            new XAttribute(“left”, “Vehicle number:”),
            new XAttribute(“right”, “A001MR77”)); //substitute real data
        section.AddAfterSelf(newElement);
    }
    return markup;
}
```

##### Example 3.

Customizable in an external loyalty system, sending payment receipts and sales receipts by email instead of printing them on a printer.

Add `<section>` tags to the payment receipt and sales receipt templates:
```
@inherits TemplateBase<IReceiptCheque>
@{
    var order = Model.Order;
}

<doc>
  <section name="ReceiptCheque" data="@order.Id">
  ...
</doc>
```
In the cash memo template:

```
@inherits TemplateBase<ICashMemoCheque>
@{
    var order = Model.Order;
}
  
<doc>
  <section name="CashMemo" data="@order.Id">
  ...
</doc>
```
In the plugin code:
```
public XDocument BeforeFormatDocument(XDocument doc)
{
    XElement receiptChequeSection = doc
        .Elements(“section”)
        .FirstOrDefault(e => e.Attribute(“name”) != null && Attribute(‘name’) == “ReceiptCheque”);
  
    if (receiptChequeSection != null)
    {
        //current document - payment receipt
        orderId = receiptChequeSection.Attributes[“orderId”]?.Value
        if (orderId != null)
        {
            //Use the plugin data model (customer settings)
            var customerInfo = GetCustomerInfo(orderId);
            if (customerSettings != null && customerSettings.SendElectronicReceiptOnly)
            {
                SendElectronicReceipt(customerInfo.UserId, doc); //Some plugin method for sending an electronic receipt
                return new XElement(“nodoc”); //Do not print the document on the printer
            }
        }
    }
  
    //Similar code for a sales receipt
  
    return doc;
}
```
In this example, the plugin received the buyer's settings (not available in the razor model ITemplateRootModel), and if the `SendElectronicReceiptOnly` option is set, the entire document is replaced with `<nodoc />`

When the document is finally sent to the printer, the `<section>` tags are ignored and do not affect the appearance of the document.
