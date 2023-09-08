---
title: External Payment Types
layout: default
---
# Integration With External Payment Types
## Overview 
If you implement the [`IExternalPaymentProcessor`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IExternalPaymentProcessor.htm) interface and register it properly, a new payment system will be available in Syrve RMS.
For simplicity, we call it the external payment type. 
Plugins used for external payment types must be [licensed](Licensing.html).


## External payment system registration
A plugin registers an external payment type using [`IOperationService.RegisterPaymentSystem(...)`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_RegisterPaymentSystem.htm).
`paymentSystem` is a required parameter to be passed to this method. It is a class instance that implements `IExternalPaymentProcessor`. 
Upon registration, a new payment system will be available in Syrve Office.
You can find it under the name IExternalPaymentProcessor.PaymentSystemName in the «Non-cash» type field if you select the «External Payment Type» as the payment type in the new payment type window.

![backPT](../../img/payment/backPT.png)

Если создать [тип оплаты](https://ru.iiko.help/smart/project-iikooffice/topic-103) этой платёжной системы, на iikoFront на экранах кассы и предоплаты появится возможность выбрать этот внешний тип оплаты.

![frontPT](../../img/payment/frontPT.png)


Пояснения к терминам:

- *Платёжная система* – декларирует, как именно осуществляется проведение и возврат платежей. На весь iikoRMS платёжная система декларируется в единственном экземпляре. 
- *Тип оплаты* - относится к какой-либо платёжной системе.
Имеет настраиваемые свойства вроде фискальности, счетов перемещения и т.д.
Если сделать внешний тип оплаты фискальным, он попадёт в раздел *«Банковские карты»*, если оставить нефискальным, он будет в разделе *«Безналичный расчёт»*.
Можно заводить сколько угодно типов оплаты каждой платёжной системы.   
- *Элемент оплаты* – относится к заказам в iikoFront. Когда пользователь [на экране кассы](https://ru.iiko.help/smart/project-iikofront/topic-72) или в [окне предоплат и платежей](https://ru.iiko.help/smart/project-iikofront/topic-72/a/h2_10) выбирает какой-либо тип оплаты, в заказ добавляется элемент оплаты соответствующего типа оплаты.
Также элементы оплаты могут быть добавлены средствами API ([Добавление оплат](Payments.html)).

## Интерфейс IExternalPaymentProcessor
Чтобы реализовать необходимую бизнес-логику по проведению и возврату платежа внешним типом оплаты, нужно реализовать интерфейс [`IExternalPaymentProcessor`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IExternalPaymentProcessor.htm):
```cs
public interface IExternalPaymentProcessor
{
    string PaymentSystemKey { get; }
    string PaymentSystemName { get; }
    
    void CollectData(Guid orderId, Guid paymentTypeId, [NotNull] IUser cashier, IReceiptPrinter printer, UI.IViewManager viewManager, IPaymentDataContext context, UI.IProgressBar progressBar);
    void OnPaymentAdded([NotNull] IOrder order, [NotNull] IPaymentItem paymentItem, [NotNull] IUser cashier, [NotNull] IOperationService operationService, IReceiptPrinter printer, UI.IViewManager viewManager, IPaymentDataContext context, UI.IProgressBar progressBar);
    bool OnPreliminaryPaymentEditing([NotNull] IOrder order, [NotNull] IPaymentItem paymentItem, [NotNull] IUser cashier, [NotNull] IOperationService operationService, IReceiptPrinter printer, UI.IViewManager viewManager, IPaymentDataContext context, UI.IProgressBar progressBar);

    void Pay(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar);
    void EmergencyCancelPayment(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar);
    void ReturnPayment(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar);
    void ReturnPaymentWithoutOrder(decimal sum, Guid paymentTypeId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IProgressBar progressBar);

    void PaySilently(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IPaymentDataContext context);
    void EmergencyCancelPaymentSilently(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IPaymentDataContext context);
    bool CanPaySilently(decimal sum, Guid? orderId, Guid paymentTypeId, IPaymentDataContext context);
}
```

Здесь:

- `PaymentSystemKey` – это уникальный ключ, с которым регистрируется новая внешняя платёжная система.
- `PaymentSystemName` – имя, которое будет показываться на UI в iikoOffice.


## Метод проведения оплаты
Когда пользователь iikoFront выберет на экране кассы тип оплаты, задаст сумму и нажмёт кнопку *«Оплатить»*, или когда пользователь внесёт предоплату определенным типом оплаты, управление придёт в метод [`Pay()`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_Pay.htm):
```cs
void Pay(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar);
```

Здесь:

- `sum` – сумма платежа;
- `orderId` – id заказа в iikoFront;
- `paymentTypeId` – id типа оплаты. Список всех типов оплаты можно получить методом [`IOperationService.GetPaymentTypes()`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_GetPaymentTypes.htm), конкретный тип оплаты можно получить методом [`OperationService_TryGetPaymentTypeById(...)`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_TryGetPaymentTypeById.htm);
- `transactionId` – id транзакции;
- `pointOfSale` – [Точка продаж](GroupsAndPointsOfSale.html), на которой проводится данный элемент оплаты;
- `cashier` – кассир;
- `printer` – экземпляр [`IReceiptPrinter`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IReceiptPrinter.htm), который дает возможность печатать на принтере квитанций iikoFront;
- `viewManager` – экземпляр [`IViewManager`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_UI_IViewManager.htm) для [показа окон](ViewManager.html) в процессе проведения оплаты;
- `context` – экземпляр [`IPaymentDataContext`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IPaymentDataContext.htm) для сохранения данных в элемент оплаты;
- `progressBar` – экземпляр [`IProgressBar`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_UI_IProgressBar.htm) для изменения текста на прогрессбаре в процессе проведения оплаты.

Подробнее сигнатуру объектов можно найти [в документации](https://syrve.github.io/front.api.sdk/v6).

Например, если требуется реализовать интеграцию с гостиничной системой:

- В процессе оплаты нужно спросить у пользователя номер комнаты или карту от комнаты.
- Затем обратиться к гостиничному сервису.
- В случае успеха напечатать квитанцию с суммой и именем гостя, которое нам вернет гостиничная система.
- В iikoRMS сохранить введенный номер или прокатанную карту для просмотра этих данных в OLAP.
- Для последующей отмены понадобится знать, карта ли была прокатана или введен номер.
- В случае неуспеха прервать оплату.

```cs
[Serializable]
internal class IsCardClass
{
    public bool IsCard;
}
public void Pay(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, IPointOfSale pointOfSale,  IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar)
{
    // Показать в iikoFront окно ввода номера и прокатки карты
    var input = viewManager.ShowInputDialog("Введите номер или прокатайте карту", InputDialogTypes.Card | InputDialogTypes.Number);
    string room = null;
    string cardTrack = null;
    
    // Если был введен номер, то результат типа NumberInputDialogResult
    var roomNum = input as NumberInputDialogResult;
    if (roomNum != null)
        room = roomNum.Number.ToString();
    
    // Если была прокатана карта, то результат типа CardInputDialogResult
    var card = input as CardInputDialogResult;
    if (card != null)
        cardTrack = card.FullCardTrack;
    
    if (room == null && cardTrack == null)
        // Ничего не было введено, прекращаем операцию.
        throw new PaymentActionFailedException("Не было введено данных.");
    
    // Получаем заказ средствами API по id через IOperationService.
    var order = PluginContext.Operations.TryGetOrderById(orderId.Value);
    
    // Выполняем произвольные методы. Например, проводим платёж в некой hotelSystem, которая вернет имя гостя, если платёж «принят» и null, если платёж отклонён.
    var guestName = hotelSystem.ProcessPaymentOnGuest(cardTrack, room, order?.Number, transactionId, sum);
    if (guestName == null)
        // Платёж не прошёл, прекращаем операцию.
        throw new PaymentActionFailedException("Платеж не прошёл.");
    
    // Формирование квитанции для печати. Квитанция состоит из XElement
    var slip = new ReceiptSlip
    {
        Doc =  new XElement(Tags.Doc,
            new XElement(Tags.Pair, "Гость", guestName),
            new XElement(Tags.Pair, "Сумма", sum))
    };
    
    // Печать.
    printer.Print(slip);
    var cardInfoData = new IsCardClass { IsCard = card != null };
    var cardType = cardInfoData.IsCard
        ? "My Hotel System Card"
        : "My Hotel System Room";
    // Сохранение данных, которые будут показаны в отчётах.
    context.SetInfoForReports(room ?? cardTrack, cardType);
    // Сохранение данных, которые будут использованы для возврата оплаты.
    context.SetRollbackData(cardInfoData);
}
```
 
Исключение типа [`PaymentActionFailedException`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_PaymentActionFailedException.htm) служит для прерывания операции оплаты.
Пользователю iikoFront будет показан `message` этого исключения. Это имеет смысл, если возникли какие-либо проблемы при общении с внешним сервисом, оплата не может быть проведена и нужно проинформировать пользователя о причинах. 

Для «тихого» прерывания операции можно воспользоваться исключением типа [`PaymentActionCancelledException`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_PaymentActionCancelledException.htm).
Это имеет смысл, если в процессе оплаты было показано диалоговое окно и пользователь нажал кнопку *«Отмена»*. 

Аргументы [`IReceiptPrinter`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IReceiptPrinter.htm), [`IViewManager`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_UI_IViewManager.htm) и  [`IPaymentDataContext`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IPaymentDataContext.htm) «живут» только в процессе выполнения метода, после завершения метода экземпляры уничтожаются.
Так что сохранять их в переменные не имеет смысла, т.к. вне метода их нельзя будет использовать.

### Тихое проведение оплаты (Silent-оплата)
Иногда бизнесу нужны решения по оплате плагинными типами оплаты из самих плагинов, без входа на экран кассы iikoFront.
Для этого плагин должен реализовать метод [CanPaySilently](https://iiko.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_CanPaySilently.htm) процессора оплаты плагина.
Результатом метода является ответ на вопрос _«Имеет ли плагин возможность проводить оплату тихо?»_.
Для того чтобы появилась такая возможность, необходимо чтобы в заказ предварительно был добавлен плагинный элемент оплаты.
Для тихого проведения оплаты можно вызвать метод [ProcessPrepay](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_ProcessPrepay.htm) c флагом `isProcessed` равным `false`.
В `SDK` приводится пример с использованием пользовательского класса со свойством _SilentPay_:
```cs
[Serializable]
public class PaymentAdditionalData
{
    public bool SilentPay { get; set; }
}

private string Serialize<T>(T data) where T : class
{
    using (var sw = new StringWriter())
    using (var writer = XmlWriter.Create(sw))
    {
        new XmlSerializer(typeof(T)).Serialize(writer, data);
        return sw.ToString();
    }
}
private void AddAndProcessExternalPrepay()
{
    var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
    var paymentType = PluginContext.Operations.GetPaymentTypes().Single(i => i.Kind == PaymentTypeKind.External && i.Name == "SamplePaymentType");
    
    var additionalData = new ExternalPaymentItemAdditionalData
    {
        CustomData = Serialize(new PaymentAdditionalData {SilentPay = true})
    };
    var credentials = PluginContext.Operations.AuthenticateByPin("777");
    var paymentItem = PluginContext.Operations.AddExternalPaymentItem(order.ResultSum, false, additionalData, paymentType, order, credentials);

    PluginContext.Operations.ProcessPrepay(credentials, order, paymentItem);
}
```
В свою очередь iikoFront передает указанный для оплаты сериализованный класс в контекст оплаты([IPaymentContext](https://iiko.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_IPaymentDataContext.htm)), далее в методе [CanPaySilently](https://iiko.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_CanPaySilently.htm) класс извлекается и десериализуется:
```
public bool CanPaySilently(decimal sum, Guid? orderId, Guid paymentTypeId, IPaymentDataContext context)
{
    var customData = context.GetCustomData<PaymentAdditionalData>();    
    return customData?.SilentPay ?? false;
}
```
В зависимости от ответа возвращаемого значения метода [CanPaySilently](https://iiko.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_CanPaySilently.htm) iikoFront вызовет `Pay` или `PaySilently` метод процессора плагина.
Таким образом плагин сам решает как должен проводиться вновь добавляемый платеж.

## Методы возврата оплаты

```cs
void EmergencyCancelPayment(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar);
void ReturnPayment(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar);
void ReturnPaymentWithoutOrder(decimal sum, Guid paymentTypeId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IProgressBar progressBar);
void EmergencyCancelPaymentSilently(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IPaymentDataContext context);
```

Методы `EmergencyCancelPayment()` и `ReturnPayment()` вызываются, когда пользователь на iikoFront инициирует возврат проведённого ранее платежа.

В метод `ReturnPayment()` управление передаётся, когда на экране закрытого заказа нажимают кнопку *«Частичный возврат чека»* или *«Удаление заказа»*.
Или если пользователь удаляет проведённую предоплату.
В метод `EmergencyCancelPayment()` управление передаётся, когда для ещё не закрытого заказа отменяют уже проведённую оплату.
Например, если оплата фискальная и возникли трудности с печатью фискального чека и оплату прерывают.
Если для второго случая не требуется специфической логики, то можно просто вызвать из метода `EmergencyCancelPayment()` метод `ReturnPayment()`. 

Методы принимают те же параметры, что и метод оплаты. `transactionId` тот же, что передавался в исполненную ранее операцию `Pay()`.

Методы считаются успешно завершёнными, если в процессе выполнения не возникло исключений типа `PaymentActionFailedException` или `PaymentActionCancelledException`. Если эти исключения возникли, так же, как и для оплаты, операция возврата прерывается.

Пример кода интеграции с гостиничной системой.
Метод возврата отменяет транзакцию и печатает чек с отменяемой суммой и сохранёнными данными: была ли прокатана карта, или был введён номер.

```cs
[Serializable]
public class IsCardClass
{
    public bool IsCard;
}

public void ReturnPayment(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar)
{
    // Выполняктся произвольные методы. Например, по id транзакции платёж   озвращаетется в некой hotelSystem, которая вернет true, если платёж  успешно ткатился и false, если возврат не удался.
    var success = hotelSystem.ProcessReturnPayment(transactionId);
    if (!success)
        throw new PaymentActionFailedException("Не получилось вернуть плату.");
    
    // Получаем данные, сохранённые в элементе оплаты.
    var isCard = context.GetRollbackData<IsCardClass>();
    
    var slip = new ReceiptSlip
    {
        Doc =  new XElement(Tags.Doc, 
            new XElement(Tags.Pair, "Возврат суммы", sum),
            new XElement(Tags.Pair, "Была ли карта", isCard.IsCard ? "ДА" :    "НЕТ" ))
    };
    printer.Print(slip);
}

public void EmergencyCancelPayment(decimal sum, Guid? orderId, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context, IProgressBar progressBar)
{
    ReturnPayment(sum, orderId, paymentTypeId, transactionId, pointOfSale, cashier, printer, viewManager, context, progressBar);
}
```

Метод [`ReturnPaymentWithoutOrder()`](https://iiko.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_ReturnPaymentWithoutOrder.htm) вызывается, когда происходит [возврат товаров](https://ru.iiko.help/smart/project-iikofront/topic-38) внешним типом оплаты.
Возможность возвращать оплату за товары без оплаченных ранее заказов внешними типами появилась начиная с версии iiko 6.2.2. 
Чтобы на UI возврата товаров появилась возможность выбрать внешний тип оплаты, нужно регистрировать платёжную систему с опциональным параметром `canProcessPaymentReturnWithoutOrder = true`. Т.е.

```cs
var disposable = PluginContext.Operations.RegisterPaymentSystem(paymentSystem, true);
```

В отличие от всех упомянутых выше методов, метод `ReturnPaymentWithoutOrder()` не имеет контекста заказа и проведённой ранее оплаты.
Предполагается, что суммы и типа оплаты достаточно для выполнения возврата.
В процессе данной операции доступна возможность показывать пользователю диалоговые окна и печатать квитанции, так же, как и для всех упомянутых выше методов. 

## Методы сбора данных

```cs
void CollectData(Guid orderId, Guid paymentTypeId, [NotNull] IUser cashier, IReceiptPrinter printer, UI.IViewManager viewManager, IPaymentDataContext context, UI.IProgressBar progressBar);
void OnPaymentAdded([NotNull] IOrder order, [NotNull] IPaymentItem paymentItem, [NotNull] IUser cashier, [NotNull] IOperationService operationService, IReceiptPrinter printer, UI.IViewManager viewManager, IPaymentDataContext context, UI.IProgressBar progressBar);
bool OnPreliminaryPaymentEditing([NotNull] IOrder order, [NotNull] IPaymentItem paymentItem, [NotNull] IUser cashier, [NotNull] IOperationService operationService, IReceiptPrinter printer, UI.IViewManager viewManager, IPaymentDataContext context, UI.IProgressBar progressBar);
```

Если требуется собрать какие-либо данные не в момент нажатия на кнопку *«Оплатить»* на экране кассы, а в момент добавления элемента внешнего типа оплаты в заказ, то можно реализовать это в методе `CollectData()`. 

Метод `OnPaymentAdded()` вызывается после добавления элемента оплаты в заказ. Особенность этого метода в том, что одним из его аргументов является `IOperationService operationService`.
В отличие от [`PluginContext.Operations`](https://iiko.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_PluginContext_Operations.htm), у данного экземпляра есть полномочия вносить изменения в текущий заказ. Это нужно, например, чтобы задать сумму для добавляемого элемента оплаты или вообще добавить какое-либо блюдо в заказ.
 
Метод [`OnPreliminaryPaymentEditing()`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_OnPreliminaryPaymentEditing.htm) вызывается при редактировании предварительных платежей.
Для данного метода также доступна возможность вносить изменения в текущий заказ через аргумент `IOperationService operationService`.
Метод возвращает `bool`, смысл возвращаемого значения следующий: доступно ли изменение суммы элемента предварительной оплаты с UI после завершения данного метода. 

## Открытие и закрытие кассовой смены в iikoFront
Некоторым внешним платёжным системам нужно выполнять на своей стороне определённые действия при открытии и закрытии кассовой смены на iikoFront.
Например, для банковских систем при закрытии смены нужно проводить сверку.
Для этого нужно подписаться на [`INotificationService.SubscribeOnCafeSessionOpening`](https://iiko.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_INotificationService_SubscribeOnCafeSessionOpening.htm) и [`INotificationService.SubscribeOnCafeSessionClosing`](https://iiko.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_INotificationService_SubscribeOnCafeSessionClosing.htm).

При открытии и закрытии кассовой смены в соответствующий observer приходит новое событие.
Пример кода, который при открытии и закрытии смены печатает на принтере ключ платежной системы и открыта или закрыта смена:

```cs
ctor
{
    // ...   
    PluginContext.Notifications.SubscribeOnCafeSessionClosing(CafeSessionClosing);
    PluginContext.Notifications.SubscribeOnCafeSessionOpening(CafeSessionOpening)                
}

private void CafeSessionOpening([NotNull] IReceiptPrinter printer, [NotNull] IProgressBar progressBar)
{
    PluginContext.Log.Info("Cafe session opening.");
    var message =
        "Я не могу подключиться к своему серверу и открыть смену.";
    PluginContext.Operations.AddNotificationMessage(message, "SamplePaymentPlugin");
}

private void CafeSessionClosing([NotNull] IReceiptPrinter printer, [NotNull] IProgressBar progressBar)
{
    PluginContext.Log.Info("Cafe session closing.");
    var slip = new ReceiptSlip
    {
        Doc = new XElement(Tags.Doc,
            new XElement(Tags.Center, PaymentSystemKey),
            new XElement(Tags.Center, "Cafe session closed."))
    };
    printer.Print(slip);
}

```
Если необходимо показать пользователю какое-либо предупреждение, можно сделать это с помощью уведомлений.
Исключения, возникшие в процессе выполнения `CafeSessionOpening()` и `CafeSessionClosing()`, не прерывают операций открытия и закрытия кассовой смены на iikoFront.
Более того, при возникновении исключения в обработчике, он считается сломанным и не вызывается до перезапуска плагина.
 
