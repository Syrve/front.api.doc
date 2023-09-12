---
title: Payment Actions
layout: default
---
## Adding Payments
The following methods are used to add payments

- [IEditSession.AddPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/Overload_Resto_Front_Api_Editors_IEditSession_AddPaymentItem.htm) — add a payment

- [IEditSession.AddExternalPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/Overload_Resto_Front_Api_Editors_IEditSession_AddExternalPaymentItem.htm) — add an external payment

- [IEditSession.AddPreliminaryPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/Overload_Resto_Front_Api_Editors_IEditSession_AddPreliminaryPaymentItem.htm) — add an advance payment (*it is useful for delivery orders only*)

You can also use the same-name [methods](https://syrve.github.io/front.api.sdk/v6/html/Methods_T_Resto_Front_Api_Extensions_OperationServiceExtensions.htm) of extension operations where the [editing session]({{ site.baseurl }}/v6/en/Data%20editing.html) is created implicitly (*as opposed to multiple order actions. If apart from payment, you need, for instance, to add a guest, use methods within the editing session*). To set up and register external payment types, see the [External Payment Types](PaymentProcessor.html) article.

##### Payment restrictions:
Payment can be added only if the order is `New` or the `Guest Bill` is printed, otherwise, the method throws the [`ConstraintViolationException`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_ConstraintViolationException.htm)exception. Besides, you cannot add several unposted payments of one type (*NOTE: it is planned to lift this restriction*).

##### Examples

- Adding cash payments

```cs
var deliveryOrder = PluginContext.Operations.GetDeliveryOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "DINERS");
var additionalData = new CardPaymentItemAdditionalData { CardNumber = "123456" };
PluginContext.Operations.AddPreliminaryPaymentItem(150, additionalData, paymentType, deliveryOrder, PluginContext.Operations.GetCredentials());
```

- Adding card payments to delivery orders

```cs
var deliveryOrder = PluginContext.Operations.GetDeliveryOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "DINERS");
var additionalData = new CardPaymentItemAdditionalData { CardNumber = "123456" };
PluginContext.Operations.AddPreliminaryPaymentItem(150, additionalData, paymentType, deliveryOrder, PluginContext.Operations.GetCredentials());
```

- Adding external unposted payments

```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "DINERS");
var additionalData = new CardPaymentItemAdditionalData { CardNumber = "123456" };
PluginContext.Operations.AddExternalPaymentItem(150, false, additionalData, paymentType, order, PluginContext.Operations.GetCredentials());
```

![card](../../img/payment/api_cardExternal.png)

Comments:
- The `PluginContext.Operations.GetOrders().Last(...)` expression is used in the examples, which means getting the last order on the list. Use a corresponding selection criterion to solve business tasks.
- `PluginContext.Operations.GetCredentials()` — hereinafter, examples show an extension method [made available](https://github.com/iiko/front.api.sdk/blob/master/sample/v6/Resto.Front.Api.SamplePlugin/OperationServiceExtensions.cs) in the SamplePlugin example.

## Payment for the order
The following methods are used to pay orders:

- [IOperationService.PayOrder](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_PayOrder.htm) — order payment

- [IOperationService.PayOrderAndPayOutOnUser](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_PayOrderAndPayOutOnUser.htm) — order payment to waiter
there is also a method to convert the payment into prepayment in the POS

- [IOperationService.ProcessPrepay](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_ProcessPrepay.htm)

If the payment is effected using the fiscal cash method, then the user, on behalf of which the `IOperationService.PayOrderAndPayOutOnUser` is made, will have the [fiscal withdrawal](https://en.syrve.help/articles/#!syrve-pos-8-5/pay-order-to-waiter).

#### Примеры

##### Оплата заказа, в котором достаточно внесенных денежных средств

Пусть существует заказ IOrder order, который необходимо закрыть в iikoFront и в заказ уже внесено достаточно проведенных оплат
(*проведенные элементы оплаты это либо предоплаты, внесенные на экране кассы iikoFront, либо оплаты, добавленные по инициативе плагина методом `AddExternalPaymentItem` с флагом `isProcessed` равным `true`*).
Для такого заказа можно вызвать метод:
```cs
operationService.PayOrder(credentials, order);
```

![payOrder](../../img/payment/api_payOrder.png)

##### Оплата заказа наличными с расчетом официанту

Пусть существует заказ `IOrder order`, который необходимо оплатить наличными на всю сумму заказа и закрыть в iikoFront.
Для этого нужно выбрать соответствующий `paymentType`.
Затем вызывается метод `IOperationService.PayOrderAndPayOutOnUser`.
В примере ниже заказ оплачивается наличными на всю сумму заказа:
```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New || o.Status == OrderStatus.Bill);
var credentials = PluginContext.Operations.AuthenticateByPin("777");
var paymentType = operationService.GetPaymentTypesToPayOutOnUser().First(x => x.IsCash);
PluginContext.Operations.PayOrderAndPayOutOnUser(credentials, order, paymentType, order.ResultSum);
```

##### Оплата плагинным типом оплаты

Пусть существует `IOrder order` который нужно оплатить плагинным типом оплаты.

```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().Single(i => i.Kind== PaymentTypeKind.External && i.Name == "SamplePaymentType");
var credentials = PluginContext.Operations.GetCredentials();
PluginContext.Operations.PayOrderAndPayOutOnUser(credentials, order, paymentType, order.ResultSum);
```

##### Комментарии:
- При оплате заказа на главной кассе (в кассовую смену главной кассы) с помощью методов [IOperationService.PayOrder](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_PayOrder.htm) или
 [IOperationService.PayOrderAndPayOutOnUser](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_PayOrderAndPayOutOnUser.htm) заказ закроется, распечатаются все необходимые квитанции, будет напечатан фискальный чек, а сам заказ окажется в закрытых в iikoFront (_NOTE: дополнить раздел пояснением про фискальное изъятие в расчет официанту_).

### Возврат оплаты
Возврат оплаты и возврат заказов по инициативе плагина пока не реализован, поэтому должен выполняться со стационарных терминалов iikoFront пользователем системы.

### Проведение оплаты

#### Добавление в заказ оплаты, проведенной на внешней стороне
В некоторых случаях требуется добавить такой элемент оплаты в заказ, обработка которого уже выполнена вне iikoFront.
Для этого используется метод `AddExternalPaymentItem`, которому в параметре `isProcessed` передается значение `true`.
В данном случае iikoFront считает, что необходимые транзакции, связанные с элементом оплаты, были выполнены вовне.
Поэтому со своей стороны не предпринимает действий по обработке и помечает данный элемент оплаты проведенным.

##### Пример
```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(x => x.Kind == PaymentTypeKind.Cash);
PluginContext.Operations.AddExternalPaymentItem(150, true, null, paymentType, order, PluginContext.Operations.GetCredentials());
```

![cash](../../img/payment/api_cashExternalProcessed.png)

#### Добавление в заказ проведенной оплаты и превращение ее в предоплату iikoFront

Иногда требуется в заказ добавить такой элемент оплаты, чтобы он отображался в отчетах iikoOffice до закрытия заказа.
Тогда в заказ нужно добавить оплату методом `AddExternalPaymentItem` с параметром `isProcessed` равным `true`, затем вызвать метод [IOperationService.ProcessPrepay](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_ProcessPrepay.htm).

##### Пример
```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(x > x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "DINERS");
var additionalData = new CardPaymentItemAdditionalData { CardNumber  "123456" };
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddExternalPaymentItem(150, true, additionalData, paymentType, order, credentials);
order = PluginContext.Operations.GetOrderById(order.Id);
PluginContext.Operations.ProcessPrepay(credentials, order, paymentItem);
```
![card](../../img/payment/api_cardExternalPrepay.png)

#### Добавление в заказ предварительного платежа с последующим превращением в предоплату iikoFront

Для оплаты доставочного заказа следует сначала добавить предварительную оплату с помощью метода [IEditSession.AddPreliminaryPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/Overload_Resto_Front_Api_Editors_IEditSession_AddPreliminaryPaymentItem.htm), а после вызвать метод [IOperationService.ProcessPrepay](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_ProcessPrepay.htm).

##### Пример
```cs
var order = PluginContext.Operations.GetDeliveryOrders().Last(o => o.Status == OrderStatus.New || o.Status == OrderStatus.Bill);
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(i => i.Kind == PaymentTypeKind.Cash);
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddPreliminaryPaymentItem(order.ResultSum, null, paymentType, order, credentials);
PluginContext.Operations.ProcessPrepay(credentials, PluginContext.Operations.GetDeliveryOrderById(order.Id), paymentItem);
```

## Типы оплат, которые поддерживают тихое проведение (Silent-оплата)
Порой клиентам нужна возможность проводить предоплату или вносить чаевые по инициативе плагина (без входа на попап предоплаты или чаевых iikoFront) непроведенным типом оплаты (для последующего проведения на стороне iikoFront).
Непроведенный тип оплаты подразумевает, что он был создан c флагом `isProcessed` равным `false`.
Проведение какой-либо оплаты по инициативе плагина требует, чтобы оплата поддерживала так называемую тихую оплату, когда для проведения не требуется взаимодействие с пользовательским интерфейсом iikoFront (например, для сбора данных), считается, что все необходимые данные уже собраны.
Среди типов оплат, поддерживающих тихую оплату, есть те, которые поддерживают ее по умолчанию:
- Наличные
- Банковские карты, платежная система которых в задана как "Внешняя".
Настройка платежной системы типа оплаты в iikoOffice должна быть следующей:
![paymentType](../../img/payment/cardPaymentType.png)

Также, поддержку тихой оплаты можно реализовать для внешнего плагинного типа.
Подробнее см. в разделе [Внешние типы оплаты](PaymentProcessor.html).

**Дополнительно:**
- Добавление чаевых в заказ см. в разделе [Чаевые](Donations.html).

## Удаление оплат

- [IEditSession.DeletePaymentItem](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_DeletePaymentItem.htm) &mdash; удалить оплату

- [IEditSession.DeleteExternalPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_DeleteExternalPaymentItem.htm) &mdash; удалить внешнюю оплату

- [IEditSession.DeletePreliminaryPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_DeletePreliminaryPaymentItem.htm) &mdash; удалить предварительную оплату _(имеет смысл только для доставочного заказа)_

##### Пример

- Удаление внешнего элемента оплаты из заказа
```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var paymentItem = order.Payments.FirstOrDefault(i => i.IsExternal);
if (paymentItem != null)
    PluginContext.Operations.DeleteExternalPaymentItem(paymentItem, order, PluginContext.Operations.GetCredentials());
```

Больше примеров можно найти в проекте SDK SamplePaymentPlugin.