---
title: It is now possible to open and close personal shifts without employee PIN codes
layout: default
---

Functions for opening and closing personal shifts was added earlier, but they only allowed managing the personal shift of the user on whose behalf the plugin was operating after logging in with a PIN code. Now you can manage personal shifts and other users too.

For this purpose, an optional argument `user` has been added to the methods [`OpenPersonalSession`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_OpenPersonalSession.htm) and [`ClosePersonalSession`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_ClosePersonalSession.htm). If this argument is not specified, by default the personal shift is still opened or closed for the user on whose behalf the plugin is acting, this user, as before, is determined by the `credentials` argument obtained as a result of [authorization](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_AuthenticateByPin.htm) by PIN code..

Now, acting on behalf of a service user with a known PIN code, the plugin can open or close the personal shift of another user, specified as `user`, without knowing his PIN code. To manage someone else's personal shift, a service user must have the following rights:

- `F_OPIN` “Confirm opening and closing of personal shift using PIN code” - for opening someone else’s personal shift
- `F_KIS` “Forcibly close personal shifts” - to close someone else’s personal shift
