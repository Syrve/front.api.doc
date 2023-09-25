---
title: User authentication without PIN code
layout: default
---

In the V8Preview7 API, you can now confirm actions for users without a PIN. But this will require a special license.

Each plugin instance must have its own unique `ClientId` of type `Guid`.
At startup, the plugin should grab the license slot with the module `21057201`,
calling the method [`ILicensingService.AcquireSlot`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_ILicensingService_AcquireSlot.htm)
and passing on your `ClientId`.
The result of the method execution should be remembered by the plugin and displayed when the plugin terminates.
This licensing scheme is described in more detail in the article [*"Licensing"*]({{ site.baseurl }}/v6/en/Licensing.html), chapter *"Fee for external connection to the plugin"*.

Next, to authenticate without a PIN code, you need to call the method
[`IOperationService.AuthenticateByUser`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AuthenticateByUser.htm),
passing the user into it [`IUser`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Security_IUser.htm)
and `ClientId` of the given plugin instance used to capture the license with module `21057201`.
This method will return an object [`ICredentials`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Security_ICredentials.htm),
with which you can continue working as before
(similar to method [`IOperationService.AuthenticateByPin`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AuthenticateByPin.htm)).

Thus, the client needs to issue as many slots for the module `21057201` as there are instances of plugins (plugins can be different) that use user authentication without a PIN code.
