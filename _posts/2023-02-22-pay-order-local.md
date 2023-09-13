---
title: Pay for your order at the current cash register
layout: default
---

Previously, orders from the API could only be paid at the main terminal.
In this case, the main terminal had to be a cash register (with a connected cash registrar).
In one of the [previous versions]({{ site.baseurl }}/2020/12/23/terminals-group-changes.html)
we made an attempt to get rid of the term “Main Cash Desk” by decoupling the main terminal from the point of sale and, as a result, from the fiscal registrar.
API V8 took the next step by allowing payment for the order on the local terminal.


In methods[`PayOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_PayOrder.htm)
and [`PayOrderAndPayOutOnUser`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_PayOrderAndPayOutOnUser.htm)
a new argument `isPaymentLocal` has been added, which depends on
will the order be paid at the local fiscal registrar,
or it will be paid at the fiscal registrar of the main terminal.

Let us remind you that payment for the order using
[`PayOrderAndPayOutOnUser`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_PayOrderAndPayOutOnUser.htm)
cash, creates debt for waiters.
Previously, such debt was taken into account only at the main terminal.
Now the debt is taken into account at all terminals at which such payment took place.
Such a debt can only be repaid at the terminal on which it was generated (such payment took place).