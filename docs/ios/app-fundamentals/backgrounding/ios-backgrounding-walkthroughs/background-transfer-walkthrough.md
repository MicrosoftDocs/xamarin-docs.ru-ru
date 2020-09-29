---
title: Фоновая перенаправление и NSURLSession в Xamarin. iOS
description: В этом документе представлено пошаговое руководство, в котором показано, как использовать фоновую пересылку и NSUrlSession, чтобы начать загрузку большого образа и продолжить загрузку, когда приложение помещается в фоновом режиме.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/02/2020
ms.openlocfilehash: b95b78ed36fcec122006cab7ce9663c43f9b3096
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436666"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Фоновая перенаправление и NSURLSession в Xamarin. iOS

Фоновая передача инициируется с помощью настройки фонового `NSURLSession` и постановку загрузки или загрузки задач. Если задачи выполняются во время фонового, приостановленного или завершенного работы приложения, iOS уведомляет приложение, вызывая обработчик завершения в *AppDelegateе*приложения. Следующая схема демонстрирует это в действии:

 [![Фоновая передача инициируется с помощью параметров Background NSURLSession и постановку Upload/Download Tasks.](background-transfer-walkthrough-images/transfer.png)](background-transfer-walkthrough-images/transfer.png#lightbox)

## <a name="configuring-a-background-session"></a>Настройка фонового сеанса

Чтобы создать фоновый сеанс, создайте новый `NSUrlSession` и настройте его с помощью `NSUrlSessionConfiguration` объекта.

Объект конфигурации определяет действия, выполняемые сеансом, и типы задач, которые он может выполнять.
Сеансы, настроенные с помощью `CreateBackgroundSessionConfiguration` метода, выполняются в отдельном процессе и выполняют передачу по усмотрению (Wi-Fi) для сохранения данных и времени работы аккумулятора.
В следующем примере кода показана правильная настройка сеанса фоновой пересылки с помощью `CreateBackgroundSessionConfiguration` метода и уникального идентификатора строки:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, new MySessionDelegate(), new NSOperationQueue());

}
```

Помимо объекта конфигурации, сеансу также требуется делегат сеанса и очередь.
Очередь определяет порядок, в котором задачи будут выполнены. Делегат сеанса чаперонес процесс передачи и обрабатывает проверку подлинности, кэширование и другие проблемы, связанные с сеансом.

## <a name="working-with-tasks-and-delegates"></a>Работа с задачами и делегатами

Теперь, когда мы настроили фоновый сеанс, давайте начнем задачи по обработке перемещения. Мы можем отследить эти задачи с помощью экземпляра, который `NSUrlSessionDelegate` называется делегатом сеанса. Делегат сеанса отвечает за пробуждение завершенного или приостановленного приложения в фоновом режиме для выполнения проверки подлинности, ошибок или завершения передачи.

`NSUrlSessionDelegate`Предоставляет следующие основные методы для проверки состояния перемещения.

- *Дидфинишевентсфорбаккграундсессион* — этот метод вызывается после завершения всех задач и завершения перемещения.
- *Дидрецеивечалленже* — вызывается для запроса учетных данных, когда требуется авторизация.
- *Дидбекомеинвалидвисеррор* — вызывается, если  `NSURLSession` станет недействительным.

Для фоновых сеансов требуются более специализированные делегаты в зависимости от типов выполняемых задач. Фоновые сеансы ограничены двумя типами задач:

- *Отправка задач* . задачи типа  `NSUrlSessionUploadTask` используют `INSUrlSessionTaskDelegate` интерфейс, реализующий `INSUrlSessionDelegate` . Это предоставляет дополнительные методы отслеживания хода передачи, обработки перенаправления HTTP и т. д.
- *Задачи скачивания* — задачи типа  `NSUrlSessionDownloadTask` Используйте `INSUrlSessionDownloadDelegate` интерфейс, который реализует `INSUrlSessionDelegate` и `INSUrlSessionTaskDelegate` . Это предоставляет все методы для отправки задач, а также методы, связанные с загрузкой, для отслеживания хода загрузки и определения момента возобновления или завершения задачи загрузки.

В следующем коде определяется задача, которую можно использовать для загрузки изображения из URL-адреса. Задача запускается путем вызова `CreateDownloadTask` в фоновом сеансе и передачи запроса URL-адреса:

```csharp
const string DownloadURLString = "http://xamarin.com/images/xamarin.png"; // or other hosted file
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Затем создайте новый делегат для скачивания сеанса, чтобы контролировать все задачи скачивания в этом сеансе. Класс делегата должен наследовать `NSObject` и реализовывать требуемый интерфейс:

```csharp
public class MySessionDelegate : NSObject, INSUrlSessionDownloadDelegate
{
  public void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Чтобы узнать ход выполнения задачи скачивания, переопределите `DidWriteData` метод для отслеживания хода выполнения и даже обновите пользовательский интерфейс. Обновления пользовательского интерфейса будут отображаться немедленно, если приложение находится на переднем плане, или будет ожидать, когда пользователь в следующий раз откроет приложение.

API делегата сеанса предоставляет широкий набор средств для взаимодействия с задачами. Полный список методов делегата сеанса см `NSUrlSessionDelegate` . в документации по API.

> [!IMPORTANT]
> Фоновые сеансы запускаются в фоновом потоке, поэтому любые вызовы для обновления пользовательского интерфейса должны быть явно запущены в потоке пользовательского интерфейса путем вызова, `InvokeOnMainThread` чтобы избежать завершения работы приложения iOS. 

## <a name="handling-transfer-completion"></a>Обработка завершения перемещения

Заключительным этапом является предоставление приложению сведений о завершении всех связанных с сеансом задач и обработке нового содержимого.

В `AppDelegate` Подпишитесь на `HandleEventsForBackgroundUrl` событие. Когда приложение переходит в фоновый режим и выполняется сеанс передачи, этот метод вызывается, и система передает нам обработчик завершения:

```csharp
public System.Action backgroundSessionCompletionHandler;

public void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Используйте обработчик завершения, чтобы сообщить iOS о завершении обработки приложения.

Вспомним, что сеанс может порождать несколько задач для обработки перемещения. По завершении выполнения последней задачи приостановленное или завершенное приложение повторно запускается в фоновом режиме. Затем приложение повторно подключается к, `NSURLSession` используя уникальный идентификатор сеанса, и вызывает `DidFinishEventsForBackgroundSession` делегат сеанса. Этот метод является возможностью приложения для работы с новым содержимым, включая обновление пользовательского интерфейса в соответствии с результатами перемещения:

```csharp
public void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

После завершения обработки нового содержимого вызовите обработчик завершения, чтобы система знала, что можно получить моментальный снимок приложения и вернуться в спящий режим:

```csharp
public void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

В этом пошаговом руководстве были рассмотрены основные шаги для реализации службы фоновой пересылки в iOS 7 и более поздних версиях.

## <a name="related-links"></a>Связанные ссылки

- [Простая фоновая перенаправление (пример)](/samples/xamarin/ios-samples/simplebackgroundtransfer)