---
title: Покупка потребляемых продуктов в Xamarin. iOS
description: В этом документе описываются потребляемые продукты в Xamarin. iOS. Потребляемые продукты являются единым набором функций, таких как «валюта в игре».
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: d6cc17d4a3a77487689357641999525a539b442f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435535"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Покупка потребляемых продуктов в Xamarin. iOS

Использование потребляемых продуктов проще всего реализовать, так как нет необходимости в восстановлении. Они полезны для таких продуктов, как используемая в игре валюта или единая функция использования. Пользователи могут повторно приобрести потребляемые продукты повторно.

## <a name="built-in-product-delivery"></a>Встроенная Доставка продуктов

В образце кода, прилагаемом к этому документу, показаны встроенные продукты — идентификаторы продуктов жестко связаны с приложением, так как они тесно связаны с кодом, который разблокирует функцию после оплаты. Процесс покупки можно выработать следующим образом:   
   
[![Визуализация процесса покупки](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Базовый рабочий процесс:   
   
 1. Приложение добавляет объект `SKPayment` в очередь. При необходимости пользователю будет предложено ввести идентификатор Apple ID и появится запрос на подтверждение оплаты.   
   
 2. StoreKit отправляет запрос на сервер для обработки.   
   
 3. По завершении транзакции сервер отвечает за получение транзакции.   
   
 4. `SKPaymentTransactionObserver`Подкласс получает уведомление и обрабатывает его.   
   
 5. Приложение включает продукт (путем обновления `NSUserDefaults` или какого-либо другого механизма), а затем вызывает StoreKit `FinishTransaction` .

Существует другой тип рабочего процесса — *доставленные серверные продукты* , которые обсуждаются далее в документе (см. раздел *Проверка поступления и серверные продукты*).

## <a name="consumable-products-example"></a>Пример использования продуктов

[Код инапппурчасесампле](/samples/xamarin/ios-samples/storekit) содержит проект с именем *расходных материалов* , который реализует базовую валюту "в игре" (называемую "обезьяной". В этом примере показано, как реализовать два продукта, приобретенных в приложении, чтобы позволить пользователю купить столько «кредитов», сколько нужно. в реальных приложениях это также будет быть каким-то способом.   

Приложение отображается на этих снимках экрана — каждая покупка добавляет к балансу пользователя дополнительные "кредиты".   

 [![Каждая покупка добавляет кредитные кредиты в баланс пользователей](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   

Взаимодействие между пользовательскими классами, StoreKit и App Store, выглядит следующим образом:   

 [![Взаимодействие между пользовательскими классами, StoreKit и App Store](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

### <a name="viewcontroller-methods"></a>Методы ViewController

В дополнение к свойствам и методам, необходимым для получения сведений о продукте, контроллеру представления требуются дополнительные наблюдатели уведомлений для прослушивания уведомлений, связанных с покупкой. Это только `NSObjects` то, что будет зарегистрировано и удалено в `ViewWillAppear` и `ViewWillDisappear` соответственно.

```csharp
NSObject succeededObserver, failedObserver;
```

Конструктор также создаст `SKProductsRequestDelegate` подкласс ( `InAppPurchaseManager` ), который, в свою очередь, создает и регистрирует `SKPaymentTransactionObserver` ( `CustomPaymentObserver` ).   

Первая часть обработки транзакции покупки в приложении заключается в обработке нажатия кнопки, когда пользователь хочет купить что-либо, как показано в следующем коде из примера приложения:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

Вторая часть пользовательского интерфейса обрабатывает уведомление о том, что транзакция была успешной, в данном случае путем обновления отображаемого баланса:

```csharp
succeededObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Последняя часть пользовательского интерфейса отображает сообщение, если транзакция отменяется по какой-либо причине. В примере кода сообщение просто записывается в окно вывода:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Помимо этих методов на контроллере представления, для используемой транзакции покупки продукта также требуется код в `SKProductsRequestDelegate` и `SKPaymentTransactionObserver` .

### <a name="inapppurchasemanager-methods"></a>Методы Инапппурчасеманажер

В примере кода реализован ряд методов, связанных с покупкой, для класса Инапппурчасеманажер, включая `PurchaseProduct` метод, который создает `SKPayment` экземпляр и добавляет его в очередь для обработки:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Добавление платежа в очередь является асинхронной операцией. Приложение восстанавливает управление, пока StoreKit обрабатывает транзакцию и отправляет ее на серверы Apple. На этом этапе iOS будет проверять, что пользователь вошел в App Store, и при необходимости запрашивать идентификатор Apple ID и пароль.   

Предполагая, что пользователь успешно прошел проверку подлинности в магазине приложений и соглашается с транзакцией, он `SKPaymentTransactionObserver` получает ответ StoreKit и вызывает следующий метод для выполнения транзакции и ее завершения.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Последний шаг заключается в том, чтобы уведомить StoreKit о том, что вы успешно выполнили транзакцию, вызвав `FinishTransaction` :

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

После доставки продукта `SKPaymentQueue.DefaultQueue.FinishTransaction` необходимо вызвать метод, чтобы удалить транзакцию из очереди платежей.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Методы Скпайменттрансактионобсервер (Кустомпайментобсервер)

StoreKit вызывает `UpdatedTransactions` метод при получении ответа от серверов Apple и передает в `SKPaymentTransaction` код массив объектов для проверки. Метод выполняет цикл по каждой транзакции и осуществляет другую функцию на основе состояния транзакции (как показано ниже):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`CompleteTransaction`Метод был рассмотрен ранее в этом разделе — он сохраняет сведения о приобретении в `NSUserDefaults` , завершает транзакцию с помощью StoreKit и, наконец, сообщает пользовательскому интерфейсу об обновлении.

### <a name="purchasing-multiple-products"></a>Приобретение нескольких продуктов

Если в приложении есть смысл приобрести несколько продуктов, используйте `SKMutablePayment` класс и задайте поле Quantity:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Код, обрабатывающий завершенную транзакцию, должен также запросить свойство Quantity, чтобы правильно удовлетворить покупку:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Когда пользователь приобретает несколько количеств, в оповещении StoreKit будет отражено количество, Цена за единицу и Общая цена, на которую будет произведена плата, как показано на следующем снимке экрана:

[![Подтверждение покупки](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Обработка простоев сети

Для покупок из приложений требуется работающее сетевое подключение для StoreKit, чтобы взаимодействовать с серверами Apple. Если сетевое подключение недоступно, покупка в приложении будет недоступна.

### <a name="product-requests"></a>Запросы продукта

Если сеть недоступна во время создания `SKProductRequest` , `RequestFailed` `SKProductsRequestDelegate` будет вызван метод подкласса ( `InAppPurchaseManager` ), как показано ниже:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

Затем ViewController прослушивает уведомление и выводит сообщение на кнопки покупки:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Так как сетевое подключение может быть временным на мобильных устройствах, приложениям может потребоваться мониторинг состояния сети с помощью платформы SystemConfiguration Framework и повторная попытка, когда будет доступно сетевое подключение. Обратитесь к Apple или в том, на котором он используется.

### <a name="purchase-transactions"></a>Транзакции покупки

По возможности очередь оплаты StoreKit будет хранить и пересылать запросы на покупку, поэтому воздействие сбоя сети будет зависеть от того, когда в процессе приобретения произошел сбой сети.   

Если во время транзакции происходит ошибка, то `SKPaymentTransactionObserver` подкласс ( `CustomPaymentObserver` ) будет иметь `UpdatedTransactions` вызываемый метод, а `SKPaymentTransaction` класс будет находиться в состоянии Failed.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction`Метод определяет, была ли ошибка вызвана отменой пользователем, как показано ниже:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Даже если транзакция завершается неудачно, `FinishTransaction` необходимо вызвать метод, чтобы удалить транзакцию из очереди платежей:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Затем в примере кода отправляется уведомление, чтобы ViewController мог отобразить сообщение. Приложения не должны показывать дополнительное сообщение, если пользователь отменил транзакцию. Ниже перечислены другие коды ошибок, которые могут возникнуть.

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Ограничения обработки

**Параметры > общие > ограничения** в iOS позволяют пользователям блокировать определенные функции устройства.   

Вы можете запросить, разрешено ли пользователю делать покупки в приложении с помощью `SKPaymentQueue.CanMakePayments` метода. Если этот параметр возвращает значение false, пользователь не может получить доступ к покупкам в приложении. StoreKit автоматически отображает пользователю сообщение об ошибке при попытке покупки. Проверив это значение, приложение может скрыть кнопки покупки или предпринять другие действия, чтобы помочь пользователю.   

В `InAppPurchaseManager.cs` файле `CanMakePayments` метод создает оболочку для функции StoreKit следующим образом:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Чтобы протестировать этот метод, используйте функцию " **ограничения** " в iOS, чтобы отключить **покупки в приложении**:   

 [![Использование функции ограничений в iOS для отключения покупок в приложении](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   

В этом примере кода из `ConsumableViewController` реагирует на `CanMakePayments` возвращение значения false путем отображения **AppStore отключенного** текста на отключенных кнопках.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

Приложение выглядит следующим образом, если функция " **покупки в приложении** " ограничена — кнопки "Покупка" отключены.   

 [![Приложение выглядит следующим образом, если функция "покупки в приложении" ограничена, кнопки "Покупка" отключены](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   

Сведения о продукте по-прежнему могут быть запрошены `CanMakePayments` , если значение равно false, поэтому приложение по-прежнему может получать и отображать цены. Это означает, что при удалении `CanMakePayments` проверки из кода кнопки покупки по-прежнему будут активны, однако при попытке покупки пользователь увидит сообщение о том, что **покупки в приложении не разрешены** (создаются StoreKit при обращении к очереди оплаты):   

 [![Покупки из приложения не разрешены](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   

Реальные приложения могут использовать другой подход к обработке ограничений, например, полностью скрыть кнопки и, возможно, предложить более подробное сообщение, чем предупреждение, которое StoreKit показано автоматически.