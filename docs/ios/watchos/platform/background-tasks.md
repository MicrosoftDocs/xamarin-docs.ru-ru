---
title: Фоновые задачи watchOS в Xamarin
description: В этом документе описывается использование фоновых задач с watchOS в Xamarin, Просмотр типов фоновых задач, использование ресурсов, внедрение фоновых задач, планирование, рекомендации и многое другое.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/13/2017
ms.openlocfilehash: 84953ce2ec09cc757b5719991e499dc24b708cae
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996114"
---
# <a name="watchos-background-tasks-in-xamarin"></a>Фоновые задачи watchOS в Xamarin

В watchOS 3 есть три основных способа, с помощью которых приложение наблюдения может обеспечить актуальность данных:

- Использование одной из нескольких новых фоновых задач.
- Проследите за проявлением циферблата (получив дополнительные время на обновление).
- Пользователь закрепляет приложение в новом закрепления (где он хранится в памяти и часто обновляется).

## <a name="keeping-an-app-up-to-date"></a>Обновление приложения в актуальном состоянии

Прежде чем обсуждать все способы, которыми разработчик может оставлять данные и обновленный пользовательский интерфейс watchOS приложения, в этом разделе вы узнаете о типичном наборе шаблонов использования, а также о том, как пользователь может перемещаться между iPhone и их Apple Watch в течение дня в зависимости от времени суток и активности, выполняемой в данный момент (например, при движении).

Возьмем следующий пример:

[![Как пользователь может перемещаться между iPhone и их Apple Watch в течение дня](background-tasks-images/update00.png)](background-tasks-images/update00.png#lightbox)

1. В утром, в то время как в процессе ожидания кофе пользователь просматривает текущие новости на iPhone в течение нескольких минут.
2. Прежде чем покинуть кафе, они быстро проверяют погоду, усложняя свой просмотр.
3. Перед обедом они используют приложение Maps на iPhone, чтобы найти ближайший ресторан и зарегистрировать резервирование в соответствии с клиентом.
4. При командировке в ресторан они получают уведомление о своих Apple Watch и с кратким представлением о том, что их встречи работают поздно.
5. В вечером они используют приложение Maps на iPhone для проверки трафика перед назначением домой.
6. Дома они получают уведомление iMessage на своих Apple Watch с просьбой выбрать несколько молоко, и они используют функцию быстрого ответа для отправки ответа "ОК".

Из-за "краткого обзора" (менее трех секунд) того, как пользователь хочет использовать Apple Watch приложение, обычно недостаточно времени для получения необходимой информации и обновления пользовательского интерфейса приложения перед его отображением пользователю.

Используя новые API Apple, входящие в watchOS 3, приложение может запланировать _фоновое обновление_ и подготовить нужные сведения перед запросом пользователя. Возьмем пример усложнения погоды, описанный выше:

[![Пример усложнения погоды](background-tasks-images/update01.png)](background-tasks-images/update01.png#lightbox)

1. Приложение планируется пробуждении системой в определенное время.
2. Приложение извлекает информацию, которая потребуется для создания обновления.
3. Приложение повторно создает пользовательский интерфейс для отражения новых данных.
4. Когда пользователь просматривает сложность приложения, он имеет актуальную информацию, и пользователь не должен ждать обновления.

Как показано выше, система watchOS пробуждает приложение с помощью одной или нескольких задач, для которых доступен очень ограниченный пул:

[![Система watchOS пробуждает приложение с помощью одной или нескольких задач.](background-tasks-images/update02.png)](background-tasks-images/update02.png#lightbox)

Компания Apple рекомендует сделать большую часть этой задачи (так как это ограниченный ресурс для приложения), добавив его до тех пор, пока приложение не завершит процесс обновления.

Система доставляет эти задачи, вызывая новый `HandleBackgroundTasks` метод `WKExtensionDelegate` делегата. Например.

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
  public class ExtensionDelegate : WKExtensionDelegate
  {
    #region Constructors
    public ExtensionDelegate ()
    {
    }
    #endregion

    #region Override Methods
    public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
    {
      // Handle background request here
      ...
    }
    #endregion
  }
}
```

Когда приложение завершит данную задачу, она вернет ее в систему, пометив ее как завершенную:

[![Задача возвращается в систему, помечая ее как завершенную](background-tasks-images/update03.png)](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks"></a>

## <a name="new-background-tasks"></a>Новые фоновые задачи

в watchOS 3 представлено несколько фоновых задач, которые приложение может использовать для обновления сведений, гарантирующих, что он должен иметь содержимое, необходимое пользователю, прежде чем открыть приложение, например:

- **Фоновое обновление приложения** . Задача [вкаппликатионрефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) позволяет приложению обновить свое состояние в фоновом режиме. Обычно это будет включать в себя другую задачу, например загрузку нового содержимого из Интернета с помощью [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Фоновое обновление моментального снимка** . Задача [вкснапшотрефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) позволяет приложению обновлять содержимое и пользовательский интерфейс до того, как система создаст моментальный снимок, который будет использоваться для заполнения закрепления.
- **Подключение к фоновому просмотру** . Задача [вкватчконнективитирефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) запускается для приложения при получении фоновых данных из парного iPhone.
- **Фоновый сеанс URL-адреса** — задача [вкурлсессионрефрешбаккграундтаск](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) запускается для приложения, когда фоновому переносу требуется авторизация или завершение (успешно или по ошибке).

Эти задачи будут подробно рассмотрены в следующих разделах.

<a name="WKApplicationRefreshBackgroundTask"></a>

### <a name="wkapplicationrefreshbackgroundtask"></a>вкаппликатионрефрешбаккграундтаск

`WKApplicationRefreshBackgroundTask`— Это универсальная задача, которую можно запланировать на пробуждении приложения в будущем:

[![Вкаппликатионрефрешбаккграундтаск пробуждении в будущем](background-tasks-images/update04.png)](background-tasks-images/update04.png#lightbox)

В среде выполнения задачи приложение может выполнять любые локальные операции обработки, такие как обновление временной шкалы усложнения или выбор некоторых необходимых данных с помощью `NSUrlSession` .

<a name="WKURLSessionRefreshBackgroundTask"></a>

### <a name="wkurlsessionrefreshbackgroundtask"></a>вкурлсессионрефрешбаккграундтаск

Система отправит сообщение `WKURLSessionRefreshBackgroundTask` после того, как данные будут загружены и готовы к обработке приложением:

[![Вкурлсессионрефрешбаккграундтаск после завершения загрузки данных](background-tasks-images/update05.png)](background-tasks-images/update05.png#lightbox)

Приложение не осталось работать, пока данные скачиваются в фоновом режиме. Вместо этого приложение планирует запрос данных, затем приостанавливается и система обрабатывает загрузку данных, выполняя только после завершения загрузки приложение.

<a name="WKSnapshotRefreshBackgroundTask"></a>

### <a name="wksnapshotrefreshbackgroundtask"></a>вкснапшотрефрешбаккграундтаск

В watchOS 3 Компания Apple добавила закрепление, где пользователи могут закреплять свои любимые приложения и быстро получать к ним доступ. Когда пользователь нажимает кнопку сбоку на Apple Watch, отображается коллекция снимков закрепленных приложений. Пользователь может прокрутить влево или вправо, чтобы найти нужное приложение, а затем коснуться приложения, чтобы запустить его, заменив моментальный снимок на интерфейс выполняющегося приложения.

[![Замена моментального снимка интерфейсом выполняющихся приложений](background-tasks-images/update06.png)](background-tasks-images/update06.png#lightbox)

Система периодически получает снимки пользовательского интерфейса приложения (путем отправки `WKSnapshotRefreshBackgroundTask` ) и использует эти моментальные снимки для заполнения закрепления. watchOS предоставляет приложению возможность обновить содержимое и пользовательский интерфейс перед созданием моментального снимка.

Моментальные снимки очень важны в watchOS 3, так как они работают как образы для предварительного просмотра и запуска приложения. Если пользователь выполняет сопоставление в приложении в закрепления, он разворачивается в полноэкранный режим, выполняется переход на передний план и начинается выполнение, поэтому крайне важно, чтобы моментальный снимок был обновлен:

[![Если пользователь сопоставлен с приложением в закрепления, он будет расширен до полноэкранного режима.](background-tasks-images/update07.png)](background-tasks-images/update07.png#lightbox)

Опять же, система выдаст a, `WKSnapshotRefreshBackgroundTask` чтобы приложение может подготовиться (путем обновления данных и пользовательского интерфейса) до создания моментального снимка:

[![Приложение может подготовиться путем обновления данных и пользовательского интерфейса до создания моментального снимка](background-tasks-images/update08.png)](background-tasks-images/update08.png#lightbox)

Когда приложение помечает `WKSnapshotRefreshBackgroundTask` завершенное, система автоматически создаст моментальный снимок пользовательского интерфейса приложения.

> [!IMPORTANT]
> Важно всегда планировать `WKSnapshotRefreshBackgroundTask` после того, как приложение получит новые данные и обновило пользовательский интерфейс, или пользователь не увидит измененную информацию.

Кроме того, когда пользователь получает уведомление от приложения и использует его для переноса приложения на передний план, моментальный снимок должен быть обновлен так же, как и экран запуска.

[![Пользователь получает уведомление от приложения и выполняет его касание для переноса приложения на передний план.](background-tasks-images/update09.png)](background-tasks-images/update09.png#lightbox)

Если с момента взаимодействия пользователя с приложением watchOS прошло более одного часа, он сможет вернуться к состоянию по умолчанию. Состояние по умолчанию может означать различные вещи для различных приложений и, в зависимости от структуры приложения, может вообще не иметь состояния по умолчанию.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask"></a>

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>вкватчконнективитирефрешбаккграундтаск

В watchOS 3 Компания Apple имеет интегрированное наблюдение за подключением к API фонового обновления через новый `WKWatchConnectivityRefreshBackgroundTask` . С помощью этой новой функции приложение iPhone может предоставлять новые данные на свой аналог приложения для просмотра, пока приложение watchOS выполняется в фоновом режиме:

[![Приложение iPhone может доставлять новые данные в свой аналог приложения для просмотра, пока приложение watchOS выполняется в фоновом режиме.](background-tasks-images/update10.png)](background-tasks-images/update10.png#lightbox)

Запуск принудительной отправки, контекст приложения, Отправка файла или обновление сведений о пользователе из приложения iPhone приведет к пробуждать Apple Watch приложение в фоновом режиме.

Когда приложение Watch пробуждении через, `WKWatchConnectivityRefreshBackgroundTask` ему потребуется использовать стандартные методы API для получения данных из приложения iPhone.

[![Поток данных Вкватчконнективитирефрешбаккграундтаск](background-tasks-images/update11.png)](background-tasks-images/update11.png#lightbox)

1. Убедитесь, что сеанс активирован.
2. Отслеживайте новое `HasContentPending` свойство, если оно имеет значение `true` , приложение по-прежнему содержит данные для обработки. Как и ранее, приложение должно удерживать задачу до завершения обработки всех данных.
3. Если больше нет данных для обработки ( `HasContentPending = false` ), пометьте задачу как выполненную, чтобы вернуть ее в систему. Невыполнение этого действия приведет к исчерпанию выделенной фоновой среды выполнения приложения, в результате чего будет получен отчет о сбое.

<a name="The-Background-API-Lifecycle"></a>

## <a name="the-background-api-lifecycle"></a>Фоновый жизненный цикл API

Поместив все части нового API фоновых задач вместе, типичный набор взаимодействий будет выглядеть следующим образом:

[![Фоновый жизненный цикл API](background-tasks-images/update12.png)](background-tasks-images/update12.png#lightbox)

1. Во-первых, приложение watchOS планирует, чтобы фоновая задача вернулся к активности в будущем.
2. Приложение пробуждении системой и отправило задачу.
3. Приложение обрабатывает задачу для выполнения любой необходимой работы.
4. В результате обработки задачи приложению может потребоваться запланировать больше фоновых задач, чтобы выполнить дополнительную работу в будущем, например загрузить дополнительное содержимое с помощью `NSUrlSession` .
5. Приложение помечает задачу как завершенную и возвращает ее в систему.

<a name="Using-Resources-Responsibly"></a>

## <a name="using-resources-responsibly"></a>Использование ресурсов в качестве ответственного

Очень важно, чтобы приложение watchOS в рамках этой экосистемы было очень важным, ограничивая его сток на общих ресурсах системы.

Рассмотрим следующий сценарий:

[![WatchOS приложение ограничивает свой сток в общих ресурсах системы.](background-tasks-images/update13.png)](background-tasks-images/update13.png#lightbox)

1. Пользователь запускает приложение watchOS в 1:00 PM.
2. Приложение планирует задачу для пробуждения и загрузки нового содержимого за час в 2:00 РМ.
3. В 1:50. Пользователь повторно открывает приложение, которое позволяет обновлять свои данные и пользовательский интерфейс в данный момент.
4. Вместо того, чтобы позволить задаче снова выйти из приложения в течение 10 минут, приложение должно повторно запланировать выполнение задачи спустя час в 2:50 РМ.

Хотя каждое приложение отличается, Apple предлагает найти закономерности использования, как показано выше, чтобы помочь экономить системные ресурсы.

<a name="Implementing-Background-Tasks"></a>

## <a name="implementing-background-tasks"></a>Реализация фоновых задач

Например, в этом документе будет использоваться фиктивное приложение Монкэйсокцер для спорта, которое сообщает пользователю о прохождении футболки.

Рассмотрим следующий типичный сценарий использования:

[![Типичный сценарий использования](background-tasks-images/update14.png)](background-tasks-images/update14.png#lightbox)

Любимая футбольная команда пользователя воспроизводит большое совпадение от 7:00 до 9:00 PM, поэтому приложение должно рассчитывать на то, что пользователь должен регулярно проверять баллы и принимать решение об интервале обновления в 30 минут.

1. Пользователь открывает приложение и планирует задачу для фонового обновления через 30 минут позже. Фоновый API позволяет выполнять только один тип фоновой задачи в определенный момент времени.
2. Приложение получает задачу и обновляет свои данные и пользовательский интерфейс, а затем планирует выполнение другой фоновой задачи через 30 минут позже. Важно, чтобы разработчик находился в планировании другой фоновой задачи, или приложение никогда не будет повторно пробудится для получения дополнительных обновлений.
3. Опять же, приложение получает задачу и обновляет ее данные, обновляет пользовательский интерфейс и планирует выполнение другой фоновой задачи через 30 минут позже.
4. Один и тот же процесс повторяется снова.
5. Получается последняя фоновая задача, и приложение снова обновляет свои данные и пользовательский интерфейс. Так как это окончательная оценка, она не запланирует новое фоновое обновление.

<a name="Scheduling-for-Background-Update"></a>

## <a name="scheduling-for-background-update"></a>Планирование фонового обновления

Учитывая приведенный выше сценарий, приложение Монкэйсокцер может использовать следующий код для планирования фонового обновления:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
  // Create a fire date 30 minutes into the future
  var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

  // Create
  var userInfo = new NSMutableDictionary ();
  userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
  userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

  // Schedule for update
  WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
    // Was the Task successfully scheduled?
    if (error == null) {
      // Yes, handle if needed
    } else {
      // No, report error
    }
  });
}
```

Он создает `NSDate` 30 минут в будущем, когда приложение хочет быть пробудится, и создает объект `NSMutableDictionary` для хранения сведений о запрошенной задаче. `ScheduleBackgroundRefresh`Метод класса `SharedExtension` используется для запроса задачи в расписании.

Система возвратит, `NSError` если не удалось запланировать запрошенную задачу.

<a name="Processing-the-Update"></a>

## <a name="processing-the-update"></a>Обработка обновления

Далее подробнее рассмотрим 5-минутное окно, в котором показаны шаги, необходимые для обновления оценки.

[![Окно в 5 минут, в котором показаны шаги, необходимые для обновления оценки](background-tasks-images/update15.png)](background-tasks-images/update15.png#lightbox)

1. На 7:30:02 PM приложение выдается системой и получает фоновую задачу обновления. Его первым приоритетом является получение последних оценок с сервера. См. раздел [планирование NSUrlSession](#Scheduling-a-NSUrlSession) ниже.
2. В 7:30:05 приложение завершает исходную задачу, система переводит приложение в спящий режим и загружает запрошенные данные в фоновом режиме.
3. Когда система завершит загрузку, она создаст новую задачу для пробуждения приложения, чтобы она могла обработать загруженные данные. См. раздел [Обработка фоновых задач](#Handling-Background-Tasks) и [Обработка загрузки, выполненной](#Handling-the-Download-Completing) ниже.
4. Приложение сохраняет обновленные сведения и помечает задачу как завершенную. В настоящее время разработчик может обновить пользовательский интерфейс приложения, однако Apple предлагает планировать задачу создания моментального снимка для обработки этого процесса. См. раздел [Планирование обновления моментального снимка](#Scheduling-a-Snapshot-Update) ниже.
5. Приложение получает задачу моментального снимка, обновляет пользовательский интерфейс и помечает задачу как завершенную. См. раздел [обработка обновления моментального снимка](#Handling-a-Snapshot-Update) ниже.

<a name="Scheduling-a-NSUrlSession"></a>

## <a name="scheduling-a-nsurlsession"></a>Планирование NSUrlSession

Чтобы запланировать загрузку последних оценок, можно использовать следующий код:

```csharp
private void ScheduleURLUpdateSession ()
{
  // Create new configuration
  var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

  // Create new session
  var backgroundSession = NSUrlSession.FromConfiguration (configuration);

  // Create and start download task
  var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
  downloadTask.Resume ();
}
```

Он настраивает и создает новый `NSUrlSession` , а затем использует этот сеанс для создания новой задачи скачивания с помощью `CreateDownloadTask` метода. Он вызывает `Resume` метод задачи скачивания для запуска сеанса.

<a name="Handling-Background-Tasks"></a>

## <a name="handling-background-tasks"></a>Обработка фоновых задач

Переопределяя `HandleBackgroundTasks` метод `WKExtensionDelegate` , приложение может обрабатывать входящие фоновые задачи:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
  public class ExtensionDelegate : WKExtensionDelegate
  {
    #region Computed Properties
    public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
    #endregion

    ...

    #region Public Methods
    public void CompleteTask (WKRefreshBackgroundTask task)
    {
      // Mark the task completed and remove from the collection
      task.SetTaskCompleted ();
      PendingTasks.Remove (task);
    }
    #endregion

    #region Override Methods
    public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
    {
      // Handle background request
      foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Is this a background session task?
        var urlTask = task as WKUrlSessionRefreshBackgroundTask;
        if (urlTask != null) {
          // Create new configuration
          var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

          // Create new session
          var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

          // Keep track of all pending tasks
          PendingTasks.Add (task);
        } else {
          // Ensure that all tasks are completed
          task.SetTaskCompleted ();
        }
      }
    }
    #endregion

    ...
  }
}
```

`HandleBackgroundTasks`Метод циклически проходит по всем задачам, которые система отправила приложению (в `backgroundTasks` ) поиск `WKUrlSessionRefreshBackgroundTask` . Если он найден, он повторно присоединяется к сеансу и присоединяется `NSUrlSessionDownloadDelegate` для завершения загрузки (см. раздел [Обработка загрузки, завершенной](#Handling-the-Download-Completing) ниже).

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Он сохраняет обработчик для задачи до ее завершения, добавляя его в коллекцию:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Необходимо завершить все задачи, отправленные в приложение, для любой задачи, которая сейчас не обрабатывается, пометить как завершенную:

```csharp
if (urlTask != null) {
  ...
} else {
  // Ensure that all tasks are completed
  task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing"></a>

## <a name="handling-the-download-completing"></a>Обработка завершения загрузки

Приложение Монкэйсокцер использует следующий `NSUrlSessionDownloadDelegate` делегат для обработки завершения загрузки и обработки запрошенных данных:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
  public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
  {
    #region Computed Properties
    public ExtensionDelegate WatchExtensionDelegate { get; set; }

    public WKRefreshBackgroundTask Task { get; set; }
    #endregion

    #region Constructors
    public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
    {
      // Initialize
      this.WatchExtensionDelegate = extensionDelegate;
      this.Task = task;
    }
    #endregion

    #region Override Methods
    public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
    {
      // Handle the downloaded data
      ...

      // Mark the task completed
      WatchExtensionDelegate.CompleteTask (Task);

    }
    #endregion
  }
}
```

При инициализации он сохраняет маркер как для, так и для `ExtensionDelegate` `WKRefreshBackgroundTask` порожденного. Он переопределяет `DidFinishDownloading` метод для завершения загрузки. Затем использует `CompleteTask` метод объекта, `ExtensionDelegate` чтобы уведомить о завершении задачи и удалить ее из коллекции ожидающих задач. См. раздел [Обработка фоновых задач](#Handling-Background-Tasks) выше.

<a name="Scheduling-a-Snapshot-Update"></a>

## <a name="scheduling-a-snapshot-update"></a>Планирование обновления моментального снимка

Следующий код можно использовать для планирования задачи создания моментального снимка с целью обновления пользовательского интерфейса с учетом последних оценок:

```csharp
private void ScheduleSnapshotUpdate ()
{
  // Create a fire date of now
  var fireDate = NSDate.FromTimeIntervalSinceNow (0);

  // Create user info dictionary
  var userInfo = new NSMutableDictionary ();
  userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
  userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

  // Schedule for update
  WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
    // Was the Task successfully scheduled?
    if (error == null) {
      // Yes, handle if needed
    } else {
      // No, report error
    }
  });
}
```

Как и `ScheduleURLUpdateSession` метод выше, он создает новый `NSDate` для, когда приложение хочет быть пробудится, и создает объект `NSMutableDictionary` для хранения сведений о запрошенной задаче. `ScheduleSnapshotRefresh`Метод класса `SharedExtension` используется для запроса задачи в расписании.

Система возвратит, `NSError` если не удалось запланировать запрошенную задачу.

<a name="Handling-a-Snapshot-Update"></a>

## <a name="handling-a-snapshot-update"></a>Обработка обновления моментального снимка

Для обработки задачи «моментальный снимок» `HandleBackgroundTasks` метод (см. раздел [Обработка фоновых задач](#Handling-Background-Tasks) выше) изменяется так, чтобы выглядеть следующим образом:

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
  // Handle background request
  foreach (WKRefreshBackgroundTask task in backgroundTasks) {
    // Take action based on task type
    if (task is WKUrlSessionRefreshBackgroundTask) {
      var urlTask = task as WKUrlSessionRefreshBackgroundTask;

      // Create new configuration
      var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

      // Create new session
      var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

      // Keep track of all pending tasks
      PendingTasks.Add (task);
    } else if (task is WKSnapshotRefreshBackgroundTask) {
      var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

      // Update UI
      ...

      // Create a expiration date 30 minutes into the future
      var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

      // Create user info dictionary
      var userInfo = new NSMutableDictionary ();
      userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
      userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

      // Mark task complete
      snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
    } else {
      // Ensure that all tasks are completed
      task.SetTaskCompleted ();
    }
  }
}
```

Метод проверяет тип обрабатываемой задачи. Если это так, `WKSnapshotRefreshBackgroundTask` он получает доступ к задаче:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Метод обновляет пользовательский интерфейс, а затем создает, `NSDate` чтобы сообщить системе, когда моментальный снимок будет устаревшим. Он создает со сведениями `NSMutableDictionary` о пользователе для описания нового моментального снимка и помечает задачу snapshot, которая была выполнена с помощью этих сведений:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Кроме того, он сообщает задаче моментального снимка, что приложение не возвращается в состояние по умолчанию (в первом параметре). Приложения, не имеющие концепции состояния по умолчанию, должны всегда устанавливать для этого свойства значение `true` .

<a name="Working-Efficiently"></a>

## <a name="working-efficiently"></a>Эффективная работа

Как видно из приведенного выше примера в течение пяти минут, когда приложение Монкэйсокцер потребовало обновления своих оценок, эффективно работать и использовать новые фоновые задачи watchOS 3, приложение было активно всего 15 секунд:

[![Приложение было активно в течение 15 секунд](background-tasks-images/update16.png)](background-tasks-images/update16.png#lightbox)

Это снижает влияние приложения на доступное Apple Watch ресурсы и время работы батареи, а также позволяет приложению работать лучше с другими приложениями, работающими на часах.

<a name="How-Scheduling-Works"></a>

## <a name="how-scheduling-works"></a>Как работает планирование

Хотя приложение watchOS 3 находится на переднем плане, оно всегда планируется запускать и может выполнять любой тип обработки, например обновление данных или перерисовку пользовательского интерфейса. Когда приложение перемещается в фон, оно обычно приостанавливается системой, и все операции среды выполнения останавливаются.

Несмотря на то, что приложение находится в фоновом режиме, оно может быть нацелено на быстрое выполнение определенной задачи. Таким образом, в watchOS 2 система может временно перебудить фоновое приложение, чтобы выполнить такие действия, как обработка длинного уведомления или обновление сложности приложения. В watchOS 3 существует несколько новых способов запуска приложения в фоновом режиме.

Хотя приложение находится в фоновом режиме, система накладывает несколько ограничений на него:

- Выполнение любой конкретной задачи подается только через несколько секунд. Система учитывает не только время передачи, но и объем ресурсов ЦП, потребляемый приложением для получения этого предела.
- Все приложения, превышающие ограничения, будут уничтожены следующими кодами ошибок:
  - **ЦП** — 0xc51bad01
  - **Время** — 0xc51bad02
- Система будет накладывать различные ограничения в зависимости от типа фоновой задачи, которая запросила выполнение приложения. Например, `WKApplicationRefreshBackgroundTask` и `WKURLSessionRefreshBackgroundTask` задачи получают несколько более длинных сред выполнения по отношению к другим типам фоновых задач.

<a name="Complications-and-App-Updates"></a>

### <a name="complications-and-app-updates"></a>Сложности и обновления приложений

Помимо новых фоновых задач, добавленных компанией Apple в watchOS 3, сложности приложения watchOS могут повлиять на то, как и когда приложение получает фоновые обновления.

Сложности — это небольшие визуальные элементы, которые позволяют быстро получить полезную информацию. В зависимости от выбранного лица с контрольными данными пользователь может настроить просмотр с помощью одной или нескольких осложнений, которые могут быть предоставлены приложением-наблюдателем в watchOS 3.

Если пользователь включает одну из осложнений приложения на свой экран, он предоставляет приложению следующие обновленные преимущества:

- Это приводит к тому, что система сохраняет приложение в готовом к запуске состоянии, где оно пытается запустить приложение в фоновом режиме, сохраняет его в памяти и предоставляет его для обновления еще раз.
- Сложности гарантированно загружаются по крайней мере 50 обновлений push-уведомлений в день.

Разработчик всегда должен стремиться к созданию привлекательных осложнений для своих приложений, чтобы заставить пользователя добавлять их к наблюдателю по указанным выше причинам.

В watchOS 2 сложности были основным способом получения приложением среды выполнения в фоновом режиме. В watchOS 3 приложение с усложнением по-прежнему будет поддерживать получение нескольких обновлений в час, однако оно может использовать, `WKExtensions` чтобы запросить больше времени выполнения для обновления своих осложнений.

Рассмотрим следующий код, который используется для обновления сложности из подключенного приложения iPhone:

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

  // Get session and the number of remaining transfers
  var session = WCSession.DefaultSession;
  var transfers = session.RemainingComplicationUserInfoTransfers;

  // Create user info dictionary
  var iconattrs = new Dictionary<NSString, NSObject>
    {
      {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
      {new NSString ("reason"), new NSString ("UpdateScore")}
    };

  var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

  // Take action based on the number of transfers left
  if (transfers < 1) {
    // No transfers left, either attempt to send or inform
    // user of situation.
    ...
  } else if (transfers < 11) {
    // Running low on transfers, only send on important updates
    // else conserve for a significant change.
    ...
  } else {
    // Send data
    session.TransferCurrentComplicationUserInfo (userInfo);
  }
}
```

Он использует `RemainingComplicationUserInfoTransfers` свойство объекта, `WCSession` чтобы увидеть, сколько высокоприоритетных операций передачи приложения осталось в течение дня, а затем принимает действие на основе этого числа. Если приложение начинает работать с низким уровнем передачи, оно может удерживать незначительные обновления и отправлять сведения только при значительном изменении.

<a name="Scheduling-and-Dock"></a>

### <a name="scheduling-and-the-dock"></a>Планирование и закрепление

В watchOS 3 Компания Apple добавила закрепление, где пользователи могут закреплять свои любимые приложения и быстро получать к ним доступ. Когда пользователь нажимает кнопку сбоку на Apple Watch, отображается коллекция снимков закрепленных приложений. Пользователь может прокрутить влево или вправо, чтобы найти нужное приложение, а затем коснуться приложения, чтобы запустить его, заменив моментальный снимок на интерфейс выполняющегося приложения.

[![Закрепить](background-tasks-images/dock01.png)](background-tasks-images/dock01.png#lightbox)

Система периодически создает моментальные снимки пользовательского интерфейса приложения и использует эти моментальные снимки для заполнения документов. watchOS предоставляет приложению возможность обновить содержимое и пользовательский интерфейс перед созданием моментального снимка.

Приложения, которые были закреплены на закреплениях, могут рассчитывать на следующее:

- Они получат по крайней мере одно обновление в час. Сюда входят задача обновления приложения и задача моментального снимка.
- Бюджет обновления распределяется между всеми приложениями в закрепления. Таким образом, чем меньше приложений закреплено пользователем, тем больше потенциальных обновлений будет получать каждое приложение.
- Приложение будет храниться в памяти, поэтому приложение будет быстро возобновлено при выборе на закрепления.

Последнее приложение, которое выполнял пользователь, будет считаться последним _использованным_ приложением и будет занимать последний слот в закрепления. После этого пользователь может навсегда закрепить его на закрепления. Последнее использование будет рассматриваться как любое другое любимое приложение, которое пользователь уже заблокировал на закрепление.

> [!IMPORTANT]
> Приложения, добавленные на начальном экране, не будут получать никаких стандартных расписаний. Чтобы получать регулярное планирование и фоновые обновления, приложение _должно_ быть Добавлено на закрепление.

Как упоминалось ранее в этом документе, моментальные снимки очень важны в watchOS 3, так как они работают как образы для предварительного просмотра и запуска приложения. Если пользователь выполняет сопоставление в приложении в закрепления, он разворачивается в полноэкранный режим, выполняется переход на передний план и начинается выполнение, поэтому крайне важно, чтобы моментальный снимок был обновлен.

Возможны случаи, когда система решает ему потребовать свежего снимка пользовательского интерфейса приложения. В этом случае запрос моментального снимка не будет учитываться в бюджете среды выполнения приложения. Следующий запрос запустит системный моментальный снимок:

- Обновление временной шкалы усложняется.
- Взаимодействие пользователя с уведомлением приложения.
- Переключение с переднего плана на фоновое состояние.
- Через один час в фоновом состоянии приложение может вернуться к состоянию по умолчанию.
- При первой загрузке watchOS.

<a name="Best-Practices"></a>

## <a name="best-practices"></a>Рекомендации

При работе с фоновыми задачами Компания Apple предлагает следующие рекомендации:

- Запланируйте расписание так часто, как это необходимо для обновления приложения. Каждый раз, когда приложение запускается, необходимо повторно оценить его будущие потребности и при необходимости настроить это расписание.
- Если система отправляет фоновую задачу обновления, а приложению не требуется обновление, отложите работу, пока не будет действительно требуется обновление.
- Рассмотрим все возможности среды выполнения, доступные для приложения:
  - Активация закрепления и переднего плана.
  - Уведомления.
  - Обновления усложняется.
  - Фоновое обновление.
- Используйте `ScheduleBackgroundRefresh` для фоновой среды выполнения общего назначения, например:
  - Опрос системы для получения сведений.
  - Запланируйте будущее `NSURLSessions` для запроса данных в фоновом режиме.
  - Известные переходы по времени.
  - Активация обновлений усложнения.

<a name="Snapshot-Best-Practices"></a>

## <a name="snapshot-best-practices"></a>Рекомендации по созданию моментальных снимков

При работе с обновлениями моментальных снимков Apple предоставляет следующие рекомендации:

- Делать моментальные снимки недействительными, например при наличии значительных изменений содержимого.
- Избегайте недействительности моментальных снимков с высокой частотой. Например, приложение таймера не должно обновлять моментальный снимок каждую секунду, оно должно выполняться только после завершения таймера.

<a name="App-Data-Flow"></a>

## <a name="app-data-flow"></a>Поток данных приложения

Для работы с потоком данных Apple рекомендует следующее:

[![Схема потока данных приложения](background-tasks-images/update17.png)](background-tasks-images/update17.png#lightbox)

Внешнее событие (например, просмотр подключения) выводит приложение из спящего режима. Это заставляет приложение обновить свою модель данных (которая представляет текущее состояние приложений). В результате изменения модели данных приложению потребуется обновить свои сложности, запросить новый моментальный снимок, возможно, запустить фоновый режим `NSURLSession` для получения дополнительных данных и планирования дальнейшего обновления в фоновом режиме.

<a name="The-App-Lifecycle"></a>

## <a name="the-app-lifecycle"></a>Жизненный цикл приложения

Благодаря закреплениям и возможности закреплять в ней избранные приложения компания Apple считает, что пользователи будут переноситься между большим числом приложений, гораздо чаще, а затем — с watchOS 2. В результате приложение должно быть готово к обработке этого изменения и быстро перемещаться между передним и фоновым состояниями.

Компания Apple имеет следующие рекомендации.

- Убедитесь, что приложение заканчивает фоновую задачу как можно скорее при переходе на передний план активации.
- Обязательно завершите все фоновые работы, прежде чем вводить фон, вызвав `NSProcessInfo.PerformExpiringActivity` .
- При тестировании приложения в симуляторе watchOS ни один из бюджетов задач не будет применен, поэтому приложение может обновляться так, как необходимо для правильного тестирования функции.
- Всегда проверяйте реальное Apple Watch оборудование, чтобы убедиться, что приложение не работает за прерядом бюджетов перед публикацией в iTunes Connect.
- Компания Apple предлагает сохранить Apple Watch на зарядном устройство во время тестирования и отладки.
- Убедитесь в тщательном тестировании как холодного запуска, так и возобновления работы приложения.
- Убедитесь, что все задачи приложения завершены.
- Изменяйте количество приложений, закрепленных в закреплениях, для тестирования наиболее подходящих и худших сценариев.

<a name="Summary"></a>

## <a name="summary"></a>Сводка

В этой статье были рассмотрены улучшения, внесенные компанией Apple в watchOS, а также способы их использования для обновления приложения для просмотра. Во первых, он охватывает все новые фоновые задачи Apple, добавленные в watchOS 3. Затем он охватывает фоновый жизненный цикл API и способ реализации фоновых задач в приложении Xamarin watchOS. Наконец, мы рассмотрели, как работает планирование, и предоставили некоторые рекомендации.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
